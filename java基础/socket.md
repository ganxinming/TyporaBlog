# 1.网络编程

### 1.URL

```java
URL url=new URL("http://www.baidu.com?name=张三");
System.out.println(url.getHost());
System.out.println(url.getPath());
System.out.println(url.getQuery());
```

通过URL读取页面

```
//通过URL读取网页内容
public static void readPage(String path) throws IOException {
    URL url=new URL(path);
    //获得输入流
    InputStream in=url.openStream();
    //转成字符输入流
    InputStreamReader read=new InputStreamReader(in,"UTF-8");
    //套层buffer，加快
    BufferedReader read1=new BufferedReader(read);
    String data=null;
    //bufferReader专有的readLine，一次读一行很好用
    while ((data=read1.readLine()) != null){
        System.out.println(data);
    }
```

### 2.InetAddress

```
try {
    InetAddress inetAddress= null;
    //这个不需要加http协议。
    inetAddress = InetAddress.getByName("www.baidu.com");
    System.out.println(inetAddress.getHostAddress());
    System.out.println(inetAddress.getHostName());
} catch (UnknownHostException e) {
    e.printStackTrace();
}
```

### 3.socket

主要两个对象,ServerSocket和Socket.

Server：

```
package socket;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class Server {
     public static final int PORT=8888;
    public static void main(String[] args) throws IOException {
        //创建ServerSocket
        ServerSocket server=new ServerSocket(PORT);
        //如果未得到客户端请求连接，下面代码是阻塞的。
        //accept()监听是否有客户端连接，如果有，返回这个socket。
         Socket socket= server.accept();
         //从Socket中得到输入流(从客户端输入的信息),用Buffer套着，提高效率.
           BufferedReader readB=new BufferedReader(new InputStreamReader(socket.getInputStream()));
             String info=null;
             //readLine，Buffer流的独特读法，可以直接读一行。
        while ((info= readB.readLine())!= null){
            System.out.println("我这边是服务端：接收到客户端信息是 ---"+info);
        }
        //关闭输入流
        socket.shutdownInput();
        //得到客户端消息，下面就是回客户端，服务端将要发送的消息
        OutputStream out=socket.getOutputStream();
        PrintWriter writer=new PrintWriter(out);
        System.out.println("请回复:");
        Scanner in=new Scanner(System.in);
        String message=in.nextLine();
        writer.write(message);
        //因为套用了buffer，还有在缓冲区数据需要刷新一下。
        writer.flush();
        //关闭连接
        writer.close();
        readB.close();
        socket.close();
        server.close();
    }
}
```

client:

```
package socket;

import java.io.*;
import java.net.Socket;
import java.util.Scanner;

public class Client {
    public static void main(String[] args) throws IOException {
        String ip="127.0.0.1";
        //通过这个可以直接连接
        Socket client=new Socket(ip,Server.PORT);
        //获得输出流，用于发送消息给服务端.
        PrintWriter writer=new PrintWriter(client.getOutputStream());
        System.out.println("请回复:");
        Scanner in=new Scanner(System.in);
        String message=in.nextLine();
        writer.write(message);
        writer.flush();
        client.shutdownOutput();
        //用来读取服务端回复的消息.
        BufferedReader readB=new BufferedReader(new InputStreamReader(client.getInputStream()));
        String info=null;
        while ((info= readB.readLine())!= null){
            System.out.println("我这边是客户端：接收到服务端的信息是 ---"+info);
        }
        client.shutdownInput();
        writer.flush();
        writer.close();
        client.close();

    }
}
```