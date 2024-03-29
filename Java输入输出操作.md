# Java输入输出操作

​	介绍Java中用于输入和输出的各种API。

## 输入输出流

​	在Java中，可以从其中读入一个**字节序列**的**对象**称为输入流，可以向其中写一个字节序列的对象称为输出流。这些字节序列的来源可以是文件、网络连接、内存块等。抽象类InputStream和OutputStream构成了输入/输出（I\O）类层次结构的基础。字节流一般以Stream结尾。

​	因为面向字节流不便于处理**Unicode(字符)**形式存储的信息，因此从抽象类Reader和Writer中继承出来一个专门用于处理Unicode字符的单独的类层次结构。这些类拥有的读入和写出操作都是基于两字节char（char类型就是专门用来存储Unicode编码）值的，而不是byte流。字符流一般以Reader/Writer结尾。

### 读写字节

​	InputStream类有一个抽象方法：

·	abstract int read();

​	这个方法将读入一个字节，并返回读入的字节，或者在遇到输入源结尾时返回-1。InputStream还有若干个非抽象方法，可以读入一个字节数组，或者跳过大量字节。

​	类似的，OutputStream中也有向某个输出位置写入一个字节和字节数组的方法。

​	transferto()方法可以将所有字节从一个输入流传递到一个输出流

```java
in.transferto(out);
```

​	read和write方法在执行时都将阻塞，直至字节操作完成。这意味着如果流不能立即访问（网络等因素），那么当前线程将被阻塞。

​	avaliable方法可以检查当前可读入字节数量，在读之前判断一下字节是否可读就可以避免没有字节读入而阻塞。

​	完成读写操作之后应该调用close方法关闭流，释放操作系统的资源。关闭一个输出流的同时还会冲刷用于该输出流的缓冲区：因为字节会在缓冲区中等待形成更大的包，若不冲刷，则写出字节的最后一个包可能永远也得不到传递，因此，使用完一个流应该直接关闭以冲刷缓冲区，也可以人为手动的冲刷缓冲区

### 读写字符

​	和读写字符一样，Reader::read()将读入一个字符，输入流结束时返回-1，Reader::Writer()