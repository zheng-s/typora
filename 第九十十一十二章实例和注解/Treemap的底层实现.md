TreeMap是红黑二叉树的典型实现

root用来存储整个树的根节点。我们继续跟踪Entry(是TreeMap的内部类)的代码：

![图9-23 Entry底层源码.png](../../../blog/zheng-s/source/image/1495695046855639.png)

图9-23 Entry底层源码

   可以看到里面存储了本身数据、左节点、右节点、父节点、以及节点颜色。 TreeMap的put()/remove()方法大量使用了红黑树的理论。