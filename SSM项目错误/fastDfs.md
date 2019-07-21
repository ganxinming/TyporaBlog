1.开虚拟机，首先ping下看下是否通了，没有的话，关闭防火墙，对于虚拟机的虚拟配置，还原默认设置。

2.web模块引入mavn依赖，引入两个配置文件，其中client.conf 引入服务端主机号和端口号，resource.properties用于表示图片服务器地址，用于赋值给变量（在图片路径的时候有用）。

3.任何文件的上传都要配置多媒体解析器，在web模块的spring-mvc配置。因为要用到resource.properties

，这个可以通过如下代码获取到里面的变量。所以这个文件也需要加载到spring-mvc中，不然注解怎么获取值？

```
@Value("${IMAGE_SERVER_URL}")
private String IMAGE_SERVER_URL;
```

4.因为我们用的是FastDFSClient，和其他相关的类，需要将他们引入到common中。