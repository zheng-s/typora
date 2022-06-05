java的线程是通过java.lang.Thread类来实现的。VM启动时会有一个由主方法所定义的线程。可以通过创建Thread的实例来创建新的线程。每个线程都是通过某个特定Thread对象所对应的方法run（）来完成其操作的，方法run()称为线程体。通过调用Thread类的start()方法来启动一个线程。

在Java当中，线程通常都有五种状态，创建、就绪、运行、阻塞和死亡。
　　第一是创建状态。在生成线程对象，并没有调用该对象的start方法，这是线程处于创建状态。
　　第二是就绪状态。当调用了线程对象的start方法之后，该线程就进入了就绪状态，但是此时线程调度程序还没有把该线程设置为当前线程，此时处于就绪状态。在线程运行之后，从等待或者睡眠中回来之后，也会处于就绪状态。
　　第三是运行状态。线程调度程序将处于就绪状态的线程设置为当前线程，此时线程就进入了运行状态，开始运行run函数当中的代码。
　　第四是阻塞状态。线程正在运行的时候，被暂停，通常是为了等待某个时间的发生(比如说某项资源就绪)之后再继续运行。sleep,suspend，wait等方法都可以导致线程阻塞。
　　第五是死亡状态。如果一个线程的run方法执行结束或者调用stop方法后，该线程就会死亡。对于已经死亡的线程，无法再使用start方法令其进入就绪。

实现并启动线程有两种方法
1、写一个类继承自Thread类，重写run方法。用start方法启动线程
2、写一个类实现Runnable接口，实现run方法。用new Thread(Runnable target).start()方法来启动

多线程原理：相当于玩游戏机，只有一个游戏机（cpu），可是有很多人要玩，于是，start是排队！等CPU选中你就是轮到你，你就run（），当CPU的运行的时间片执行完，这个线程就继续排队，等待下一次的run（）。

调用start（）后，线程会被 放到等待队列，等待CPU调度， 并不一定要马上开始执行，只是将这个线程置于可动行状态。然后通过JVM，线程Thread会调用run（）方法，执行本线程的线程体。**先调用start后调用run，这么麻烦，为了不直接调用run？就是为了实现多线程的优点，没这个start不行。**

1. start（）方法来启动线程，真正实现了多线程运行。**这时无需等待run方法体代码执行完毕**，可以直接继续执行下面的代码；通过调用Thread类的start()方法来启动一个线程， 这时此线程是处于就绪状态， 并没有运行。 然后通过此Thread类调用方法run()来完成其运行操作的， 这里方法run()称为线程体，它包含了要执行的这个线程的内容， Run方法运行结束， 此线程终止。然后CPU再调度其它线程。
2. run（）方法当作普通方法的方式调用。程序还是要顺序执行，要等待run方法体执行完毕后，才可继续执行下面的代码； 程序中只有主线程——这一个线程， 其程序执行路径还是只有一条， 这样就没有达到写线程的目的。
记住：多线程就是分时利用CPU，宏观上让所有线程一起执行 ，也叫并发

```java
public class Test {
	public static void main(String[] args) {
		Runner1 runner1 = new Runner1();
		Runner2 runner2 = new Runner2();
//		Thread(Runnable target) 分配新的 Thread 对象。
		Thread thread1 = new Thread(runner1);
		Thread thread2 = new Thread(runner2);
//		thread1.start();
//		thread2.start();
		thread1.run();
		thread2.run();
	}
}
 
class Runner1 implements Runnable { // 实现了Runnable接口，jdk就知道这个类是一个线程
	public void run() {
		for (int i = 0; i < 100; i++) {
			System.out.println("进入Runner1运行状态——————————" + i);
		}
	}
}
 
class Runner2 implements Runnable { // 实现了Runnable接口，jdk就知道这个类是一个线程
	public void run() {
		for (int i = 0; i < 100; i++) {
			System.out.println("进入Runner2运行状态==========" + i);
		}
	}
}
```

