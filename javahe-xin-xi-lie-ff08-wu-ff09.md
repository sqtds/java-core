# java I/O

## 一，概述 {#一，概述}

[![](http://sqtds.github.io/img/2015/I.O.png)](http://sqtds.github.io/img/2015/I.O.png)

## 二，BIO {#二，BIO}

java I/O主要分为下面这几类：

* 基于字节操作的 I/O 接口：InputStream 和 OutputStream
* 基于字符操作的 I/O 接口：Writer 和 Reader
* 基于磁盘操作的 I/O 接口：File
* 基于网络操作的 I/O 接口：Socket

字符与字节：

1. InputStream 类以二进制输入/输出，I/O速度快且效率搞，但是它的read（）方法读到的是一个字节（二进制数据），很不利于人们阅读，而且无法直接对文件中的字符进行操作，比如替换，查找（必须以字节形式操作）；
   而Reader类弥补了这个缺陷，可以以文本格式输入/输出，非常方便；比如可以使用while\(\(ch = filereader.read\(\)\)!=-1 \)循环来读取文件；可以使用BufferedReader的readLine\(\)方法一行一行的读取文本。
2. InputStreamReader 是字节流通向字符流的桥梁：它使用指定的 charset 读取字节并将其解码为字符。它使用的字符集可以由名称指定或显式给定，或者可以接受平台默认的字符集。
3. BufferedReader的最大特点就是缓冲区的设置。BufferReader类用来包装所有其 read\(\) 操作可能开销很高的 Reader（如 FileReader 和InputStreamReader）。

## 三，NIO {#三，NIO}

nio基于块，io基于流。NIO 将最耗时的 I/O 操作\(即填充和提取缓冲区\)转移回操作系统，因而可以极大地提高速度。

* 面向流的I/O系统一次一个字节地处理数据。一个输入流产生一个字节的数据，一个输出流消费一个字节的数据。为流式数据创建过滤器非常容易。链接几个过滤器，以便每个过滤器只负责单个复杂处理机制的一部分，这样也是相对简单的。不利的一面是，面向流的 I/O 通常相当慢。
* 面向块的I/O系统以块的形式处理数据。每一个操作都在一步中产生或者消费一个数据块。按块处理数据比按\(流式的\)字节处理数据要快得多。但是面向块的 I/O 缺少一些面向流的 I/O 所具有的优雅性和简单性。

### 1，Buffer {#1，Buffer}

#### 1.1 Buffer类型 {#1-1_Buffer类型}

对于每一种基本 Java 类型都有一种缓冲区类型：

* ByteBuffer
* CharBuffer
* ShortBuffer
* IntBuffer
* LongBuffer
* FloatBuffer
* DoubleBuffer

#### 1.2 Buffer中的参数项: {#1-2_Buffer中的参数项:}

* capacity 缓冲区数组的总长度
* position 下一个要操作的数据元素的位置
* limit 缓冲区数组中不可操作的下一个元素的位置，limit&lt;=capacity
* mark 用于记录当前 position 的前一个位置或者默认是 0
 
  [![](http://sqtds.github.io/img/2015/buffer.jpeg)](http://sqtds.github.io/img/2015/buffer.jpeg)

#### 1.3 Buffer中的常用方法 {#1-3_Buffer中的常用方法}

```
ByteBuffer buf = ByteBuffer.allocate(48); // create buffer

buf.flip();  //make buffer ready for read
buf.clear(); //make buffer ready for writing
buf.compact();//make buffer ready for writing,copies all unread data to the beginning of the Buffer

buf.rewind();//sets the position back to 0, so you can reread all the data in the buffer. 

buf.mark() ;//mark a given position 
buf.reset();//reset the position back to the marked position
```

[![](http://sqtds.github.io/img/2015/buffers-modes.png)](http://sqtds.github.io/img/2015/buffers-modes.png)

#### 1.4 缓冲区分片 {#1-4_缓冲区分片}

slice\(\) 方法根据现有的缓冲区创建一种子缓冲区 。也就是说，它创建一个新的缓冲区，新缓冲区与原来的缓冲区的一部分共享数据。

#### 1.5 直接和间接缓冲区 {#1-5_直接和间接缓冲区}

直接缓冲区 是为加快 I/O 速度，而以一种特殊的方式分配其内存的缓冲区。给定一个直接字节缓冲区，Java 虚拟机将尽最大努力直接对它执行本机 I/O 操作。也就是说，它会在每一次调用底层操作系统的本机 I/O 操作之前\(或之后\)，尝试避免将缓冲区的内容拷贝到一个中间缓冲区中\(或者从一个中间缓冲区中拷贝数据\)。

#### 1.6 内存映射文件 I/O {#1-6_内存映射文件_I/O}

内存映射文件 I/O 是一种读和写文件数据的方法，它可以比常规的基于流或者基于通道的 I/O 快得多。一般来说，只有文件中实际读取或者写入的部分才会送入（或者 映射 ）到内存中。

现代操作系统一般根据需要将文件的部分映射为内存的部分，从而实现文件系统。Java 内存映射机制不过是在底层操作系统中可以采用这种机制时，提供了对该机制的访问。

### 2，Channel {#2，Channel}

Channel是一个对象，可以通过它读取和写入数据。举个例子，channel就马路，buffer就是汽车，数据就是装在汽车上的沙子。  
通道与流的不同之处在于通道是双向的。而流只是在一个方向上移动\(一个流必须是 InputStream 或者 OutputStream 的子类\)， 而 通道 可以用于读、写或者同时用于读写。

#### 2.1 Channel的类型 {#2-1_Channel的类型}

* FileChannel //文件
* DatagramChannel //UDP
* SocketChannel //TCP
* ServerSocketChannel //TCP服务端

#### 2.2 FileChannel {#2-2_FileChannel}

FileChannel可以实现从Channel到Channel间的直接数据传输.

* FileChannel.transferFrom\(\) 将数据从来源channel传输过来。
* FileChannel.transferTo\(\) 将数据传输到其他的channel中。

#### 2.3 SocketChannel {#2-3_SocketChannel}

SocketChannel是连接tcp网络的通道。其操作模式如下：

```
SocketChannel socketChannel = SocketChannel.open();
socketChannel.connect(new InetSocketAddress("localhost", 80));
ByteBuffer buf = ByteBuffer.allocate(48);
int bytesRead = socketChannel.read(buf);

ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());
buf.flip();
while(buf.hasRemaining()) {
    channel.write(buf);
}

```

#### 2.4 ServerSocketChannel {#2-4_ServerSocketChannel}

监听tcp连接。

```
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
serverSocketChannel.socket().bind(new InetSocketAddress(9999));
serverSocketChannel.configureBlocking(false);
while(true){
    SocketChannel socketChannel =
            serverSocketChannel.accept();
    //do something with socketChannel...
}

```

#### 2.5 DatagramChannel {#2-5_DatagramChannel}

UDP连接

#### 2.6 Pipe {#2-6_Pipe}

管道是一种线程之间进行数据交换的方式。  
Pipe有一个source channel 和一个sink channel.你可以写数据到sink channel中，从source channel中读取数据.  
[![](http://sqtds.github.io/img/2015/pipe-internals.png)](http://sqtds.github.io/img/2015/pipe-internals.png)

```
Pipe pipe = Pipe.open();
Pipe.SinkChannel sinkChannel = pipe.sink();
sinkChannel.write(buf);
Pipe.SourceChannel sourceChannel = pipe.source();
inChannel.read(buf);

```

#### 2.7 分散和聚集 {#2-7_分散和聚集}

分散/聚集 I/O 是使用多个而不是单个缓冲区来保存数据的读写方法。  
在 分散读取 中，通道依次填充每个缓冲区。填满一个缓冲区后，它就开始填充下一个。在某种意义上，缓冲区数组就像一个大缓冲区。

接口：

```
ScatteringByteChannel
long read( ByteBuffer[] dsts );
long read( ByteBuffer[] dsts, int offset, int length );

GatheringByteChannel
long write( ByteBuffer[] srcs );
long write( ByteBuffer[] srcs, int offset, int length );
```

分散/聚集 I/O 对于将数据划分为几个部分很有用。例如，您可能在编写一个使用消息对象的网络应用程序，每一个消息被划分为固定长度的头部和固定长度的正文。您可以创建一个刚好可以容纳头部的缓冲区和另一个刚好可以容纳正文的缓冲区。当您将它们放入一个数组中并使用分散读取来向它们读入消息时，头部和正文将整齐地划分到这两个缓冲区中。

### 3，Selector {#3，Selector}

selector用于多路复用，可以同时监听一组通信信道（Channel）上的 I/O 状态。

传统的套接字服务器的处理方式是对于每一个客户端套接字连接，都新创建一个线程来进行处理。创建线程是很耗时的操作，而有的实现会采用线程池。不过一个请求一个线程的处理模型并不是很理想。原因在于耗费时间创建的线程，在大部分时间可能处于等待的状态。而多路复用I/O的基本做法是由一个线程来管理多个套接字连接。该线程会负责根据连接的状态，来进行相应的处理。多路复用I/O依靠操作系统提供的select或相似系统调用的支持，选择那些已经就绪的套接字连接来处理。

```
Selector selector = Selector.open();
channel.configureBlocking(false);
SelectionKey key = channel.register(selector, SelectionKey.OP_READ);

while(true) {
  int readyChannels = selector.select();
  if(readyChannels == 0) continue;
  Set<SelectionKey> selectedKeys = selector.selectedKeys();
  Iterator<SelectionKey> keyIterator = selectedKeys.iterator();
  while(keyIterator.hasNext()) {
    SelectionKey key = keyIterator.next();
    if(key.isAcceptable()) {
        // a connection was accepted by a ServerSocketChannel.
    } else if (key.isConnectable()) {
        // a connection was established with a remote server.
    } else if (key.isReadable()) {
        // a channel is ready for reading
    } else if (key.isWritable()) {
        // a channel is ready for writing
    }
    keyIterator.remove();
  }
}
```

## 四，AIO {#四，AIO}

## 五，其他 {#五，其他}

### 1，同步与异步 {#1，同步与异步}

所谓同步就是一个任务的完成需要依赖另外一个任务时，只有等待被依赖的任务完成后，依赖的任务才能算完成，这是一种可靠的任务序列。要么成功都成功，失败都失败，两个任务的状态可以保持一致。而异步是不需要等待被依赖的任务完成，只是通知被依赖的任务要完成什么工作，依赖的任务也立即执行，只要自己完成了整个任务就算完成了。至于被依赖的任务最终是否真正完成，依赖它的任务无法确定，所以它是不可靠的任务序列。

### 2，阻塞与非阻塞 {#2，阻塞与非阻塞}

阻塞与非阻塞主要是从 CPU 的消耗上来说的，阻塞就是 CPU 停下来等待一个慢的操作完成 CPU 才接着完成其它的事。非阻塞就是在这个慢的操作在执行时 CPU 去干其它别的事，等这个慢的操作完成时，CPU 再接着完成后续的操作。虽然表面上看非阻塞的方式可以明显的提高 CPU 的利用率，但是也带了另外一种后果就是系统的线程切换增加。

### 3，网络I/O优化 {#3，网络I/O优化}

1. 一个是减少网络交互的次数，缓存。
2. 减少网络传输数据量的大小，压缩。
3. 尽量减少编码，最好直接以字节发送。
4. 同步异步，阻塞非阻塞的选择。

## 六，参考资料 {#六，参考资料}

1. [深入分析 Java I/O 的工作机制](http://www.ibm.com/developerworks/cn/java/j-lo-javaio/)
2. [Java深度历险（八）——Java I/O](http://www.infoq.com/cn/articles/cf-java-i-o)
3. [Java I/O底层是如何工作的？](http://www.importnew.com/14111.html)
4. [Java NIO Overview](http://tutorials.jenkov.com/java-nio/overview.html)
5. [NIO 入门](http://www.ibm.com/developerworks/cn/education/java/j-nio/section2.html)

  




