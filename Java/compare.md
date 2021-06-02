#Compare
我发现刷题 可能会上瘾 
ok
今天说说 Arrays.sort 中的compare

以两个 二元矩阵比较大小为例
Int[] nums=new Int[10];
Integer[] aa=new Integer[10];
Arrays.sort(nums);
Arrays.sort(aa,(x,y)->y-x);
int[][] cc=new int[10][10];
Arrays.sort(cc,(x,y)->y[0]-x[0]);
PriortyQueue<Task> queue=new PriortyQueue<>((t1,t1)->{
  if(t1.processingTime==t2.processingTime){
     return t1.id-t2.id;
  }else{
     return t1.processTime-t2.processTime;
  }
});
TreeSet<Integer> set=new TreeSet<Integer>();
Integer a=set.floor(q[i][0]);
Integer b=set.ceiling(q[i][0]);


== 表示两者引用 是否一致
equal 表示 两者值 是否相等

