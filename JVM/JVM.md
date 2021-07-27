# 1、JVM内存模型

[原文链接](https://thinkwon.blog.csdn.net/article/details/104390752)

![image-20201017204353391](https://gitee.com/feigeCode/picture/raw/master/img/image-20201017204353391.png)

JVM包括两个子系统和两个组件，两个子系统为Class Loader（类装载）、Exection engine（执行引擎）；两个组件为Runtime Data Area（运行时数据区），Native Interface（本地接口）。

- `Class loader(类装载)`：根据给定的全限定名类名(如：java.lang.Object)来装载class文件到Runtime data area中的method area。
- `Execution engine(执行引擎）`：执行classes中的指令。
- `Native Interface(本地接口)`：与native libraries交互，是其它编程语言交互的接口。
- `Runtime data area(运行时数据区域)`：这就是我们常说的JVM的内存。

**作用** ：首先通过编译器把 Java 代码转换成字节码，类加载器（ClassLoader）再把字节码加载到内存中，将其放在运行时数据区（Runtime data area）的方法区内，而字节码文件只是 JVM 的一套指令集规范，并不能直接交给底层操作系统去执行，因此需要特定的命令解析器执行引擎（Execution Engine），将字节码翻译成底层系统指令，再交由 CPU 去执行，而这个过程中需要调用其他语言的本地库接口（Native Interface）来实现整个程序的功能。



# 2、 JVM运行时数据区

**Java 虚拟机在执行 Java 程序的过程中会把它所管理的内存区域划分为若干个不同的数据区域**。这些区域都有各自的用途，以及创建和销毁的时间，有些区域随着虚拟机进程的启动而存在，有些区域则是依赖线程的启动和结束而建立和销毁。Java 虚拟机所管理的内存被划分为如下几个区域：

![img](https://gitee.com/feigeCode/picture/raw/master/img/20200103213220764.png)

不同虚拟机的运行时数据区可能略微有所不同，但都会遵从 Java 虚拟机规范， Java 虚拟机规范规定的区域分为以下 5 个部分：

- `程序计数器（Program Counter Register）`：当前线程所执行的字节码的行号指示器，字节码解析器的工作是通过改变这个计数器的值，来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能，都需要依赖这个计数器来完成；
- `Java 虚拟机栈（Java Virtual Machine Stack）`：用于存储局部变量表、操作数栈、动态链接、方法出口等信息；
- `本地方法栈（Native Method Stack）`：与虚拟机栈的作用是一样的，只不过虚拟机栈是服务 Java 方法的，而本地方法栈是为虚拟机调用 Native 方法服务的；
- `Java 堆（Java Heap）`：Java 虚拟机中内存最大的一块，是被所有线程共享的，几乎所有的对象实例都在这里分配内存；
- `方法区（Methed Area）`：用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据。

# 2、栈（Stack）

**百度百科**

栈（stack）又名堆栈，它是一种运算受限的线性表。限定仅在表尾进行插入和删除操作的线性表。这一端被称为栈顶，相对地，把另一端称为栈底。向一个栈插入新元素又称作进栈、入栈或压栈，它是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。

**栈是先进后出（FILO）**，与之相对应的还有队列，**队列是先进先出（FIFO）**

**栈存放：局部变量，操作数栈，返回结果。该区更关注的是程序方法的执行。**



![image-20201017222157975](https://gitee.com/feigeCode/picture/raw/master/img/image-20201017222157975.png)

![image-20201017215635829](https://gitee.com/feigeCode/picture/raw/master/img/image-20201017215635829.png)

　　每当启动一个新线程时，Java虚拟机都会为它分配一个Java栈。Java栈以帧为单位保存线程的运行状态。虚拟机只会直接对Java栈执行两种操作：以帧为单位的压栈和出栈。

​		某个线程正在执行的方法被称为该线程的当前方法，当前方法使用的栈帧称为当前帧，当前方法所属的类称为当前类，当前类的常量池称为当前常量池。在线程执行一个方法时，它会跟踪当前类和当前常量池。此外，当虚拟机遇到栈内操作指令时，它对当前帧内数据执行操作。

　　每当线程调用一个Java方法时，虚拟机都会在该线程的Java栈中压入一个新帧。而这个新帧自然就成为了当前帧。在执行这个方法时，它使用这个帧来存储参数、局部变量、中间运算结果等数据。

　　Java方法可以以两种方式完成。一种通过return返回的，称为正常返回；一种是通过抛出异常而异常终止的。不管以哪种方式返回，虚拟机都会将当前帧弹出Java栈然后释放掉，这样上一个方法的帧就成为当前帧了。

　　Java帧上的所有数据都是此线程私有的。任何线程都不能访问另一个线程的栈数据，因此我们不需要考虑多线程情况下栈数据的访问同步问题。当一个线程调用一个方法时，方法的的局部变量保存在调用线程Java栈的帧中。只有一个线程能总是访问那些局部变量，即调用方法的线程。

# 3、堆（Heap）

![image-20201017225833596](https://gitee.com/feigeCode/picture/raw/master/img/image-20201017225833596.png)

 **一、新生代**

　　新生代主要用来存放新生的对象。一般占据堆空间的1/3。在新生代中，保存着大量的刚刚创建的对象，但是大部分的对象都是朝生夕死，所以在新生代中会频繁的进行MinorGC，进行垃圾回收。新生代又细分为三个区：Eden区、SurvivorFrom、ServivorTo区，三个区的默认比例为：8：1：1。

-  **Eden区：**Java新创建的对象绝大部分会分配在Eden区（如果对象太大，则直接分配到老年代）。当Eden区内存不够的时候，就会触发MinorGC（新生代采用的是复制算法），对新生代进行一次垃圾回收。
- **SurvivorFrom区和To区：**在GC开始的时候，对象只会存在于Eden区和名为From的Survivor区，To区是空的，一次MinorGc过后，Eden区和SurvivorFrom区存活的对象会移动到SurvivorTo区中，然后会清空Eden区和SurvivorFrom区，并对存活的对象的年龄+1，如果对象的年龄达到15，则直接分配到老年代。MinorGC完成后，SurvivorFrom区和SurvivorTo区的功能进行互换。下一次MinorGC时，会把SurvivorTo区和Eden区存活的对象放入SurvivorFrom区中，并计算对象存活的年龄。

**二、老年代**

　　老年代主要存放应用中生命周期长的内存对象。老年代比较稳定，不会频繁的进行MajorGC。而在MaiorGC之前才会先进行一次MinorGc，使得新生的对象进入老年代而导致空间不够才会触发。当无法找到足够大的连续空间分配给新创建的较大对象也会提前触发一次MajorGC进行垃圾回收腾出空间。

　　在老年代中，MajorGC采用了标记—清除算法：首先扫描一次所有老年代里的对象，标记出存活的对象，然后回收没有标记的对象。MajorGC的耗时比较长。因为要扫描再回收。MajorGC会产生内存碎片，当老年代也没有内存分配给新来的对象的时候，就会抛出OOM（Out of Memory）异常。

**三、永久代**

　　永久代指的是永久保存区域。主要存放Class和Meta（元数据）的信息。Classic在被加载的时候被放入永久区域，它和存放的实例的区域不同，在Java8中，永久代已经被移除，取而代之的是一个称之为“元数据区”（元空间）的区域。元空间和永久代类似，都是对JVM中规范中方法的实现。不过元空间与永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存的限制。类的元数据放入native memory，字符串池和类的静态变量放入java堆中。这样可以加载多少类的元数据就不再由MaxPermSize控制，而由系统的实际可用空间来控制。

采用元空间而不用永久代的原因：

- 为了解决永久代的OOM问题，元数据和class对象存放在永久代中，容易出现性能问题和内存溢出。
- 类及方法的信息等比较难确定其大小，因此对于永久代大小指定比较困难，大小容易出现永久代溢出，太大容易导致老年代溢出（堆内存不变，此消彼长）。
- 永久代会为GC带来不必要的复杂度，并且回收效率偏低。

[原文链接](https://www.cnblogs.com/byqin/p/12512528.html)

# 4、方法区

 类的元数据（元数据并不是类的Class对象！Class对象是加载的最终产品，类的方法代码，变量名，方法名，访问权限，返回值等等都是在方法区的）才是存在方法区的！

1. 静态变量放在方法区
2. 静态的对象还是放在堆。

# 5、HotSpot虚拟机对象探秘

## 对象的创建

说到对象的创建，首先让我们看看 `Java` 中提供的几种对象创建方式：

| Header                             | 解释             |
| ---------------------------------- | ---------------- |
| 使用new关键字                      | 调用了构造函数   |
| 使用Class的newInstance方法         | 调用了构造函数   |
| 使用Constructor类的newInstance方法 | 调用了构造函数   |
| 使用clone方法                      | 没有调用构造函数 |
| 使用反序列化                       | 没有调用构造函数 |

下面是对象创建的主要流程:

![img](https://gitee.com/feigeCode/picture/raw/master/img/20200103213726902.png)

虚拟机遇到一条new指令时，先检查常量池是否已经加载相应的类，如果没有，必须先执行相应的类加载。类加载通过后，接下来分配内存。若Java堆中内存是绝对规整的，使用“指针碰撞“方式分配内存；如果不是规整的，就从空闲列表中分配，叫做”空闲列表“方式。划分内存时还需要考虑一个问题-并发，也有两种方式: CAS同步处理，或者本地线程分配缓冲(Thread Local Allocation Buffer, TLAB)。然后内存空间初始化操作，接着是做一些必要的对象设置(元信息、哈希码…)，最后执行`<init>`方法。

## 为对象分配内存

类加载完成后，接着会在Java堆中划分一块内存分配给对象。内存分配根据Java堆是否规整，有两种方式：

- 指针碰撞：如果Java堆的内存是规整，即所有用过的内存放在一边，而空闲的的放在另一边。分配内存时将位于中间的指针指示器向空闲的内存移动一段与对象大小相等的距离，这样便完成分配内存工作。
- 空闲列表：如果Java堆的内存不是规整的，则需要由虚拟机维护一个列表来记录那些内存是可用的，这样在分配的时候可以从列表中查询到足够大的内存分配给对象，并在分配后更新列表记录。

选择哪种分配方式是由 Java 堆是否规整来决定的，而 Java 堆是否规整又由所采用的垃圾收集器是否带有压缩整理功能决定。

![内存分配的两种方式](https://gitee.com/feigeCode/picture/raw/master/img/20200103213812259.png)

## 处理并发安全问题

对象的创建在虚拟机中是一个非常频繁的行为，哪怕只是修改一个指针所指向的位置，在并发情况下也是不安全的，可能出现正在给对象 A 分配内存，指针还没来得及修改，对象 B 又同时使用了原来的指针来分配内存的情况。解决这个问题有两种方案：

- 对分配内存空间的动作进行同步处理（采用 CAS + 失败重试来保障更新操作的原子性）；
- 把内存分配的动作按照线程划分在不同的空间之中进行，即每个线程在 Java 堆中预先分配一小块内存，称为本地线程分配缓冲（Thread Local Allocation Buffer, TLAB）。哪个线程要分配内存，就在哪个线程的 TLAB 上分配。只有 TLAB 用完并分配新的 TLAB 时，才需要同步锁。通过-XX:+/-UserTLAB参数来设定虚拟机是否使用TLAB。

![内存分配时保证线程安全的两种方式](https://gitee.com/feigeCode/picture/raw/master/img/20200103213833317.png)

## 对象的访问定位

`Java`程序需要通过 `JVM` 栈上的引用访问堆中的具体对象。对象的访问方式取决于 `JVM` 虚拟机的实现。目前主流的访问方式有 **句柄** 和 **直接指针** 两种方式。

> **指针：** 指向对象，代表一个对象在内存中的起始地址。
>
> **句柄：** 可以理解为指向指针的指针，维护着对象的指针。句柄不直接指向对象，而是指向对象的指针（句柄不发生变化，指向固定内存地址），再由对象的指针指向对象的真实内存地址。

### 句柄访问

`Java`堆中划分出一块内存来作为**句柄池**，引用中存储对象的**句柄地址**，而句柄中包含了**对象实例数据**与**对象类型数据**各自的**具体地址**信息，具体构造如下图所示：

![img](https://gitee.com/feigeCode/picture/raw/master/img/20200103213926911.png)

**优势**：引用中存储的是**稳定**的句柄地址，在对象被移动（垃圾收集时移动对象是非常普遍的行为）时只会改变**句柄中**的**实例数据指针**，而**引用**本身不需要修改。

### 直接指针

如果使用**直接指针**访问，**引用** 中存储的直接就是**对象地址**，那么`Java`堆对象内部的布局中就必须考虑如何放置访问**类型数据**的相关信息。

![img](https://gitee.com/feigeCode/picture/raw/master/img/20200103213948956.png)

**优势**：速度更**快**，节省了**一次指针定位**的时间开销。由于对象的访问在`Java`中非常频繁，因此这类开销积少成多后也是非常可观的执行成本。HotSpot 中采用的就是这种方式。

## 深拷贝和浅拷贝

浅拷贝（shallowCopy）只是增加了一个指针指向已存在的内存地址，

深拷贝（deepCopy）是增加了一个指针并且申请了一个新的内存，使这个增加的指针指向这个新的内存，

使用深拷贝的情况下，释放内存的时候不会因为出现浅拷贝时释放同一个内存的错误。

浅复制：仅仅是指向被复制的内存地址，如果原地址发生改变，那么浅复制出来的对象也会相应的改变。

深复制：在计算机中开辟一块**新的内存地址**用于存放复制的对象。

# 6、垃圾回收机制

java语言最显著的特点就是引入了垃圾回收机制，它使java程序员在编写程序时不再考虑内存管理的问题。

对于GC来说，当程序员创建对象时，GC就开始监控这个对象的地址、大小以及使用情况。

通常，GC采用有向图的方式记录和管理堆(heap)中的所有对象。通过这种方式确定哪些对象是"可达的"，哪些对象是"不可达的"。当GC确定一些对象为"不可达"时，GC就有责任回收这些内存空间。

可以。程序员可以手动执行System.gc()，通知GC运行，但是Java语言规范并不保证GC一定会执行。

## Java 中都有哪些引用类型？

- 强引用：发生 gc 的时候不会被回收。
- 软引用：有用但不是必须的对象，在发生内存溢出之前会被回收。
- 弱引用：有用但不是必须的对象，在下一次GC时会被回收。
- 虚引用（幽灵引用/幻影引用）：无法通过虚引用获得对象，用 PhantomReference 实现虚引用，虚引用的用途是在 gc 时返回一个通知。

## 怎么判断对象是否可以被回收？

垃圾收集器在做垃圾回收的时候，首先需要判定的就是哪些内存是需要被回收的，哪些对象是「存活」的，是不可以被回收的；哪些对象已经「死掉」了，需要被回收。

一般有两种方法来判断：

- 引用计数器法：为每个对象创建一个引用计数，有对象引用时计数器 +1，引用被释放时计数 -1，当计数器为 0 时就可以被回收。它有一个缺点不能解决循环引用的问题；
- 可达性分析算法：从 GC Roots 开始向下搜索，搜索所走过的路径称为引用链。当一个对象到 GC Roots 没有任何引用链相连时，则证明此对象是可以被回收的。

## 在Java中，对象什么时候可以被垃圾回收

当对象对当前使用这个对象的应用程序变得不可触及的时候，这个对象就可以被回收了。
垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完全垃圾回收(Full GC)。如果你仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。

## JVM中的永久代中会发生垃圾回收吗

垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完全垃圾回收(Full GC)。如果你仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。请参考下Java8：从永久代到元数据区
(译者注：Java8中已经移除了永久代，新加了一个叫做元数据区的native内存区)

## JVM三种垃圾回收算法

### 复制算法

按照容量划分二个大小相等的内存区域，当一块用完的时候将活着的对象复制到另一块上，然后再把已使用的内存空间一次清理掉。

![image-20201017234530719](https://gitee.com/feigeCode/picture/raw/master/img/image-20201017234530719.png)

优点：按顺序分配内存，实现简单、运行高效，不用考虑内存碎片。

缺点：内存使用率不高，只有原来的一半，对象存活率高时会频繁复制。



### 标记-清除算法

把回收对象进行标记，然后回收被标记的对象

![image-20201017232615025](https://gitee.com/feigeCode/picture/raw/master/img/image-20201017232615025.png)

优点：实现简单，不需要对象进行移动。

缺点：标记、清除过程效率低，产生大量不连续的内存碎片，提高了垃圾回收的频率



### 标记-压缩算法

把回收对象进行标记，然后把存活对象全部移到一起，清除被标记的对象

![image-20201017233454683](https://gitee.com/feigeCode/picture/raw/master/img/image-20201017233454683.png)

**优点**：解决了标记-清除算法存在的内存碎片问题。

**缺点**：仍需要进行局部对象移动，一定程度上降低了效率。



### 分代算法

根据对象存活周期的不同将内存划分为几块，一般是新生代和老年代，新生代基本采用复制算法，老年代采用标记-压缩算法。

# 7、JVM调优

```java
package com.feige.jvm;

public class JvmDemo1 {
    public static void main(String[] args) {
        long maxMemory = Runtime.getRuntime().maxMemory();
        long totalMemory = Runtime.getRuntime().totalMemory();
        System.out.println("maxMemory:" + maxMemory/(double)1024/1024 +"m"+ "\ntotalMemory:" + totalMemory/(double)1024/1024+"m");
    }
}
```

~~~txt
maxMemory:1788.5m
totalMemory:121.0m
~~~

**常用的 JVM 调优的参数**

- `-Xms2g`：初始化推大小为 2g；
- `-Xmx2g`：堆最大内存为 2g；
- `-XX:NewRatio=4`：设置年轻的和老年代的内存比例为 1:4；
- `-XX:SurvivorRatio=8`：设置新生代 Eden 和 Survivor 比例为 8:2；
- `–XX:+UseParNewGC`：指定使用 ParNew + Serial Old 垃圾回收器组合；
- `-XX:+UseParallelOldGC`：指定使用 ParNew + ParNew Old 垃圾回收器组合；
- `-XX:+UseConcMarkSweepGC`：指定使用 CMS + Serial Old 垃圾回收器组合；
- `-XX:+PrintGC`：开启打印 gc 信息；
- `-XX:+PrintGCDetails`：打印 gc 详细信息。
- `-XX:+HeapDumpOnOutOfMemoryError` ：当遇到OutOfMemoryError时，生成Dump文件
- `-XX:HeapDumpPath=E:\project\java-project\java-notes-code\JVM\heapdump.hprof`：指定生成Dump文件的路径



# 8、JProfiler

从网上下载JProfiler，下一步无脑安装

idea安装JProfiler插件，File->Settings->Plugins，输入JProfiler点击安装，安装成功重启即可

重启成功后点击File->Settings->Tools->JProfiler，选择JProfiler.exe文件路径输入即可

生成Dump文件

![image-20201018140136743](https://gitee.com/feigeCode/picture/raw/master/img/image-20201018140136743.png)

~~~java
package com.feige.jvm;

import java.util.ArrayList;


public class JvmDemo {
    byte[] bytes = new byte[1024];
    //-Xms2m -Xmx8m -XX:+PrintGCDetails
    //-Xms2m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=E:\project\java-project\java-notes-code\JVM\heapdump.hprof
    public static void main(String[] args) {
        int count = 0;
        ArrayList<JvmDemo> jvmDemos = new ArrayList<JvmDemo>();
        try {
            while (true){
                jvmDemos.add(new JvmDemo());
                count++;
            }
        } catch (Error e) {
            System.out.println("count:" + count);
            e.printStackTrace();
        }
    }
}

~~~



~~~txt
java.lang.OutOfMemoryError: Java heap space
Dumping heap to E:\project\java-project\java-notes-code\JVM\heapdump.hprof ...
Heap dump file created [8345370 bytes in 0.014 secs]
count:6324
Exception in thread "main" Exception in thread "Monitor Ctrl-Break" java.lang.OutOfMemoryError: Java heap space
java.lang.OutOfMemoryError: GC overhead limit exceeded
~~~

用JProfiler打开E:\project\java-project\java-notes-code\JVM\heapdump.hprof文件

![image-20201018141311052](https://gitee.com/feigeCode/picture/raw/master/img/image-20201018141311052.png)

可以看到是那一行出错

![image-20201018141501727](https://gitee.com/feigeCode/picture/raw/master/img/image-20201018141501727.png)