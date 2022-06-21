# TCP/IP

1. Ip地址
2. 端口号

> TCP/IP协议在java中的体现为Socket对象，有IP地址和端口号

服务器：ServerSocket

客户端：Socket  

三次握手之后就会建立逻辑连接，这个连接包含一个IO对象，可以使用IO对象来进行通信。

> 使用OutPutStream来发送数据，InPutStream接受数据

ServerSocket::accept()方法可以获得一个Socket对象，服务器没有IO流，每次接受一个请求都会有一个Socket对象，使用该Socket对象的IO流来进行数据交互，即服务器使用客户端的流和客户端交互。 

```java
Socket socket = ServerSocket.accept();·
```

### TCP客户端

​	向服务器发送连接请求，给服务器发送数据，读取服务器返回的数据。

​	java.net.Socket：此类实现客户端套接字。套接字是两台机器通信的端点，包含了Ip地址和端口号的网络单位。

构造方法：

- Socket(String host,int port)创建一个流套接字并将其连接到指定主机上的指定端口号。

成员方法：

- OutPutStream getOutPutStream() 返回此套接字的输出流
- InPutStream getInPutStream() 返回此套接字的输入流
- void close() 关闭此套接字

实现步骤：

1. 创建一个客户端对象Socket，构造方法绑定服务器的IP地址和端口号
2. 使用Socket::getOutPutStream()获取网络字节输出流OutPutStream对象
3. 使用上一步OutPutStream::write()给服务器发送数据
4. 使用Socket::getInPutStream()获取网络字节输入流InPutStream对象
5. 使用上一步InPutStream对象接受服务器回写的数据
6. 释放资源（关闭Socket）

注意事项：

1. 客户端和服务器端交互，必须使用Socket中提供的流，不能使用自己创建的流
2. 创建Socket对象时，就相当于与服务器进行了三次握手建立了连接，若无法建立连接则抛出异常。

### TCP上传文件

###### 原理：客户端读取本地文件，转化为流，发送到服务器端，服务器端读取流，并转化为文件保存到服务器本地。

#### 实现步骤

1. 客户端使用本地的字节流读取文件
2. 客户端使用socket提供的输出流传输到服务器端
3. 服务器端使用socket提供的出入流接收文件
4. 服务器端使用本地字节输出流将文件保存到服务器上