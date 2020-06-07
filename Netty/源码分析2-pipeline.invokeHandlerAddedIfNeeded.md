#第2篇 pipeline.invokeHandlerAddedIfNeeded()
   
   不知道小伙伴 有没有看懂第1篇 哈哈。。至少应该看懂了注册吧，或者至少 感受到了开源项目的模块化啦吧。。。<br>
   好吧，这些都不重要，因为  今天要说的是 pipeline ！！ 又会带来多少新坑呢 ?! 不重要，先看下这个方法是怎么实现的吧！！！！
   ```
      //一路debug（没有具体操作，只做跳转功能的代码 没有贴出）
      private void callHandlerAddedForAllHandlers() {
             final PendingHandlerCallback pendingHandlerCallbackHead;
             synchronized (this) {
                 assert !registered;
                  
                 // This Channel itself was registered.
                 registered = true;
     
                 pendingHandlerCallbackHead = this.pendingHandlerCallbackHead;
                 // Null out so it can be GC'ed.
                 this.pendingHandlerCallbackHead = null;
             }
     
             // This must happen outside of the synchronized(...) block as otherwise handlerAdded(...) may be called while
             // holding the lock and so produce a deadlock if handlerAdded(...) will try to add another handler from outside
             // the EventLoop.
             PendingHandlerCallback task = pendingHandlerCallbackHead;
             while (task != null) {
                 task.execute();
                 task = task.next;
             }
         }
   ```
   这里必须要添加的前提是: 在Serverbootstrap 中初始化Channel，有对pipeline 进行addLast 操作，
   这里是addlast的实际操作，康康都干了啥？
   ```
  public final ChannelPipeline addLast(EventExecutorGroup group, String name, ChannelHandler handler) {
        final AbstractChannelHandlerContext newCtx;
        synchronized (this) {
            //检查 
            checkMultiplicity(handler);
            // 创建一个 context,这个Context可以是新坑，这个类的作用是invork Handler的方法，
            // 记录handler的状态，并且确保顺序性。理由还有一个mask 这个还对ThreadLocal 进行了增强
            newCtx = newContext(group, filterName(name, handler), handler);
            // 将其加入一个pipeline 中,pipeline 是一个双向链表，可以研究下这个方法。
            addLast0(newCtx);

            // If the registered is false it means that the channel was not registered on an eventLoop yet.
            // In this case we add the context to the pipeline and add a task that will call
            // ChannelHandler.handlerAdded(...) once the channel is registered.
            if (!registered) {
                //将newCtx 设置为Pengding
                newCtx.setAddPending();
               // 将这个CTX 加载到pipeline的 pendingHandlerCallbackHead 中
                callHandlerCallbackLater(newCtx, true);
                return this;
            }

            EventExecutor executor = newCtx.executor();
            if (!executor.inEventLoop()) {
               // 如果当前的线程与excutor的线程不匹配，则将该callHandlerAdded0(newCtx)加入到executor的task
                callHandlerAddedInEventLoop(newCtx, executor);
                return this;
            }
        }
        callHandlerAdded0(newCtx);
        return this;
    }
   ```
   其实 pipeline addLast 最重要的就是 进行callHandlerAdded0(newCtx)的方法，哈哈。。。这个pending
   的后续 也是进行这波操作，绕到这里，就康康 callHandlerAdded0这个方法
  ```
    //这个方法还是调用 context 的如下方法
    final void callHandlerAdded() throws Exception {
        // We must call setAddComplete before calling handlerAdded. Otherwise if the handlerAdded method generates
        // any pipeline events ctx.handler() will miss them because the state will not allow it.
        // 首先确定 handler 已经add，不然 后续 ctx 的handler 的一些pipeline events 会不会执行，因为 没有设置
        // 上面的翻译差不多就这样，看 ctx的还是要调用handler的操作，而这些handler的具体的方法是有用户自行定义的
        // 所有 ctx 就是 处理 handler的状态的，这些所有的操作 就是模块化，才会让netty的使用十分简洁。cool
        if (setAddComplete()) {
            handler().handlerAdded(this);
        }
    }
``` 
   好的，分析完了pipeline的addLast，现在需要分析下注册那波代码中的 pipeline.fireChannelRegistered() 
   ```   
         // 看一开始就把 head 头节点作为begin，开始fire
         public final ChannelPipeline fireChannelRegistered() {
                AbstractChannelHandlerContext.invokeChannelRegistered(head);
                return this;
            }
         // ctx 的静态方法。
         static void invokeChannelRegistered(final AbstractChannelHandlerContext next) {
                 EventExecutor executor = next.executor();
                 if (executor.inEventLoop()) {
                     // 还是要channelRegister，这一波要调用handler的channelRegister啦
                     next.invokeChannelRegistered();
                 } else {
                     executor.execute(new Runnable() {
                         @Override
                         public void run() {
                             next.invokeChannelRegistered();
                         }
                     });
                 }
             }
          // 看，即将调用handler的方法
          private void invokeChannelRegistered() {
                  if (invokeHandler()) {
                      try {
                          ((ChannelInboundHandler) handler()).channelRegistered(this);
                      } catch (Throwable t) {
                          invokeExceptionCaught(t);
                      }
                  } else {
                      fireChannelRegistered();
                  }
              }
           // handler 
            public void channelRegistered(ChannelHandlerContext ctx) {
                        //这个就是 做hanndlerAdd Add 之后 会跳过
                        invokeHandlerAddedIfNeeded();
                        // 看到了么？有开始fire ，这里的fire 我感觉可以翻译成 火炬，handler 就是火炬手，一个一个传 直到 不需要传了。
                        ctx.fireChannelRegistered();
                    }
            // 这里调用 ctx 的方法，看到 findContextInbound的方法了么，这个方法应该是要换火炬手啦！
            public ChannelHandlerContext fireChannelRegistered() {
                   invokeChannelRegistered(findContextInbound(MASK_CHANNEL_REGISTERED));
                   return this;
               }
            //那就康康这个方法啦
           private AbstractChannelHandlerContext findContextInbound(int mask) {
                  AbstractChannelHandlerContext ctx = this;
                  EventExecutor currentExecutor = executor();
                  do {
                      ctx = ctx.next;
                    //这个有一个skip 是允许跳过的哈，也就是说允许其它线程执行handler
                    //这里面的mask还挺有意思的，不过这次就不展开说啦。
                  } while (skipContext(ctx, currentExecutor, mask, MASK_ONLY_INBOUND));
                  return ctx;
              }
           //然后一个不断传递火炬，其实也没有火炬这个具体的东西哈，就意思一下
   ```
    