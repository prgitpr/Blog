# Git

 references
* book<br> 
English Version [https://github.com/progit/progit2/tree/master/book](https://github.com/progit/progit2/tree/master/book)<br>
中文版 [https://www.progit.cn](https://www.progit.cn)<br>
* doc<br>
[https://git-scm.com/doc](https://git-scm.com/doc)

* git rebase<br>
  在pro git 中有提到 ，小心使用，这是一个Rebasing(中文翻译书称为 变基)操作，也就是改变之前的历史。
  最好是只改变（最好独立拥有）分支的base，保持master的base不变，不然，所有基于master的分支的base 都需要改变。
  这就是隐患。
  
  操作很简单： 
  ```
  git checkout ex
  git rebase master
  ```
  如果 继续执行
  ```
  git checkout master
  git merge ex
  ```
  会发现 ex commit 消失，长在了 原master的前头，现master 与 ex 的head 一致
  
* git merge --squash<br>
  squash 也是一波rebasing 操作
  ```
  git checkout -b ex master
  // 需要 squash 的分支 needmerge
  git merge --squash needmerge
  git commit
  // ex 分支的前头就长着（needmerge 的所有commit合为 一个）的commit，needmerge会消失
  ```
* git stash<br>
  就好比现在正在写文档，突然外卖到了，你就把文档保存下，把外卖吃完后，再打开文档。<br>
  有点区别是，外卖和文档不冲突，stash 有可能触发冲突，如果你处理的或者更新了stash的文档。
  ```
  git stash
  //一波操作后 
  git stash pop
  ```
* git blame<br>
  git blame -L
  在接收代码 可以 联系原作。或者 再出现问题bug时，看看谁动了奶酪。<br>
* git bisect
  这个在 debug 时，看看 哪里的奶酪动了。<br>
  
* git status
  查看状态 <br>

...to be  continued