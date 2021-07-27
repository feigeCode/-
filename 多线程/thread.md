# 多线程

参考资料：

[原文链接](https://thinkwon.blog.csdn.net/article/details/104863992)

[遇见狂神说](https://space.bilibili.com/95256449/)

# 1、概述

### 什么是多线程，多线程的优劣？

**百度百科**

多线程（multithreading），是指从软件或者硬件上实现多个线程并发执行的技术。具有多线程能力的计算机因有硬件支持而能够在同一时间执行多于一个线程，进而提升整体处理性能。具有这种能力的系统包括对称多处理机、多核心处理器以及芯片级多处理或同时多线程处理器。在一个[程序](https://baike.baidu.com/item/程序/71525)中，这些独立运行的程序片段叫作“[线程](https://baike.baidu.com/item/线程/103101)”（Thread），利用它编程的概念就叫作“多线程处理”



多线程：多线程是指程序中包含多个执行流，即在一个程序中可以同时运行多个不同的线程来执行不同的任务。

多线程的好处：

可以提高 CPU 的利用率。在多线程程序中，一个线程必须等待的时候，CPU 可以运行其它的线程而不是等待，这样就大大提高了程序的效率。也就是说允许单个程序创建多个并行执行的线程来完成各自的任务。

多线程的劣势：

- 线程也是程序，所以线程需要占用内存，线程越多占用内存也越多；
- 多线程需要协调和管理，所以需要 CPU 时间跟踪线程；
- 线程之间对共享资源的访问会相互影响，必须解决竞用共享资源的问题。

### 并行和并发有什么区别？

- 并发：多个任务在同一个 CPU 核上，按细分的时间片轮流(交替)执行，从逻辑上来看那些任务是同时执行。
- 并行：单位时间内，多个处理器或多核处理器同时处理多个任务，是真正意义上的“同时进行”。
- 串行：有n个任务，由一个线程按顺序执行。由于任务、方法都在一个线程执行所以不存在线程不安全情况，也就不存在临界区的问题。

做一个形象的比喻：

- 并发 = 两个队列和一台咖啡机。

- 并行 = 两个队列和两台咖啡机。

- 串行 = 一个队列和一台咖啡机。

### 线程和进程区别

#### 什么是线程和进程?

**进程**

一个在内存中运行的应用程序。每个进程都有自己独立的一块内存空间，一个进程可以有多个线程，比如在Windows系统中，一个运行的xx.exe就是一个进程。

**线程**

进程中的一个执行任务（控制单元），负责当前进程中程序的执行。一个进程至少有一个线程，一个进程可以运行多个线程，多个线程可共享数据。

**区别**

线程具有许多传统进程所具有的特征，故又称为轻型进程(Light—Weight Process)或进程元；而把传统的进程称为重型进程(Heavy—Weight Process)，它相当于只有一个线程的任务。在引入了线程的操作系统中，通常一个进程都有若干个线程，至少包含一个线程。

**根本区别**：进程是操作系统资源分配的基本单位，而线程是处理器任务调度和执行的基本单位

**资源开销**：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。

**包含关系**：如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。

**内存分配**：同一进程的线程共享本进程的地址空间和资源，而进程之间的地址空间和资源是相互独立的

**影响关系**：一个进程崩溃后，在保护模式下不会对其他进程产生影响，但是一个线程崩溃整个进程都死掉。所以多进程要比多线程健壮。

**执行过程**：每个独立的进程有程序运行的入口、顺序执行序列和程序出口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制，两者均可并发执行

### 什么是上下文切换?

多线程编程中一般线程的个数都大于 CPU 核心的个数，而一个 CPU 核心在任意时刻只能被一个线程使用，为了让这些线程都能得到有效执行，CPU 采取的策略是为每个线程分配时间片并轮转的形式。当一个线程的时间片用完的时候就会重新处于就绪状态让给其他线程使用，这个过程就属于一次上下文切换。

概括来说就是：当前任务在执行完 CPU 时间片切换到另一个任务之前会先保存自己的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。**任务从保存到再加载的过程就是一次上下文切换**。

上下文切换通常是计算密集型的。也就是说，它需要相当可观的处理器时间，在每秒几十上百次的切换中，每次切换都需要纳秒量级的时间。所以，上下文切换对系统来说意味着消耗大量的 CPU 时间，事实上，可能是操作系统中时间消耗最大的操作。

Linux 相比与其他操作系统（包括其他类 Unix 系统）有很多的优点，其中有一项就是，其上下文切换和模式切换的时间消耗非常少。



# 2、Java创建线程的四种方式

- 继承Thread类
- 实现Runnable接口
- 实现Callable接口
- 线程池

方式一：继承Thread类，重写run方法，调用start方法

~~~java
package com.feige.thread;

/**
 * 方式一：继承Thread类，重写run方法，调用start方法
 * 线程开启后不一定立即执行，有CPU调度执行
 */
public class ThreadTest extends Thread{
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            System.out.println("我在看代码:" + i);
        }
    }

    public static void main(String[] args) {
        ThreadTest threadTest = new ThreadTest();
        threadTest.start();
        for (int i = 0; i < 20; i++) {
            System.out.println("我在学习多线程:" + i);
        }
    }
}

~~~

方式二：实现Runnable接口，重写run方法，用Thread类来代理执行

~~~java
package com.feige.thread;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;

/**
 * 方式二：实现Runnable接口，重写run方法，用Thread类来代理执行
 */

public class ThreadTest2 implements Runnable {
    private String url;
    private String name;

    public ThreadTest2(String url, String name) {
        this.url = url;
        this.name = name;
    }

    public void run() {
        try {
            System.out.println("正在下载图片:" + name);
            FileUtils.copyURLToFile(new URL(this.url),new File(this.name));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

~~~

~~~java
package com.feige.thread;

public class ThreadMain {
    public static void main(String[] args) {
        ThreadTest2 threadTest1 = new ThreadTest2("https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMS8yNS8xNmVhMDQ3NjEyOTVlNzY2?x-oss-process=image/format,png", "1.png");
        ThreadTest2 threadTest2 = new ThreadTest2("https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMS8yNS8xNmVhMDQ3NjEyOTVlNzY2?x-oss-process=image/format,png", "2.png");
        ThreadTest2 threadTest3 = new ThreadTest2("https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMS8yNS8xNmVhMDQ3NjEyOTVlNzY2?x-oss-process=image/format,png", "3.png");
        new Thread(threadTest1).start();
        new Thread(threadTest2).start();
        new Thread(threadTest3).start();
    }
}

~~~

**龟兔赛跑**

~~~java
package com.feige.thread;

public class Race implements Runnable {
    private String winner;
    public void run() {
        for (int i = 1; i <= 100; i++) {
            if ("兔子".equals(Thread.currentThread().getName()) && i % 10 == 0){
                try {
                    Thread.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            if (gameOver(i)){
                break;
            }
            System.out.println(Thread.currentThread().getName() + "--> 跑了" + i +"步");
        }
    }
    public boolean gameOver(int steps){
        if (this.winner != null){
            return true;
        }else {
            if (steps >= 100){
                this.winner = Thread.currentThread().getName();
                System.out.println("winner is" + Thread.currentThread().getName());
                return true;
            }
        }
        return false;
    }
}

~~~

~~~java
package com.feige.thread;

public class ThreadMain3 {

    public static void main(String[] args) {
        Race race = new Race();
        new Thread(race,"兔子").start();
        new Thread(race,"乌龟").start();
    }
}

~~~



方式三：实现Callable<V>接口，也是通过Thread类代理，有返回值

~~~java
package com.feige.thread;

import java.util.concurrent.Callable;

/**
 * 方式三：实现Callable<V>接口，也是通过Thread类代理，有返回值
 */
public class ThreadTest4 implements Callable<String> {
    public String call() throws Exception {
        System.out.println("执行了-->" + Thread.currentThread().getName());
        return Thread.currentThread().getName();
    }
}

~~~

~~~java
package com.feige.thread;

import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class ThreadMain4 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ThreadTest4 threadTest1 = new ThreadTest4();
        ThreadTest4 threadTest2 = new ThreadTest4();
        ThreadTest4 threadTest3 = new ThreadTest4();
        FutureTask<String> futureTask1 = new FutureTask<String>(threadTest1);
        FutureTask<String> futureTask2 = new FutureTask<String>(threadTest2);
        FutureTask<String> futureTask3 = new FutureTask<String>(threadTest3);
        new Thread(futureTask1,"thread--1").start();
        new Thread(futureTask2,"thread--2").start();
        new Thread(futureTask3,"thread--3").start();
        System.out.println(futureTask1.get());
        System.out.println(futureTask2.get());
        System.out.println(futureTask3.get());
    }
}

~~~

方式四：使用 Executors 工具类创建线程池

Executors提供了一系列工厂方法用于创先线程池，返回的线程池都实现了ExecutorService接口。

主要有newFixedThreadPool，newCachedThreadPool，newSingleThreadExecutor，newScheduledThreadPool

~~~java
package com.feige.thread;

/**
 * 方式四：线程池,主要有newFixedThreadPool，newCachedThreadPool，newSingleThreadExecutor，newScheduledThreadPool
 */
public class ThreadTest5 implements Runnable {
    public void run() {
        System.out.println("执行了-->" + Thread.currentThread().getName());
    }
}

~~~

~~~java
package com.feige.thread;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadMain5 {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        ThreadTest5 threadTest5 = new ThreadTest5();
        for (int i = 0; i < 10; i++) {
            executorService.execute(threadTest5);
        }
        System.out.println("开始执行");
        executorService.shutdown();
    }
}

~~~

# 3、静态代理

~~~java
package com.feige.proxy;

/**
 * 静态代理模式总结：
 *  真是对象和代理对象都要实现同一个接口
 *  代理对象要代理真实角色
 * 好处：
 *  代理对象可以做很多真实对象做不了的事情
 *  真实对象专注做自己的事情
 */
public class StaticProxy {
    public static void main(String[] args) {
        //对比
        new Thread(() -> System.out.println("我爱你")).start();
        new WeddingCompany(new You()).HappyMarry();
    }
}


interface Marry{
    void HappyMarry();
}
class You implements Marry{
    public void HappyMarry() {
        System.out.println("结婚");
    }
}

class WeddingCompany implements Marry{
    private Marry target;

    public WeddingCompany(Marry target) {
        this.target = target;
    }

    public void HappyMarry() {
        before();
        this.target.HappyMarry();
        after();
    }

    private void after() {
        System.out.println("结婚之后，收尾款");
    }

    private void before() {
        System.out.println("结婚之前，布置现场");
        
    }
}
~~~

# 4、Lambda

了解Lambda表达式之前我们先了解一下什么是函数式接口

**函数式接口：**任何接口如果只包含一个抽象方法，那么它就是一个函数式接口，我们可以通过Lambda表达式来创建该接口的对象

例：**Runnable**接口就是一个函数式接口

~~~java
package java.lang;

@FunctionalInterface
public interface Runnable {
    
    public abstract void run();
}

~~~

**静态内部类 局部内部类 匿名内部类 Lambda表达式** 

~~~java
package com.feige.lambda;

public class LambdaTest {
    //静态内部类
    static class Like2 implements ILike{

        @Override
        public void lambda() {
            System.out.println("I like lambda2");

        }
    }
    public static void main(String[] args) {
        ILike like = new Like();
        like.lambda();
        ILike like2 = new Like2();
        like2.lambda();
        //局部内部类
        class Like3 implements ILike{

            @Override
            public void lambda() {
                System.out.println("I like lambda3");
            }
        }
        ILike like3 = new Like3();
        like3.lambda();

        //匿名内部类，没有名字，借助接口或父类
        ILike like4 = new ILike() {
            @Override
            public void lambda() {
                System.out.println("I like lambda4");

            }
        };
        like4.lambda();
        //Lambda简化
        ILike like5 = () -> {
            System.out.println("I like lambda5");
        };
        like5.lambda();
        //Lambda再简化
        like5 = () -> System.out.println("I like lambda5");
        like5.lambda();
        
        //有一个参数的Lambda
        ILove love = (int a) -> {
            System.out.println("I love " + a);
        };
        love.love(9);
        //再简化
        love = a -> System.out.println("I love " + a);
        love.love(9);
        //有两个参数的Lambda
        ILove1 love1 = (int a,int b) -> {
            System.out.println("I love " + a);
            System.out.println("I love " + b);
        };
        love1.love(1314,520);
        //再简化
        love1 = (a,b) -> {
            System.out.println("I love " + a);
            System.out.println("I love " + b);
        };
        love1.love(1314,520);
        /**
         * 总结： 
         *  接口是函数式接口
         *  lambda表达式只有一行代码的时候才能简化花括号
         *  多个参数可以去掉参数类型，要去掉就全部去掉，必须加上括号
         */
    }
    
}
//定义一个函数式接口
interface ILike{
    void lambda();
}

class Like implements ILike{
    @Override
    public void lambda() {
        System.out.println("I like lambda");
    }
}

interface ILove{
    void love(int a);
}

interface ILove1{
    void love(int a,int b);
}
~~~

# 5、线程的状态

![线程状态图](https://gitee.com/feigeCode/picture/raw/master/img/thread-status.jpg)



~~~java
package com.feige.thread;

public class ThreadStateTest {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("/////");
        });
        //观察状态
        Thread.State state = t.getState();
        System.out.println(state);
        //启动后
        t.start();
        state = t.getState();
        System.out.println(state);

        
        while (state != Thread.State.TERMINATED){
            Thread.sleep(1000);
            state = t.getState();
            System.out.println(state);
        }

    }
}

~~~

~~~txt
NEW
RUNNABLE
RUNNABLE
TIMED_WAITING
TIMED_WAITING
TIMED_WAITING
RUNNABLE
/////
TERMINATED
~~~

## 线程停止（stop）

~~~java
package com.feige.thread;

/**
 * 停止线程
 *  建议线程正常停止---> 利用次数，不建议死循环
 *  建议使用标志位---> 设置一个标志位
 *  不要使用stop()或者destroy()等过时的或者jdk不建议使用的方法
 */
public class StopThreadTest implements Runnable{
    private boolean flag = true;

    @Override
    public void run() {
        int i = 0;
        while (flag){
            System.out.println("run ---> Thread" + i);
        }
    }
    //设置一个公开的方法停止线程，转换标志位
    public void stop(){
        this.flag = false;
    }
    public static void main(String[] args) {
        StopThreadTest stopThreadTest = new StopThreadTest();
        new Thread(stopThreadTest).start();
        for (int i = 0; i < 1000; i++) {
            System.out.println("main " + i);
            if (i == 900){
                //调用stop方法切换标志位，让线程停止
                stopThreadTest.stop();
                System.out.println("该线程停止了");
            }
        }
    }
}

~~~

## 线程休眠（sleep）

**模拟网络延时：放大问题的发生性**

**买票**

~~~java
package com.feige.thread;
//模拟网络延时：放大问题的发生性
public class ThreadTest3 implements Runnable {
    private int ticketNums = 10;
    public void run() {
        while (true){
            if (ticketNums <= 0){
                break;
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "-->拿到了第"+ ticketNums-- +"张票");
        }
    }
}

~~~

~~~java
package com.feige.thread;

public class ThreadMain2 {
    public static void main(String[] args) {
        ThreadTest3 threadTest3 = new ThreadTest3();
        new Thread(threadTest3,"小王").start();
        new Thread(threadTest3,"小李").start();
        new Thread(threadTest3,"小胡").start();
    }
}

~~~

~~~txt
小胡-->拿到了第9张票
小李-->拿到了第8张票
小王-->拿到了第10张票
小李-->拿到了第7张票
小胡-->拿到了第5张票
小王-->拿到了第6张票
小王-->拿到了第4张票
小李-->拿到了第3张票
小胡-->拿到了第3张票
小王-->拿到了第2张票
小胡-->拿到了第1张票
小李-->拿到了第1张票
~~~

**这里涉及到一个问题，后面再提**

**模拟倒计时**

~~~java
package com.feige.thread;

import java.text.SimpleDateFormat;
import java.util.Date;

//模拟倒计时
public class SleepTest {
    public static void printCurrentTime() throws InterruptedException {
        Date startTime = new Date(System.currentTimeMillis());
        while (true){
            Thread.sleep(1000);
            System.out.println(new SimpleDateFormat("HH:mm:ss").format(startTime));
            startTime = new Date(System.currentTimeMillis());//更新当前时间
        }
    }

    public static void tenDown() throws InterruptedException {
        int num = 10;
        while (true){
            Thread.sleep(1000);
            System.out.println(num--);
            if (num <= 0){
                break;
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        //tenDown();
        printCurrentTime();
    }
}

~~~

## 线程礼让（yield）

- 礼让线程，让当前正在执行的线程暂停，但不阻塞
- 将线程从运行状态转为就绪状态
- 让CPU重新调度，礼让不一定成功，要看CPU的心情

~~~java
package com.feige.thread;

public class YieldTest {
    public static void main(String[] args) {
        MyYield myYield = new MyYield();
        new Thread(myYield,"a").start();
        new Thread(myYield,"b").start();
    }
}
class MyYield implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "线程开始执行");
        Thread.yield();//礼让
        System.out.println(Thread.currentThread().getName() + "线程
                           停止执行");
    }
}
~~~

~~~txt
b线程开始执行
a线程开始执行
b线程停止执行
a线程停止执行
~~~

~~~txt
a线程开始执行
a线程停止执行
b线程开始执行
b线程停止执行
~~~



## 线程合并（join）

- join合并线程，待此线程执行完成后，再执行其他线程，其他线程阻塞
- 可以想象成插队

~~~java
package com.feige.thread;

public class JoinTest implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("线程vip来了" + i);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        //启动我们的线程
        Thread thread = new Thread(new JoinTest());
        thread.start();
        //主线程
        for (int i = 0; i < 100; i++) {
            if (i == 20){
                thread.join();//插队
            }
            System.out.println("main" + i);
        }


    }
}

~~~

## 线程优先级（priority）

- 先设置优先级再启动
- 优先级低只意味着获取调度的概率低，并不是优先级低就不会被调用了，这都是看CPU的调度

~~~java
package com.feige.thread;

public class PriorityTest {
    public static void main(String[] args) {
        //主线程的默认优先级
        System.out.println(Thread.currentThread().getName() + "--> " + Thread.currentThread().getPriority());
        MyPriority myPriority = new MyPriority();
        Thread t1 = new Thread(myPriority);
        Thread t2 = new Thread(myPriority);
        Thread t3 = new Thread(myPriority);
        Thread t4 = new Thread(myPriority);
        Thread t5 = new Thread(myPriority);
        //先设置优先级，再启动
        t1.start();

        t2.setPriority(4);
        t2.start();

        t3.setPriority(Thread.MAX_PRIORITY);
        t3.start();

        t4.setPriority(8);
        t4.start();

        t5.setPriority(9);
        t5.start();
    }
}
class MyPriority implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "--> " + Thread.currentThread().getPriority());
    }
}
~~~

~~~txt
main--> 5
Thread-0--> 5
Thread-2--> 10
Thread-3--> 8
Thread-4--> 9
Thread-1--> 4
~~~

## 守护线程（daemon）

**守护线程和用户线程**

- **用户 (User) 线程**：运行在前台，执行具体的任务，如程序的主线程、连接网络的子线程等都是用户线程
- **守护 (Daemon) 线程**：运行在后台，为其他前台线程服务。也可以说守护线程是 JVM 中非守护线程的 **“佣人”**。一旦所有用户线程都结束运行，守护线程会随 JVM 一起结束工作

main 函数所在的线程就是一个用户线程啊，main 函数启动的同时在 JVM 内部同时还启动了好多守护线程，比如垃圾回收线程。

比较明显的区别之一是用户线程结束，JVM 退出，不管这个时候有没有守护线程运行。而守护线程不会影响 JVM 的退出。

**注意事项：**

1. `setDaemon(true)`必须在`start()`方法前执行，否则会抛出 `IllegalThreadStateException` 异常
2. 在守护线程中产生的新线程也是守护线程
3. 不是所有的任务都可以分配给守护线程来执行，比如读写操作或者计算逻辑
4. 守护 (Daemon) 线程中不能依靠 finally 块的内容来确保执行关闭或清理资源的逻辑。因为我们上面也说过了一旦所有用户线程都结束运行，守护线程会随 JVM 一起结束工作，所以守护 (Daemon) 线程中的 finally 语句块可能无法被执行。

~~~java
package com.feige.thread;

public class DaemonTest {
    public static void main(String[] args) {
        God god = new God();
        You you = new You();
        Thread thread = new Thread(god);
        thread.setDaemon(true);//默认是false表示用户线程，正常的线程都是用户线程
        thread.start();
        new Thread(you).start();//你，用户线程启动
    }
}
//上帝
class God implements Runnable{
    @Override
    public void run() {
        while (true){
            System.out.println("上帝保佑你");
        }
    }
}
//你
class You implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 36500; i++) {
            System.out.println("你一生开心的活着");
        }
        System.out.println("good bye");
    }
}
~~~

# 6、线程同步

- 由于同一进程的多个线程共享同一块存储空间，在带来方便的同时，也带来了，访问冲突问题，为了保证数据在方法中被访问时的正确性，在访问时加入**锁机制synchronized**，当一个线程获得对象的排它锁，独占资源，其他线程必须等待使用后释放锁即可，存在以下问题：
	- 一个线程持有锁会导致其他所有需要此锁的线程挂起
	- 在多线程竞争下，加锁，释放锁会导致比较多的上下文切换和调度延时，引起性能问题
	- 如果一个优先级高的线程等待一个优先级较低的线程释放锁会导致优先级倒置，引起性能问题；

## 不安全例子

### **买票**

~~~java
package com.feige.sync;

//不安全买票
public class UnsafeBuyTicket  {
    public static void main(String[] args) {
        BuyTicket buyTicket = new BuyTicket();
        new Thread(buyTicket,"苦逼的我").start();
        new Thread(buyTicket,"牛逼的我").start();
        new Thread(buyTicket,"黄牛党").start();
    }
}
class BuyTicket implements Runnable{
    private int ticketNums = 10;
    private boolean flag = true;//外部停止方式
    @Override
    public void run() {
        //买票
        while (flag){
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    private void buy() throws InterruptedException {
        //判断是否有票
        if (ticketNums <= 0){
            this.flag = false;
            return;
        }
        Thread.sleep(100);
        //买票
        System.out.println(Thread.currentThread().getName() + "拿到了" + ticketNums--);
    }
}
~~~

### 取钱

~~~java
package com.feige.sync;

public class UnsafeBank {
    public static void main(String[] args) {
        Account account = new Account(100,"结婚基金");
        Drawing you = new Drawing(account, 50, "你");
        Drawing girlFriend = new Drawing(account, 100, "girlFriend");
        you.start();
        girlFriend.start();
    }
}


//账户
class Account{
    int money;
    String name;
    public Account(int money,String name){
        this.money  = money;
        this.name = name;
    }
}
class Drawing extends Thread{
    Account account;
    int drawingMoney;
    int nowMoney;
    public Drawing(Account account,int drawingMoney,String name){
        super(name);
        this.account = account;
        //取了多少钱
        this.drawingMoney = drawingMoney;
    }
    public void run(){
        //判断账户有没有钱
        if (account.money - drawingMoney < 0){
            System.out.println(Thread.currentThread().getName() + "钱不够，取不了");
            return;
        }
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //卡内余额 = 余额 - 你取得钱
        account.money = account.money - drawingMoney;
        //你手里的钱
        nowMoney = nowMoney + drawingMoney;

        System.out.println(account.name + "余额为:"+account.money);
        System.out.println(this.getName() + "手里的钱" + nowMoney);
    }

}
~~~

### List

~~~java
package com.feige.sync;


import java.util.ArrayList;
import java.util.List;

//List<>不安全
public class UnsafeList {
    public static void main(String[] args) throws InterruptedException {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(() -> {
                list.add(Thread.currentThread().getName());
            }).start();
        }
        Thread.sleep(3000);
        System.out.println(list.size());
    }
}

~~~

## 安全例子

### 买票

~~~java
package com.feige.sync;

//不安全买票
public class UnsafeBuyTicket  {
    public static void main(String[] args) {
        BuyTicket buyTicket = new BuyTicket();
        new Thread(buyTicket,"苦逼的我").start();
        new Thread(buyTicket,"牛逼的我").start();
        new Thread(buyTicket,"黄牛党").start();
    }
}
class BuyTicket implements Runnable{
    private int ticketNums = 10;
    private boolean flag = true;//外部停止方式
    @Override
    public void run() {
        //买票
        while (flag){
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    //锁的是this
    private synchronized void buy() throws InterruptedException {
        //判断是否有票
        if (ticketNums <= 0){
            this.flag = false;
            return;
        }
        Thread.sleep(100);
        //买票
        System.out.println(Thread.currentThread().getName() + "拿到了" + ticketNums--);
    }
}
~~~



~~~txt
苦逼的我拿到了10
苦逼的我拿到了9
苦逼的我拿到了8
苦逼的我拿到了7
苦逼的我拿到了6
苦逼的我拿到了5
苦逼的我拿到了4
苦逼的我拿到了3
黄牛党拿到了2
黄牛党拿到了1
~~~

### 取钱

~~~java
package com.feige.sync;

public class UnsafeBank {
    public static void main(String[] args) {
        Account account = new Account(100,"结婚基金");
        Drawing you = new Drawing(account, 50, "你");
        Drawing girlFriend = new Drawing(account, 100, "girlFriend");
        you.start();
        girlFriend.start();
    }
}


//账户
class Account{
    int money;
    String name;
    public Account(int money,String name){
        this.money  = money;
        this.name = name;
    }
}
class Drawing extends Thread{
    Account account;
    int drawingMoney;
    int nowMoney;
    public Drawing(Account account,int drawingMoney,String name){
        super(name);
        this.account = account;
        //取了多少钱
        this.drawingMoney = drawingMoney;
    }
    public void run(){
        //同步块,锁账户
        synchronized (account){
            //判断账户有没有钱
            if (account.money - drawingMoney < 0){
                System.out.println(Thread.currentThread().getName() + "钱不够，取不了");
                return;
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //卡内余额 = 余额 - 你取得钱
            account.money = account.money - drawingMoney;
            //你手里的钱
            nowMoney = nowMoney + drawingMoney;

            System.out.println(account.name + "余额为:"+account.money);
            System.out.println(this.getName() + "手里的钱" + nowMoney);
        }

    }
}
~~~



~~~txt
结婚基金余额为:50
你手里的钱50
girlFriend钱不够，取不了
~~~

### List

~~~java
package com.feige.sync;


import java.util.ArrayList;
import java.util.List;

//List<>不安全
public class UnsafeList {
    public static void main(String[] args) throws InterruptedException {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(() -> {
                //锁增删改查的对象
                synchronized (list){
                    list.add(Thread.currentThread().getName());
                }
            }).start();
        }
        Thread.sleep(3000);
        System.out.println(list.size());
    }
}

~~~

~~~txt
10000
~~~

~~~java
package com.feige.sync;

import java.util.concurrent.CopyOnWriteArrayList;
//JUC下的安全list
public class JUCTest {
    public static void main(String[] args) throws InterruptedException {
        CopyOnWriteArrayList<String> strings = new CopyOnWriteArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(() -> {
                strings.add(Thread.currentThread().getName());
            }).start();
        }
        Thread.sleep(3000);
        System.out.println(strings.size());
    }
}

~~~

# 7、死锁

## **什么是线程死锁**

**百度百科：**

死锁是指两个或两个以上的进程（线程）在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁，这些永远在互相等待的进程（线程）称为死锁进程（线程）。

多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。

如下图所示，线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态。

![线程死锁](https://gitee.com/feigeCode/picture/raw/master/img/sisuo.png)

## 产生死锁的原因主要是：

-  因为系统资源不足。
-  进程运行推进的顺序不合适
- 资源分配不当等。

如果系统资源充足，进程的资源请求都能够得到满足，死锁出现的可能性就很低，否则
就会因争夺有限的资源而陷入死锁。其次，进程运行推进顺序与速度不同，也可能产生死锁。

## 产生死锁的四个必要条件：

-  互斥条件：一个资源每次只能被一个进程使用。
- 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
- 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
- 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

这四个条件是**死锁的必要条件**，只要系统发生死锁，这些条件必然成立，而只要上述条件之
一不满足，就不会发生死锁。

## 死锁的解除与预防：

理解了死锁的原因，尤其是产生死锁的四个必要条件，就可以最大可能地避免、预防和
解除死锁。所以，在系统设计、进程调度等方面注意如何不让这四个必要条件成立，如何确
定资源的合理分配算法，避免进程永久占据系统资源。此外，也要防止进程在处于等待状态
的情况下占用资源。因此，对资源的分配要给予合理的规划。

## 死锁的例子

~~~~java
package com.feige.deadlock;

/**
 * 两个人同时抱有对方的锁，形成死锁
 */
public class DeadLock {
    public static void main(String[] args) {
        MakeUp g1 = new MakeUp(0, "灰姑娘");
        MakeUp g2 = new MakeUp(1, "白雪公主");
        g1.start();
        g2.start();
    }
}
//口红
class Lipstick{

}
//镜子
class Mirror{

}

class MakeUp extends Thread{

    static Lipstick lipstick = new Lipstick();
    static Mirror mirror = new Mirror();

    int choice;//选择
    String girlName;//使用化妆品的人
    public MakeUp(int choice,String girlName){
        this.choice = choice;
        this.girlName = girlName;
    }

    @Override
    public void run() {
        try {
            makeUp();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private void makeUp() throws InterruptedException {
        if (choice == 0) {
            synchronized (lipstick) {
                System.out.println(this.girlName + "获取口红的锁");
                Thread.sleep(1000);
                synchronized (mirror) {
                    System.out.println(this.girlName + "获取镜子的锁");
                }
            }
        } else {
            synchronized (mirror) {
                System.out.println(this.girlName + "获取镜子的锁");
                Thread.sleep(2000);
                synchronized (lipstick) {
                    System.out.println(this.girlName + "获取口红的锁");
                }
            }
        }
    }
}

~~~~

~~~txt
白雪公主获取镜子的锁
灰姑娘获取口红的锁
~~~



## 修改避免死锁

~~~java
package com.feige.deadlock;

/**
 * 两个人不同时抱有对方的锁，可以运行下去
 */
public class DeadLock {
    public static void main(String[] args) {
        MakeUp g1 = new MakeUp(0, "灰姑娘");
        MakeUp g2 = new MakeUp(1, "白雪公主");
        g1.start();
        g2.start();
    }
}
//口红
class Lipstick{

}
//镜子
class Mirror{

}

class MakeUp extends Thread{

    static Lipstick lipstick = new Lipstick();
    static Mirror mirror = new Mirror();

    int choice;//选择
    String girlName;//使用化妆品的人
    public MakeUp(int choice,String girlName){
        this.choice = choice;
        this.girlName = girlName;
    }

    @Override
    public void run() {
        try {
            makeUp();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private void makeUp() throws InterruptedException {
        if (choice == 0) {
            synchronized (lipstick) {
                System.out.println(this.girlName + "获取口红的锁");
                Thread.sleep(1000);
            }
            synchronized (mirror) {
                System.out.println(this.girlName + "获取镜子的锁");
            }
        } else {
            synchronized (mirror) {
                System.out.println(this.girlName + "获取镜子的锁");
                Thread.sleep(2000);

            }
            synchronized (lipstick) {
                System.out.println(this.girlName + "获取口红的锁");
            }
        }
    }
}

~~~

~~~txt
白雪公主获取镜子的锁
灰姑娘获取口红的锁
白雪公主获取口红的锁
灰姑娘获取镜子的锁
~~~



# 8、Lock

![image-20201007122423330](https://gitee.com/feigeCode/picture/raw/master/img/image-20201007122423330.png)

## **不安全**

~~~java
package com.feige.lock;

public class LockTest {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();
        new Thread(ticket).start();
        new Thread(ticket).start();
        new Thread(ticket).start();
    }
}

class Ticket implements Runnable{
    int num = 10;
    @Override
    public void run() {
        while (true){
            if (num <= 0){
                break;
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(num--);
        }
    }
}

~~~

~~~java
9
8
10
7
5
6
4
4
4
3
2
2
1
0
0
~~~



## **加锁安全**

~~~java
package com.feige.lock;

import java.util.concurrent.locks.ReentrantLock;

public class LockTest {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();
        new Thread(ticket).start();
        new Thread(ticket).start();
        new Thread(ticket).start();
    }
}

class Ticket implements Runnable{
    int num = 10;
    private final ReentrantLock lock = new ReentrantLock();
    @Override
    public void run() {
        while (true){
            lock.lock();
            if (num <= 0){
                break;
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(num--);
            lock.unlock();
        }
    }
}

~~~

~~~txt
10
9
8
7
6
5
4
3
2
1
~~~

## synchronized与Lock对比

![image-20201007123458988](https://gitee.com/feigeCode/picture/raw/master/img/image-20201007123458988.png)

# 9、生产者和消费者

## 管程法

~~~java
package com.feige.pc;

/**
 * 生产者消费者模型-->利用缓冲区解决法：管程法
 * 生产者，消费者，产品，缓冲区
 */
public class PCTest {
    public static void main(String[] args) {
        SynContainer synContainer = new SynContainer();
        new Productor(synContainer).start();
        new Consumer(synContainer).start();
    }
}
//生产者
class Productor extends Thread{
    SynContainer synContainer;

    public Productor(SynContainer synContainer) {
        this.synContainer = synContainer;
    }

    //生产
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("生产了" + i + "只鸡");
            synContainer.push(new Chicken(i));
        }
    }
}
//消费者
class Consumer extends Thread{
    SynContainer synContainer;

    public Consumer(SynContainer synContainer) {
        this.synContainer = synContainer;
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了" + synContainer.pop().id + "只鸡");

        }
    }

}
//产品
class Chicken{
    int id;


    public Chicken(int id) {
        this.id = id;
    }
}
//缓冲区
class SynContainer{
    //需要一个容器大小
    Chicken[]  chickens = new Chicken[10];
    //容器计数器
    int count = 0;
    public synchronized void push(Chicken chicken){
        if (count == chickens.length){
            //通知消费者消费，生产者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //如果没满，我们需要丢入产品
        chickens[count] = chicken;
        count++;
        //可以通知消费者消费了
        this.notifyAll();
    }
    public synchronized Chicken pop(){
        if (count == 0){
            //等待生产者生产
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //可以消费了
        count--;
        //消费完通知生产者生产
        this.notifyAll();

        return chickens[count];
    }
}
~~~

## 信号灯法

~~~java
package com.feige.pc;

/**
 * 信号灯，标志位解决
 */
public class PC2Test {
    public static void main(String[] args) {
        TV tv = new TV();
        new Player(tv).start();
        new Watch(tv).start();
    }
}
//生产者，演员
class Player extends Thread{
    TV tv;

    public Player(TV tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            if (i % 2 == 0){
                this.tv.play("快乐大本营播放中");
            }else {
                this.tv.play("抖音记录美好生活");
            }
        }
    }
}

//消费者，观从
class Watch extends Thread{
    TV tv;

    public Watch(TV tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            this.tv.watch();
        }
    }
}
//产品-->节目
class TV{

    String voice;//表演节目
    boolean flag = true;

    //表演
    public synchronized void play(String voice){
        if (!flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("演员表演了：" + voice);
        //通知观从观看
        this.notifyAll();
        this.voice = voice;
        this.flag = !this.flag;
    }

    public synchronized void watch(){
        if (flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("观看了：" + voice);
        //通知演员表演
        this.notifyAll();
        this.flag = !this.flag;
    }
}
~~~

