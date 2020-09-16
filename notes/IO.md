

网络编程的本质就是进程间通信

通信的基础就是IO模型

# java.io基础

四个抽象类

![image-20200907231806322](IO.assets/image-20200907231806322.png)

## 字符流

![image-20200907231817398](IO.assets/image-20200907231817398.png)

下面的需要在另外的字符流上加功能，创建的时候需要传入另外的字符流

![image-20200907232104617](IO.assets/image-20200907232104617.png)



## 字节流

![image-20200907235947908](IO.assets/image-20200907235947908.png)

最后四个实现，创建时需要传入字节流实例

![image-20200908000108248](IO.assets/image-20200908000108248.png)



## 装饰器模式

![image-20200908000503110](IO.assets/image-20200908000503110.png)

就像在FileInputStream外面装饰了Buffer



## Socket

![image-20200908131154083](IO.assets/image-20200908131154083.png)

socket是网络通信的端点

### Unix中的Socket

- Unix系统中，一切皆是文件
- 打开文件就会有一个文件描述符。文件描述符表时已打开文件的索引
- 每个进程都会维护一个文件描述符表

### 通过Socket发送数据

- 应用进程创建Socket实例
- 为Socket绑定目标ip地址和端口（告诉驱动程序把这些配置绑定到Socket实例上）
- 向Socket实例中写入数据
- 驱动程序收到Socket传来的数据，然后让数据从硬件层面上发送出去

### 通过Socket发送数据

- 应用进程创建服务端Socket实例
- 为Socket绑定ip地址和端口
- 从Socket实例中读取数据



## 同步异步阻塞非阻塞

- 同步异步：强调被调用方
  - 同步：被调用之后要把自己的任务做完，然后返回结果。
  - 异步：被调用后先返回一个暂时的结果，自己的任务之后继续做，做完后去通知调用者。
- 阻塞非阻塞：强调调用方
  - 阻塞：调用其他方法之后，自己必须等待调用结果返回，才去做接下来的事。
  - 非阻塞：调用其他方法之后，不管有没有结果，接着做接下来的事。



## socket是协议吗（待更新！！！）



# BIO

## Socket和ServerSocket

![image-20200908133832670](IO.assets/image-20200908133832670.png)



### 简单服务端

~~~java
public class ServerTest {
    public static void main(String[] args) {
        int port = 8000;
        ServerSocket serverSocket = null;
        try {
            serverSocket = new ServerSocket(port);
            Socket accept = null;
            while (true){
                accept = serverSocket.accept();
                int cPort = accept.getPort();
                System.out.println(cPort+"已连接");
                BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(accept.getInputStream()));
                BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(accept.getOutputStream()));
                String s = null;
                // 读客户端发来的信息
                while ((s=bufferedReader.readLine())!=null){
                    System.out.println(cPort+"："+s);
                    // 发送给客户端信息，注意最后要加换行符
                    bufferedWriter.write(s+" 我收到了，你好呀"+"\n");
                    bufferedWriter.flush();
                }
                System.out.println(s+"退出");
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (serverSocket!=null){
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
~~~



### 简单客户端

~~~java
public class ClientTest {
    public static void main(String[] args) {
        int port = 8000;
        String ip = "127.0.0.1";
        Socket socket = null;
        try {
            socket = new Socket(ip,port);
            BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            BufferedReader console = new BufferedReader(new InputStreamReader(System.in));
            String s = null;
            while ((s=console.readLine())!=null){
                if ("quit".equals(s)){
                    break;
                }
                // 发送给服务器信息
                bufferedWriter.write(s+"\n");
                bufferedWriter.flush();
                // 读取服务器返回的信息
                String msg = bufferedReader.readLine();
                if (msg!=null){
                    System.out.println("服务器："+msg);
                }
            }
        } catch (Exception e){
            System.out.println("连接关闭");
            e.printStackTrace();
        } finally {
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
~~~



BIO中的阻塞：

- ServerSocket.accept()
- InputStream.read()
- OutputStream.write()

所以无法在同一个线程里处理多个Stream I/O

# NIO

newio

非阻塞

- 用Channel代替Stream
  - Channel是双向的，一个Channel既可以输入也可以输出
  - Channel可以阻塞可以不阻塞
- 使用Selector监控多条Channel
- 可以在一个线程里处理多个Channel I/O

## Buffer

往Channel写数据，数据是写到了Buffer中。读就是从Buffer中读。

<img src="IO.assets/image-20200908170110303.png" alt="image-20200908170110303" style="zoom: 50%;" />

写的时候从position开始写数据

### flip()

position指针放到buffer开始，limit放到最后，将模式转换为读模式

![image-20200908170411467](IO.assets/image-20200908170411467.png)

从positon可以读到limit

### clear()

切换为写模式，重置指针，相当于清空了buffer。如果之前读数据没有读完，这样写会覆盖未读数据。

<img src="IO.assets/image-20200908171130112.png" alt="image-20200908171130112" style="zoom:50%;" />

### compact()

先将未读数据拷贝到buffer的开始，position指向未读数据的下一个位置，limit到最后，然后切换为写模式。这样写不会覆盖之前未读的数据。



## Channel

Channel可以对buffer写入或者读取

两个Channel对象也可以传输数据

几个重要的Channerl：

- FIleChannel
- ServerSocketChannel
- SocketChannel





# 本地文件拷贝

## 不用缓冲区的流



## 用缓冲区的流



## NIO



~~~java

~~~









