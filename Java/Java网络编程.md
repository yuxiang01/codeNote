## 一、网络的相关概念
> [!note]- 1.1 网络通信
> 概念: 两台设备之间通过网络实现数据传输
> 网路通信: 将数据通过网络从一台设备传输到另一台设备
> `java.net` 包下提供了一系列的类/接口, 完成网络通信

> [!note]- 1.2 网络
> ![[Pasted image 20230427175944.png]]

> [!note]- 1.3 ip 地址
> ![[Pasted image 20230427191131.png]]

> [!note]- 1.4 ipv4 地址分类
> ![[Pasted image 20230427191611.png]]

> [!note]- 1.5 域名
> ![[Pasted image 20230427191648.png]]

> [!note]- 1.6 网络通信协议
> ![[Pasted image 20230427192202.png]]

> [!note]- 1.7 网络通信协议
> ![[Pasted image 20230427192425.png]]

> [!note]- 1.8 TCP 和 UDP
> ![[Pasted image 20230427193746.png]]

## 二、InetAddress 类
```java
//获取本机 InetAddress 对象 getLocalHost 
InetAddress localHost = InetAddress.getLocalHost(); 
System.out.println(localHost); 

//根据指定主机名/域名获取 ip 地址对象 getByName 
InetAddress host2 = InetAddress.getByName("ThinkPad-PC"); 
System.out.println(host2); 
InetAddress host3 = InetAddress.getByName("www.hsp.com"); 
System.out.println(host3);

//获取 InetAddress 对象的主机名 getHostName 
String host3Name = host3.getHostName(); 
System.out.println(host3Name); 

//获取 InetAddress 对象的地址 getHostAddress 
String host3Address = host3.getHostAddress(); 
System.out.println(host3Address);
```

## 三、Socket 类
1. `套接字(Socket)` 开发网络应用程序被广泛采用, 以至于成为事实上的标准 
2. 通信两端都要有 `Socket`,即两台机器间通信的端点
3. `Socket` 允许程序把网络连接当作一个流, 数据在两个 `Socket` 间通过 IO 传输
4. 一般把<font color="#4bacc6">主动发起通信</font>的应用程序属<font color="#4bacc6">客户端</font>,<font color="#f79646">等待通信请求</font>的属<font color="#f79646">服务端</font>

![[Pasted image 20230427194056.png]]

## 四、TCP 网络通信编程
1. 基于客户端——服务端的网络通信
2. 底层使用的是 TCP/IP 协议
![[Pasted image 20230428091036.png]]

>[!note]- 案例: 单向通信 (使用字节流)
> 服务端监听 9999 端口, 接受客户端信息
> ```java
> public class SocketServer {
>   public static void main(String[] args) throws IOException {
>     // 监听9999端口,要求监听端口不能被占用
>     ServerSocket serverSocket = new ServerSocket(9999);
>     // 当客户端连接9999端口时,则返回Socket对象,程序继续执行
>     // 当没有连接时,程序会阻塞,直至有连接
>     Socket socket = serverSocket.accept();
>     System.out.println("服务端 socket = " + socket.getClass());
>     InputStream inputStream = socket.getInputStream();
>     byte[] bytes = new byte[1024];
>     int readLen = 0;
>     while ((readLen = inputStream.read(bytes)) != -1) {
>       System.out.println(new String(bytes, 0, readLen));
>     }
>     inputStream.close();
>     socket.close();
>   }
> }
> ```
> 客户端发送信息
> ```java
> public class SocketClient {
>   public static void main(String[] args) throws IOException {
>     Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
>     System.out.println("客户端, socket返回 = " + socket.getClass());
>     // 得到和socket关联的 输出流对象
>     OutputStream outputStream = socket.getOutputStream();
>     // 通过输出流,写入数据到数据通道
>     outputStream.write("Hello, Server!".getBytes());
>     outputStream.flush();
>     // 关闭流和socket对象
>     outputStream.close();
>     socket.close();
>     System.out.println("客户端服务Over...");
>   }
> }
> ```

> [!note]- 案例: 双向通信 
> 服务端接受信息, 并回复信息
> ```java
> public class SocketServer {
>   public static void main(String[] args) throws IOException {
>     // 监听9999端口,要求监听端口不能被占用
>     ServerSocket serverSocket = new ServerSocket(9999);
>     // 当客户端连接9999端口时,则返回Socket对象,程序继续执行
>     // 当没有连接时,程序会阻塞,直至有连接
>     Socket socket = serverSocket.accept();
>     System.out.println("服务端 socket = " + socket.getClass());
>     InputStream inputStream = socket.getInputStream();
>     byte[] bytes = new byte[1024];
>     int readLen = 0;
>     while ((readLen = inputStream.read(bytes)) != -1) {
>       System.out.println(new String(bytes, 0, readLen));
>     }
>     OutputStream outputStream = socket.getOutputStream();
>     outputStream.write("hello, client".getBytes());
>     // 此处可以不设置结束点,当输出这句话后资源就关闭了
>     // socket.shutdownOutput();
>     outputStream.flush();
>     outputStream.close();
>     inputStream.close();
>     socket.close();
>   }
> }
> ```
> 客户端发送信息并接受信息
> ```java
> public class SocketClient {
>   public static void main(String[] args) throws IOException {
>     Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
>     System.out.println("客户端 socket = " + socket.getClass());
>     // 得到和socket关联的 输出流对象
>     OutputStream outputStream = socket.getOutputStream();
>     // 通过输出流,写入数据到数据通道
>     outputStream.write("Hello, Server!".getBytes());
>     outputStream.flush();
>     // 设置结束标识
>     socket.shutdownOutput();
>     // 获取和socket相关的 输入流对象
>     InputStream inputStream = socket.getInputStream();
>     byte[] bytes = new byte[1024];
>     int readLen = 0;
>     while ((readLen = inputStream.read(bytes)) != -1) {
>       System.out.println(new String(bytes, 0, readLen));
>     }
>     // 关闭流和socket对象
>     outputStream.close();
>     socket.close();
>   }
> }
> ```

> [!note]- 案例: 网络上传图片
> - 流程实例图分析:
> 	![[socket网络文件上传.excalidraw]]
> - 服务端
> ```java
> public class Server {
>   public static void main(String[] args) throws IOException {
>     // 监听9999端口
>     ServerSocket serverSocket = new ServerSocket(9999);
>     System.out.println("Listen to port 9999......");
>     // 等待连接
>     Socket socket = serverSocket.accept();
> 
>     // 读取客户端发送的数据
>     // 通过socket得到输入流
>     BufferedInputStream reader = new BufferedInputStream(socket.getInputStream());
>     byte[] bytes = StreamUtils.streamToByteArray(reader);
> 
>     // 将字节数组bytes[],写入指定路径
>     String path = Server.class.getResource("/").getPath()
>         + UUID.randomUUID().toString().substring(1, 5) + ".png";
>     BufferedOutputStream writer = new BufferedOutputStream(new FileOutputStream(path));
>     writer.write(bytes);
>     writer.flush();
> 
>     // 回复: “收到图片”
>     BufferedWriter bw =
>         new BufferedWriter(new OutputStreamWriter
>             (socket.getOutputStream(), StandardCharsets.UTF_8));
>     bw.write("收到图片!");
>     bw.flush();
>     socket.shutdownOutput();
> 
>     // 关闭相关流
>     bw.close();
>     writer.close();
>     reader.close();
>     socket.close();
>     serverSocket.close();
>   }
> }
> ```
> - 客户端
> ```java
>   public static void main(String[] args) throws IOException {
>     // 客户端连接至服务端 9999
>     Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
> 
>     // 创建读取磁盘文件的输入流
>     String path = "/Users/fishx/Desktop/media/axia.png";
>     BufferedInputStream reader = new BufferedInputStream(new FileInputStream(path));
> 
>     // 将输入流内容转成byte[]
>     byte[] bytes = StreamUtils.streamToByteArray(reader);
> 
>     // 通过socket获取到输出流,将bytes[]数据发送给服务端
>     BufferedOutputStream writer = new BufferedOutputStream(socket.getOutputStream());
>     writer.write(bytes);
>     writer.flush();
> 
>     // 设置写入数据的结束点,避免IO阻塞问题
>     socket.shutdownOutput();
> 
>     // 接收服务端返回的信息
>     String content = StreamUtils.streamToString(socket.getInputStream());
>     System.out.println("客户端 收到服务端 回复: " + content);
> 
>     // 关闭流
>     writer.close();
>     reader.close();
>     socket.close();
>   }
> }
> ```
> - 工具类
> ```java
> public class StreamUtils {
>   public static byte[] streamToByteArray(InputStream is) throws IOException {
>     // 创建输出流对象
>     ByteArrayOutputStream bos = new ByteArrayOutputStream();
>     // 字节数组作缓冲区
>     byte[] bytes = new byte[1024];
>     int len;
>     while ((len = is.read(bytes)) != -1) {
>       // 将输入流的内容 ->写入->  输出流对象中
>       bos.write(bytes, 0, len);
>     }
>     // 转成byte[]返回
>     byte[] byteArray = bos.toByteArray();
>     bos.close();
>     return byteArray;
>   }
> 
>   public static String streamToString(InputStream is) throws IOException {
>     BufferedReader reader = new BufferedReader(new InputStreamReader(is));
>     StringBuilder builder = new StringBuilder();
>     String line;
>     while ((line = reader.readLine()) != null) {
>       builder.append(line);
>     }
>     return builder.toString();
>   }
> }
> ```

> [!note]- netstat 指令
> 1. `netstat -an` 可以查看当前主机网络情况, 包括端口监听情况和网络连接情况
> 2. `netstat -an|more` 可以分页显示
> 3. 如果有一个外部程序 (客户端)连接到该端口, 就会显示一条连接信息

- 当客户端连接到服务端后
	- 实际上客户端也是通过一个端口和服务端进行通讯的
	- 这个端口是 TCP/IP 来随机分配的, 是不确定的

## 五、UPD 网络通信编程
1. `DatagramSocket` 和 `DatagramPacket` 类 (数据包/数据报)实现了基于 UDP 协议网络程序
2. UDP 数据报通过数据报套接字 `DatagramSocket` 发送和接收
	- 系统不保证 UDP 数据报一定能够安全送到目的地
	- 也不能确定什么时候可以抵达
3. `DatagramPacket` 对象封装了 UDP 数据报
	- 在数据报中包含了发送端的 ip 地址和端口号
	- 以及接收端的 ip 地址和端口号
4. UDP 协议每个数据报都给出了完整的地址信息
	- 因此无须建立发送方和接收方的连接

> [!note]- 基本流程
> 1. 核心类: `DatagramSocket`、`DatagramPacket`
> 2. 建立发送端, 接收端 (没有服务端和客户端概念)
> 3. 发送数据前, 建立数据包/报 `DatagramPacket` 对象
> 4. 调用 `DatagramSocket` 的发送、接收方法
> 5. 关闭 `DatagramPacket`
> 
> ![[Pasted image 20230429223733.png]]

>[!example]- 案例: 接发信息
> - PortA
> 	```java
> 	public class PortA { 
> 	  public static void main(String[] args) throws IOException {
> 	    // 1. 创建一个 DatagramSocket 对象,准备在9999端口接收数据
> 	    DatagramSocket socket = new DatagramSocket(9999);
> 	    // 2. 构建一个 DatagramPacket 对象,准备接收数据
> 	    byte[] bytes = new byte[64 * 1024];
> 	    DatagramPacket packet = new DatagramPacket(bytes, bytes.length);
> 	    // 3. 调用 接收方法,将通过网络传输的 DatagramPacket 对象填充到packet对象
> 	    // System.out.println("receiver 等待接收数据...");
> 	    socket.receive(packet);
> 	    // 4. 可以把packet进行拆包,取出数据,并显示
> 	    int length = packet.getLength(); // 实际上接收到的数据字节长度
> 	    byte[] data = packet.getData(); // 接收到的数据
> 	    String content = new String(data, 0, length);
> 	    System.out.println("接收信息: " + content);
> 	
> 	    // 回复信息
> 	    data = "好的,明天见".getBytes();
> 	    packet = new DatagramPacket(data, data.length, InetAddress.getLocalHost(), 9998);
> 	    socket.send(packet);
> 	
> 	    // 5. 关闭资源
> 	    socket.close();
> 	  }
> 	}
> 	```
> - PortB
> 	```java
> 	public class PortB {
> 	  public static void main(String[] args) throws IOException {
> 	    // 1. 创建 DatagramSocket 对象,准备在9998端口接收数据
> 	    DatagramSocket socket = new DatagramSocket(9998);
> 	    // 2. 将需要发送的数据,封装到DatagramPackct对象
> 	    byte[] data = "Hello,明天吃火锅".getBytes();
> 	
> 	    // 封装 DatagramPacket 对象(data内容字节数组,长度,主机ip,端口)
> 	    // ip: InetAddress.getByName("")
> 	    DatagramPacket packet = new DatagramPacket(data, data.length,
> 	        InetAddress.getLocalHost(), 9999);
> 	    socket.send(packet);
> 	
> 	    // 接收信息
> 	    byte[] bytes = new byte[64 * 1024];
> 	    packet = new DatagramPacket(bytes, bytes.length);
> 	    socket.receive(packet);
> 	    data = packet.getData();
> 	    System.out.println("接收信息: " + new String(data, 0, packet.getLength()));
> 	
> 	    // 3. 关闭资源
> 	    socket.close();
> 	  }
> 	}
> 	```

