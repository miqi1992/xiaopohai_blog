# Java NIO

> Java NIO(New IO或Non Blocking IO)是Java 1.4版本开始引入的一个新的IO API，可以替代标准的Java IO API。 NIO支持面向缓冲区的、基于通道的IO操作。NIO将以更加高效的方式进行文件的读写操作

### java IO与java NIO的区别

![undefined](../../../../_media/JavaNIO/bc370e19gy1gceddjoyvvj20bk03lglt.jpg)

### 一、通道(Channel)与缓冲区(Buffer)

若需要使用NIO系统，需要获取用于连接IO设备的通道以及用于容纳数据的缓冲区。然后操作缓冲区，对数据进行处理。简而言之，Channel负责传输，Buffer负责存储。 

#### 1、缓冲区(Buffer)

缓冲区(Buffer):一个用于特定数据类型的容器。由java.nio包定义的所有缓冲区都是Buffer抽象类的子类。  

Java NIO 中的 Buffer 主要用于与 NIO 通道进行交互，数据是从通道读入缓冲区，从缓冲区写入通道中的。

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gcednq6cpuj20hr0a2q44.jpg)

#### 2、Buffer的常用方法

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gcedozgovqj20hi0ag0v0.jpg)



#### 3、非直接缓冲区

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gcedpo1bgfj20h509dmyl.jpg)

#### 4、直接缓冲区

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gcedqdigzxj20gk09o76a.jpg)

``` java
package com.xiaopohai;

import org.junit.Test;

import java.nio.ByteBuffer;

/**
 * 一、缓冲区(Buffer)：在JAVA NIO中负责数据的存储。缓冲区就是数组。用于存储不同类型的数据。
 *
 * 根据数据类型的不同(boolean除外)，有以下Buffer常用子类：
 * ByteBuffer
 * CharBuffer
 * ShortBuffer
 * IntBuffer
 * LongBuffer
 * FloatBuffer
 * DoubleBuffer
 *
 * 上述缓冲区的管理方式几乎一致，通过allocate()获取缓冲区
 *
 * 二、缓冲区存取数据的两个核心方法：
 * put():存入数据到缓冲区
 *     put(byte b)：将给定单个字节写入缓冲区的当前位置
 *     put(byte[] src)：将src中的字节写入缓冲区的当前位置
 *     put(int index, byte b)：将指定字节写入缓冲区的所以为(不会移动position)
 * get():获取缓冲区中的数据
 *      get()：读取单个字节
 *      get(byte[] dst)：批量读取多个字节到dst中
 *      get(int index)：读取指定索引位置的字节(不会移动position)
 *
 * 三、缓冲区中的四个核心属性：
 * capacity：容量，表示缓冲区中最大存储数据的容量。一旦声明不能改变
 * limit：界限，表示缓冲区中可以操作数据的大小。（limit后数据不能进行读写）
 * position：位置，表示缓冲区中正在操作数的位置。
 * mark: 标记，表示记录当前position位置。可以通过reset()恢复到mark的位置
 *
 * 0 <= mark <= position <= limit <= capacity
 *
 * 四、直接缓冲区与非直接缓冲区：
 * 非直接缓冲区： 通过allocate()方法进行分配缓冲区，将缓冲区建立在JVM的内存中。
 *
 * 直接缓冲区：通过allocateDirect()方法分配直接缓冲区，将缓冲区建立在物理内存中。可以提高效率，此方法返回的缓冲区进行分配和取消分配所需成本高于非直接缓冲区
 * 直接缓冲区的内容可以驻留在常规的垃圾回收堆之外。
 * 将直接缓冲区主要分配给那些易受基础系统的本机I/0操作影响的大型、持久的缓冲区。
 * 最好仅在直接缓冲区能在程序性能方面带来明显好处时分配他们。
 * 直接字节缓冲区还可以过，通过FileChannel的map()方法将文件区域直接映射到内存中来创建
 */
public class Test01_Buffer {

    @Test
    public void test1() {
        String str = "abcde";

        //1、分配一个指定大小的缓冲区
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);

        System.out.println("--------------allocate()-------------------");
        System.out.println(byteBuffer.position());
        System.out.println(byteBuffer.limit());
        System.out.println(byteBuffer.capacity());

        //2、利用put()方法存放数据到缓冲区
        byteBuffer.put(str.getBytes());

        System.out.println("-----------------put()------------------");
        System.out.println(byteBuffer.position());
        System.out.println(byteBuffer.limit());
        System.out.println(byteBuffer.capacity());

        //3、切换到读取数据模式
        byteBuffer.flip();
        System.out.println("----------------flip()-----------------");
        System.out.println(byteBuffer.position());
        System.out.println(byteBuffer.limit());
        System.out.println(byteBuffer.capacity());

        //4、利用get()去读缓冲区中的数据
        byte[] dst = new byte[byteBuffer.limit()];
        byteBuffer.get(dst);
        System.out.println(new String(dst, 0, dst.length));
        System.out.println("--------------get()----------------------");
        System.out.println(byteBuffer.position());
        System.out.println(byteBuffer.limit());
        System.out.println(byteBuffer.capacity());

        //5、rewind() 可重复度
        byteBuffer.rewind();
        System.out.println("---------rewind()-----------------");
        System.out.println(byteBuffer.position());
        System.out.println(byteBuffer.limit());
        System.out.println(byteBuffer.capacity());

        //6、clea():清空缓冲区。但是缓冲区中的数据依然存在，但是处在“被遗忘”状态
        byteBuffer.clear();

        System.out.println("------------clear()------------");
        System.out.println(byteBuffer.position());
        System.out.println(byteBuffer.limit());
        System.out.println(byteBuffer.capacity());
    }

    @Test
    public void test2() {
        String str = "abcde";
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        byteBuffer.put(str.getBytes());
        byteBuffer.flip();

        byte[] dst = new byte[byteBuffer.limit()];
        byteBuffer.get(dst, 0, 2);
        System.out.println(new String(dst, 0, 2));
        System.out.println(byteBuffer.position());

        //mark标记
        byteBuffer.mark();

        byteBuffer.get(dst, 2, 2);
        System.out.println(new String(dst, 2, 2));
        System.out.println(byteBuffer.position());

        //reset():恢复到mark的位置
        byteBuffer.reset();
        System.out.println(byteBuffer.position());

        //判断缓冲区中是否还有剩余数据
        if(byteBuffer.hasRemaining()) {
            //获取缓冲区中可以操作的数据
            System.out.println(byteBuffer.remaining());
        }
    }

}
```

###  3、通道(Channel)

通道：由java.nio.channels包定义。

Channel表示IO源与目标打开的连接。
Channel类似于传统的“流”。但其自身不能直接访问数据，Channel只能与Buffer进行交互。

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gcegwtyc6bj20jf07gjsy.jpg)

操作系统中：通道是一种通过执行通道程序管理I/O操作的控制器，它使主机（CPU和内存）与I/O操作之间达到更高的并行程度。需要进行I/O操作时，CPU只需启动通道，然后可以继续执行自身程序，通道则执行通道程序，管理与实现I/O操作。

#### FileChannel的常用方法

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gcegz49lsgj20h908qtau.jpg)

``` java
package com.xiaopohai;

import org.junit.Test;

import java.io.*;
import java.nio.ByteBuffer;
import java.nio.CharBuffer;
import java.nio.MappedByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.charset.CharacterCodingException;
import java.nio.charset.Charset;
import java.nio.charset.CharsetDecoder;
import java.nio.charset.CharsetEncoder;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

/**
 * 通道(Channel)：用于源节点与目标节点的连接。在java NIO中负责缓冲中数据的传输。Channel本身不存储数据，需要配合缓冲区进行传输。
 *
 * 二、通道的主要实现类：
 * java.nio.channels.Channel接口：
 *      |--FileChannel：用于读取、写入、映射和操作文件的通道
 *      |--SocketChannel：通过TCP读写网络中的数据
 *      |--ServerSocketChannel：可以监听新进来的TCP连接，对每一个新进来的连接都会创建一个SocketChannel
 *      |--DatagramChannel：通过UDP读写网络中的数据通道
 *
 *
 * 三、获取通道
 * 1. java针对支持通道的类提供了getChannel()方法
 *  本地IO:
 *      FileInputStream/FileOutputStream
 *      RandomAccessFile
 *
 *  网络IO:
 *      Socket
 *      ServerSocket
 *      DatagramSocket
 *
 * 2. 在JDK1.7中的NIO.2针对各个通道提供了静态方法open()
 * 3. 在JDK1.7中的NIO.2的Files工具类的newByteChannel()
 *
 * 四、通道之间的数据传输
 *  transferFrom()
 *  transferTo()
 *
 * 五、分散(Scatter)与聚集(Gather)
 *  分散读取(Scattering Reads)：将通道中的数据分散到多个缓冲区中
 *  聚集写入（Gathering Writers）：将多个缓冲区中的数据聚集到通道中
 *
 * 五、字符集：Charset
 *  编码：字符串 -》 字符数组
 *  解码：字符数组 -》 字符串
 */
public class Test02_Channel {
    @Test
    public void test1() throws IOException {
        long start = System.currentTimeMillis();

        FileInputStream fis = null;
        FileOutputStream fos = null;

        FileChannel inChannel = null;
        FileChannel outChannel = null;

//        fis = new FileInputStream("d:/1.jpeg");
//        fos = new FileOutputStream("d:/2.jpeg");
//
//        //1、获取通道
//        inChannel = fis.getChannel();
//        outChannel = fos.getChannel();

        inChannel = FileChannel.open(Paths.get("d:/1.jpg"), StandardOpenOption.READ);
        outChannel = FileChannel.open(Paths.get("d:/2.jpg"), StandardOpenOption.WRITE, StandardOpenOption.CREATE, StandardOpenOption.READ);

        //2、分配指定大小的缓冲区
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);

        //3、将通道中的数据存入缓冲区
        while(inChannel.read(byteBuffer) != -1) {
            byteBuffer.flip();  //切换读数据的模式
            //4、将缓冲区中的数据写入都通道中
            outChannel.write(byteBuffer);
            byteBuffer.clear(); //清空缓冲区
        }
        long end = System.currentTimeMillis();

        System.out.println("耗费时间: "+ (end - start));
    }

    //使用直接缓冲区完成文件的复制(内存映射文件)
    @Test
    public void test2() throws IOException {
        long start = System.currentTimeMillis();

        FileChannel inChannel = null;
        FileChannel outChannel = null;

        inChannel = FileChannel.open(Paths.get("d:/1.jpg"), StandardOpenOption.READ);
        outChannel = FileChannel.open(Paths.get("d:/2.jpg"), StandardOpenOption.WRITE, StandardOpenOption.READ, StandardOpenOption.CREATE);

        // 内存映射文件
        MappedByteBuffer inMapperBuf = inChannel.map(FileChannel.MapMode.READ_ONLY, 0, inChannel.size());
        MappedByteBuffer outMapperBuf = outChannel.map(FileChannel.MapMode.READ_WRITE, 0, inChannel.size());
        //直接对缓冲区进行数据的读写操作
        byte[] dst = new byte[inMapperBuf.limit()];
        inMapperBuf.get(dst);
        outMapperBuf.put(dst);
    }

    //通道之间的数据传输(直接缓冲区)
    @Test
    public void test3() throws IOException {
        long start = System.currentTimeMillis();

        FileChannel inChannel = null;
        FileChannel outChannel = null;

        inChannel = FileChannel.open(Paths.get("d:/1.jpg"), StandardOpenOption.READ);
        outChannel = FileChannel.open(Paths.get("d:/2.jpg"), StandardOpenOption.READ, StandardOpenOption.WRITE, StandardOpenOption.CREATE);

        inChannel.transferTo(0, inChannel.size(), outChannel);
        outChannel.transferFrom(inChannel, 0, inChannel.size());
        long end = System.currentTimeMillis();
        System.out.println("耗费的时间: "+(end - start));
    }

    //分散读取与聚集写入
    @Test
    public void test4() throws IOException {
        long start = System.currentTimeMillis();
        RandomAccessFile raf1 = null;
        FileChannel channel1 = null;
        RandomAccessFile raf2 = null;
        FileChannel channel2 = null;

        raf1 = new RandomAccessFile("d:/1.txt", "rw");

        //1、获取通道
        channel1 = raf1.getChannel();

        //2、分配指定大小的缓冲区
        ByteBuffer buf1 = ByteBuffer.allocate(100);
        ByteBuffer buf2 = ByteBuffer.allocate(1024);

        // 分散读取
        ByteBuffer[] bufs = {buf1, buf2};
        channel1.read(bufs);

        for(ByteBuffer byteBuffer : bufs) {
            byteBuffer.flip();
        }
        System.out.println(new String(bufs[0].array(), 0, bufs[0].limit()));
        System.out.println("-------------------------");
        System.out.println(new String(bufs[1].array(), 0, bufs[1].limit()));

        //聚集写入
        raf2 = new RandomAccessFile("2.txt", "rw");
        channel2 = raf2.getChannel();
        channel2.write(bufs);
    }

    @Test
    public void test5() throws CharacterCodingException {
        Charset cs1 = Charset.forName("GBK");

        //获取编码器
        CharsetEncoder ce = cs1.newEncoder();

        //获取解码器
        CharsetDecoder cd = cs1.newDecoder();

        CharBuffer cBuf = CharBuffer.allocate(1024);
        cBuf.put("啦啦啦啦拉拉啊啦啦");
        cBuf.flip();

        ByteBuffer bBuf = null;

        bBuf = ce.encode(cBuf);
        for(int i =0; i < 12; ++i) {
            System.out.println(bBuf.get());
        }

        //解码
        bBuf.flip();
        CharBuffer cBuf2 = null;
        cBuf2 = cd.decode(bBuf);
        System.out.println(cBuf2.toString());

    }
}

```



### 二、NIO的非阻塞式网络短信

传统的 IO 流都是阻塞式的。也就是说，当一个线程调用 read() 或 write()时，该线程被阻塞，直到有一些数据被读取或写入，该线程在此期间不能执行其他任务。因此，在完成网络通信进行 IO 操作时，由于线程会阻塞，所以服务器端必须为每个客户端都提供一个独立的线程进行处理，当服务器端需要处理大量客户端时，性能急剧下降。

Java NIO 是非阻塞模式的。当线程从某通道进行读写数据时，若没有数据可用时，该线程可以进行其他任务。线程通常将非阻塞 IO 的空闲时间用于在其他通道上执行 IO 操作，所以单独的线程可以管理多个输入和输出通道。因此，NIO 可以让服务器端使用一个或有限几个线程来同时处理连接到服务器端的所有客户端。



#### 选择器(Selector)

选择器（Selector） 是 SelectableChannle 对象的多路复用器，Selector 可以同时监控多个 SelectableChannel 的 IO 状况，也就是说，利用 Selector可使一个单独的线程管理多个 Channel。Selector 是非阻塞 IO 的核心。  

``` java
package com.xiaopohai;

import org.junit.Test;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ServerSocket;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

/**
 * 一、使用NIO完成网络通信的三个核心：
 *  java.nio.channels.Channel接口：
 *  |--SelectableChannel
 *      |--SocketChannel
 *      |--ServerSocketChannel
 *      |--DatagramChannel
 *
 *      |--Pipe.SinkChannel
 *      |--Pipe.SourceChannel
 *
 * 2、缓冲区(Buffer)：负责数据的存取
 *
 * 3、选择器(Selector)：是SelectableChannel的多路复用器。用于监控SelectableChannel的IO状况
 */
public class Test03_BlockingNIO {   //没用Selector,阻塞式的
    @Test
    public void client() throws IOException {
        SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9898));
        FileChannel inChannel = FileChannel.open(Paths.get("d:/1.jpg"), StandardOpenOption.READ);

        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        while(inChannel.read(byteBuffer) != -1) {
            byteBuffer.flip();
            sChannel.write(byteBuffer);
            byteBuffer.clear();
        }
        sChannel.shutdownOutput();  //关闭发送通道，表名发送完毕

        //接收服务器的反馈
        int len = 0;
        while((len = sChannel.read(byteBuffer)) != -1) {
            byteBuffer.flip();
            System.out.println(new String(byteBuffer.array(), 0, len));
            byteBuffer.clear();
        }
        inChannel.close();
        sChannel.close();
    }

    //服务器
    @Test
    public void server() throws IOException {
        ServerSocketChannel ssChannel = ServerSocketChannel.open();
        FileChannel outChannel = FileChannel.open(Paths.get("2.jpg"), StandardOpenOption.READ, StandardOpenOption.WRITE, StandardOpenOption.CREATE);

        ssChannel.bind(new InetSocketAddress(9898));

        SocketChannel sChannel = ssChannel.accept();
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        while(sChannel.read(byteBuffer) != -1) {
            byteBuffer.flip();
            outChannel.write(byteBuffer);
            byteBuffer.clear();
        }

        //发送反馈给客户端
        byteBuffer.put("服务端接收数据成功".getBytes());
        byteBuffer.flip(); //转为读模式
        sChannel.write(byteBuffer);

        sChannel.close();
    }
}
```

#### SelectionKey

当调用register(Selector sel, int ops)将通道注册选择器时，选择器对通道的监听事件，需要通过第二个参数ops指定。  

可以监听的事件类型(用可使用SeletionKey的四个常量表示)：  

 读 : SelectionKey.OP_READ （1）
 写 : SelectionKey.OP_WRITE （4）
 连接 : SelectionKey.OP_CONNECT （8）
 接收 : SelectionKey.OP_ACCEPT （16）

若注册时不止监听一个事件，则可以使用"位或"操作符连接。  

SelectionKey：表示 SelectableChannel 和 Selector 之间的注册关系。每次向选择器注册通道时就会选择一个事件(选择键)。选择键包含两个表示为整数值的操作集。操作集的每一位都表示该键的通道所支持的一类可选择操作。  

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gceqr8vxnxj20ei06kabf.jpg)

**Selector的常用方法**

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gceqs5km4aj20h308u769.jpg)



``` java
package com.xiaopohai;

import org.junit.Test;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Date;
import java.util.Iterator;
import java.util.Scanner;

public class Test04_NonBlockingNIO {
    //客户端
    public static void main(String src[]) throws IOException {
        //1、获取通道
        SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9898));
        //2、切换非阻塞模式
        sChannel.configureBlocking(false);
        //3、分配指定大小的缓冲区
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        //4、发送数据给服务端
        Scanner scan = new Scanner(System.in);

        while(scan.hasNext()) {
            String str = scan.next();
            byteBuffer.put((new Date().toString() + "\n" + str).getBytes());
            byteBuffer.flip();
            sChannel.write(byteBuffer);
            byteBuffer.clear();
        }
        //关闭通道
        sChannel.close();
    }

    //服务器
    @Test
    public void server() throws IOException {
        //1、获取通道
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();

        //2、切换为非阻塞方式
        serverSocketChannel.configureBlocking(false);

        //3、绑定连接
        serverSocketChannel.bind(new InetSocketAddress(9898));

        //4、获取选择器
        Selector selector = Selector.open();

        //5、将通道注册到选择器上面，并且指定"监听接收事件"
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        //6、轮询式的获取选择器上已经"准备就绪"的事件
        while(selector.select() > 0) {
            //7、获取当前选择器中所有注册的"选择键(已就绪的监听事件)"
            Iterator<SelectionKey> it = selector.selectedKeys().iterator();
            while(it.hasNext()) {
                //8、获取准备"就绪"的事件
                SelectionKey selectionKey = it.next();
                //9、判断具体是什么时间准备就绪
                if(selectionKey.isAcceptable()) {

                    //10、若“接收就绪”，获取了客户端连接
                    SocketChannel socketChannel = serverSocketChannel.accept();

                    System.out.println("有新的连接建立： " + socketChannel.getRemoteAddress().toString());

                    //11、切换为非阻塞模式
                    socketChannel.configureBlocking(false);

                    //12、将该通道注册到选择器上
                    socketChannel.register(selector, SelectionKey.OP_READ);
                } else if(selectionKey.isReadable()) {
                    //13、获取当前选择器上“读就绪”状态的通道
                    SocketChannel socketChannel = (SocketChannel)selectionKey.channel();
                    //14、读取数据
                    ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
                    int len = 0;
                    while((len = socketChannel.read(byteBuffer)) > 0) {
                        byteBuffer.flip();
                        System.out.println(new String(byteBuffer.array(), 0, len));
                        byteBuffer.clear();
                    }
                }
                //15、取消选择键SelectionKey
                it.remove();
            }
        }
    }
}
```

**DatagramChannel**

Java NIO中的DatagramChannel是一个能收发UDP包的通道  

``` java
package com.xiaopohai;

import org.junit.Test;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.DatagramChannel;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.util.Iterator;
import java.util.Date;
import java.util.Scanner;

public class Test05_NonBlockNIO2 {

    public static void main(String[] args) throws IOException {
        DatagramChannel datagramChannel = DatagramChannel.open();
        datagramChannel.configureBlocking(false);
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        Scanner scanner = new Scanner(System.in);
        while(scanner.hasNext()) {
            String str = scanner.next();
            byteBuffer.put((new Date().toString()+"\n"+str).getBytes());
            byteBuffer.flip();
            datagramChannel.send(byteBuffer, new InetSocketAddress("127.0.0.1", 9898));
            byteBuffer.clear();
        }
        datagramChannel.close();
    }



    @Test
    public void receive() throws IOException {
        DatagramChannel datagramChannel = DatagramChannel.open();
        datagramChannel.configureBlocking(false);
        datagramChannel.bind(new InetSocketAddress(9898));
        Selector selector = Selector.open();
        datagramChannel.register(selector, SelectionKey.OP_READ);
        while(selector.select() > 0) {
            Iterator<SelectionKey> selectionKeyIterable = selector.selectedKeys().iterator();
            while(selectionKeyIterable.hasNext()) {
                SelectionKey selectionKey = selectionKeyIterable.next();
                if(selectionKey.isReadable()) {
                    ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
                    datagramChannel.receive(byteBuffer);

                    byteBuffer.flip();
                    System.out.println(new String(byteBuffer.array(), 0, byteBuffer.limit()));
                    byteBuffer.clear();
                }
            }
            selectionKeyIterable.remove();
        }
    }
}
```

#### 管道

Java NIO管道是2个线程之间的单向数据连接。Pipe有一个source通道和一个sink通道。数据会被写入到sink通道，从sour通道读取。  

![undefined](G:\xiaopohai\xiaopohai_blog\_media\JavaNIO\bc370e19gy1gcesgvee94j20c103s3z7.jpg)

``` java
package com.xiaopohai;

import org.junit.Test;

import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.Pipe;

public class Test06_Pipe {
    @Test
    public void test1() throws IOException {
        //1、获取管道
        Pipe pipe = Pipe.open();
        //2、将缓冲区中的数据写入管道
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        Pipe.SinkChannel sinkChannel = pipe.sink();
        byteBuffer.put("通过单向管道发送数据".getBytes());
        byteBuffer.flip();
        sinkChannel.write(byteBuffer);

        //3、读取缓冲区中的数据
        Pipe.SourceChannel sourceChannel = pipe.source();
        byteBuffer.flip();
        int len = sourceChannel.read(byteBuffer);
        System.out.println(new String(byteBuffer.array(), 0, len));

        sourceChannel.close();
        sinkChannel.close();
    }
}
```

### 三、NIO.2 -Path、Paths、Files

#### Path与Paths

+ java.nio.file.Path 接口代表一个平台无关的平台路径，描述了目录结构中文件的位置。
+ Paths 提供的 get() 方法用来获取 Path 对象：Path get(String first, String … more) : 用于将多个字符串串连成路径
+ Path的常用方法：  
  + boolean endsWith(String path) : 判断是否以 path 路径结束
  + boolean startsWith(String path) : 判断是否以 path 路径开始
  + boolean isAbsolute() : 判断是否是绝对路径
  + Path getFileName() : 返回与调用 Path 对象关联的文件名
  + Path getName(int idx) : 返回的指定索引位置 idx 的路径名称
  + int getNameCount() : 返回Path 根目录后面元素的数量
  + Path getParent() ：返回Path对象包含整个路径，不包含Path 对象指定的文件路径
  + Path getRoot() ：返回调用 Path 对象的根路径
  + Path resolve(Path p) :将相对路径解析为绝对路径
  + Path toAbsolutePath() : 作为绝对路径返回调用 Path 对象
  + String toString() ： 返回调用 Path 对象的字符串表示形式

####  Files类

java.nio.file.Files 用于操作文件或目录的工具类。  

+ Files常用方法：
  + Path copy(Path src, Path dest, CopyOption … how) : 文件的复制
  + Path createDirectory(Path path, FileAttribute< ? > … attr) : 创建一个目录
  + Path createFile(Path path, FileAttribute< ? > … arr) : 创建一个文件
  + void delete(Path path) : 删除一个文件
  + Path move(Path src, Path dest, CopyOption…how) : 将 src 移动到 dest 位置
  + long size(Path path) : 返回 path 指定文件的大小
+ Files常用方法：用于判断
  + boolean exists(Path path, LinkOption … opts) : 判断文件是否存在
  + boolean isDirectory(Path path, LinkOption … opts) : 判断是否是目录
  + boolean isExecutable(Path path) : 判断是否是可执行文件
  + boolean isHidden(Path path) : 判断是否是隐藏文件
  + boolean isReadable(Path path) : 判断文件是否可读
  + boolean isWritable(Path path) : 判断文件是否可写
  + boolean notExists(Path path, LinkOption … opts) : 判断文件是否不存在
  + public static < A extends BasicFileAttributes> A readAttributes(Path path,Class< A > type,LinkOption…options) : 获取与 path 指定的文件相关联的属性。
+ Files常用方法：用于操作内容
  + SeekableByteChannel newByteChannel(Path path, OpenOption…how) : 获取与指定文件的连接，how 指定打开方式。
  + DirectoryStream newDirectoryStream(Path path) : 打开 path 指定的目录
  + InputStream newInputStream(Path path, OpenOption…how):获取 InputStream 对象
  + OutputStream newOutputStream(Path path, OpenOption…how) : 获取 OutputStream 对象