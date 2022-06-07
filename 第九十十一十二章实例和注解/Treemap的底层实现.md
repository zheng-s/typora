TreeMap是红黑二叉树的典型实现

root用来存储整个树的根节点。我们继续跟踪Entry(是TreeMap的内部类)的代码：

![image](https://user-images.githubusercontent.com/65000660/172333574-a5c8b0b7-c69f-4405-9fdb-90c350816788.png)


图9-23 Entry底层源码

   可以看到里面存储了本身数据、左节点、右节点、父节点、以及节点颜色。 TreeMap的put()/remove()方法大量使用了红黑树的理论。
