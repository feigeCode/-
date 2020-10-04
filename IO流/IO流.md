# 1、IO流

## Java 中 IO 流分为几种?

- 按照流的流向分，可以分为输入流和输出流；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。

- Java Io流共涉及40多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java I0流的40多个类都是从如下4个抽象类基类中派生出来的。

	- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
	- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

	按操作方式分类结构图：

	![IO-操作方式分类](IO流/sdfefsdf.jpg)

	按操作对象分类结构图：

	![IO-操作对象分类](IO流/iVFNyVCMSVCQi5wbmc.jpg)

	## BIO,NIO,AIO 有什么区别?

	简答

	- BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
	- NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
	- AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

	详细回答

	- **BIO (Blocking I/O):** 同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。在活动连接数不是特别高（小于单机1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的 I/O 并且编程模型简单，也不用过多考虑系统的过载、限流等问题。线程池本身就是一个天然的漏斗，可以缓冲一些系统处理不了的连接或请求。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。
	- **NIO (New I/O):** NIO是一种同步非阻塞的I/O模型，在Java 1.4 中引入了NIO框架，对应 java.nio 包，提供了 Channel , Selector，Buffer等抽象。NIO中的N可以理解为Non-blocking，不单纯是New。它支持面向缓冲的，基于通道的I/O操作方法。 NIO提供了与传统BIO模型中的 `Socket` 和 `ServerSocket` 相对应的 `SocketChannel` 和 `ServerSocketChannel` 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞I/O来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发
	- **AIO (Asynchronous I/O):** AIO 也就是 NIO 2。在 Java 7 中引入了 NIO 的改进版 NIO 2,它是异步非阻塞的IO模型。异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。AIO 是异步IO的缩写，虽然 NIO 在网络操作中，提供了非阻塞的方法，但是 NIO 的 IO 行为还是同步的。对于 NIO 来说，我们的业务线程是在 IO 操作准备好时，得到通知，接着就由这个线程自行进行 IO 操作，IO操作本身是同步的。查阅网上相关资料，我发现就目前来说 AIO 的应用还不是很广泛，Netty 之前也尝试使用过 AIO，不过又放弃了。

	## Files的常用方法都有哪些？

	- Files. exists()：检测文件路径是否存在。
	- Files. createFile()：创建文件。
	- Files. createDirectory()：创建文件夹。
	- Files. delete()：删除一个文件或目录。
	- Files. copy()：复制文件。
	- Files. move()：移动文件。
	- Files. size()：查看文件个数。
	- Files. read()：读取文件。
	- Files. write()：写入文件。

~~~java
package com.feige.io;

import org.junit.Test;

import java.io.*;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class Main {
    /**
     * 字节流拷贝图片
     * @throws IOException
     */
    @Test
    public void test1() throws IOException {
        File file;
        File file1;
        InputStream inputStream = null;
        OutputStream outputStream = null;
        try{
            file = new File("C:\\Users\\Administrator\\Pictures\\Saved Pictures\\sw4.jpg");
            inputStream = new FileInputStream(file);
            file1 = new File("E:\\project\\java-project\\java-notes-code\\io-stream\\img\\sw4.png");
            outputStream = new FileOutputStream(file1);
            byte[] bytes = new byte[1024];
            while ((inputStream.read(bytes)) != -1){
                outputStream.write(bytes);
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            assert inputStream != null;
            inputStream.close();
            assert outputStream != null;
            outputStream.close();
        }
        
    }

    /**
     * 字符流读文件输出到控制台
     */
    @Test
    public void test2() throws IOException {
        File file;
        Reader reader = null;
        try {
            file = new File("E:\\project\\java-project\\java-notes-code\\io-stream\\img\\Main.java");
            reader = new FileReader(file);
            char[] chars = new char[1024];
            while ((reader.read(chars)) != -1){
                System.out.println(String.valueOf(chars));
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            assert reader != null;
            reader.close();
        }
        
    }

    /**
     * Files类常用的方法
     * @throws IOException
     */
    @Test
    public void test3() throws IOException {
        InputStream inputStream = null;
        try{
            File file = new File("C:\\Users\\Administrator\\Pictures\\Saved Pictures\\sw4.jpg");
            inputStream = new FileInputStream(file);
            Path path = Paths.get("E:\\project\\java-project\\java-notes-code\\io-stream\\img\\sw.jpg");
            Path path1 = Paths.get("E:\\project\\java-project\\java-notes-code\\io-stream\\img1\\sw.png");
            //拷贝文件，返回文件的大小
            long copy = Files.copy(inputStream,path);
            System.out.println(copy);
            //判断文件是否存在
            boolean exists = Files.exists(path);
            System.out.println(exists);
            //移动文件
            Path move = Files.move(path,path1);
            System.out.println(move);
            //得到文件大小
            long size = Files.size(path1);
            System.out.println(size);
            //判断文件是否存在，存在则删除
            boolean b = Files.deleteIfExists(path1);
            System.out.println(b);
            //创建新的文件
            Path file1 = Files.createFile(path1);
            System.out.println(file1);
            //创建一个文件夹
            //Files.createDirectory()
            //读取文件。
            //Files.read()
            //写入文件。
            //Files.write()
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            assert inputStream != null;
            inputStream.close();
        }
        
    }
}

~~~

[原文链接](https://thinkwon.blog.csdn.net/article/details/104390612)

