[toc]
# java 并发
- **进程**：是指一个内存中运行的应用程序，每个进程都有一个独立的内存空间，一个应用程序可以同时运行多个进程；进程也是程序的一次执行过程，是系统运行程序的基本单位；系统运行一个程序即是一个进程从创建、运行到消亡的过程。
- **线程**：线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中是可以有多个线程的，这个应用程序也可以称之为多线程程序。 


- **多线程**：从软件或者硬件上实现多个线程并发执行的技术。在单个程序中同时运行多个线程完成不同的工作。


- **并发**：指两个或多个事件再同一时间段内发生 <br >
- **并行**：指两个或多个事件再同一时刻发生 <br >
![Thread-并行与并发.png](./img/java/Thread-并行与并发.png)


## 生命周期
- **New(新建)** <br >
创建后未启动，可以通过调用 Start 方法使用

- **Runnable（可运行）** <br >
创建后再运行，可能是是等待时间片轮调度（CPU调度）

- **Blocked（阻塞态）** <br >
等待获取锁的事件，如果有共用线程的锁释放，就可以解除阻塞态
- **Runing（运行态）** <br >
经过调度正在运行的线程
- **Dead(死亡态)** <br>
进程结束运行，自行结束，或者遇到了异常结束
![Thread-生命周期.png](./img/java/Thread-生命周期.png)


## 线程使用
- 继承 Thread 类
- 实现 Runnable 接口 

### 继承 Thread 类使用线程
需要 run 方法，如果不重写 run 方法默认使用 Thread 类的 run 方法
```java
public class MyThread extends Thread{
    @Override
    public void run() {
        ///
    }
}
```

```java
public static void main(String[] args){
    MyThread mt = new MyThread();
    mt.start();
}
```

### 使用接口实现 Runnable 类
接口实现的类不属于真正意义上的线程，需要使用 Thread 调用

```java
public class MyRunnable implements Runnable{
    @Override
    public void run() {
        ///
    }
}
```

```java
public static void main(String[] args){
    MyRunnable myrun = new MyRunnable();
    new Thread(myrun).start();
}
```

### 接口实现和类实现的区别
实现接口 Runnable 接口创建多线程的好处  <br >
        1、避免了单继承的局限性 <br >
            一个类只能继承一个类，类继承了 Thread 就不能继承其他类，能够实现其它接口 <br >
        2、增加程序的扩展性，降低耦合性<br >
            实现 Runnbable 接口的方式，把设置的线程任务和开启任务j进行分离（解耦）<br >
            实现类中重写了 run 方法，可以用线程设置任务<br >
            创建 Thread 对象调用start方法开启新的线程任务<br >


## 解决线程安全
- synchronized 同步代码块
- synchroized 同步方法
- Lock 锁

### 同步代码块


## 记时等待（TimeWaiting）的两种方式 <br>
          1、使用 sleep（long m）方法，毫秒值结束后，线程睡醒进入到 Runnable / Blocked 状态<br>
          2、使用 wait（long m）方法，毫秒值结束后如果，进程没有被notify方法唤醒，就会自动醒来，线程醒来后进入到 Runnable / Blocked 状态
