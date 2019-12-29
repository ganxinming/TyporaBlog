1.tomcat基础结构

主要有server,service,context,host,engine,wrapper,connector这7个容器来组装。

![1575272991076](/home/badribbit/.config/Typora/typora-user-images/1575272991076.png)

由图可知一个server就是一个tomcat，他是整个tomcat项目。

一个server可以有多个service，一个service包含多个连接器(Connector)和一个容器(Container).

连接器作用:

Connector的底层是使用socket进行连接，Request和Response时按照http协议（协议可选）来封装，所以Connector同时实现了TCP/IP和HTTP协议.

Connector(多个)主要作用：负责处理浏览器发送过来的请求，将其转化成request和response对象(servlet能识别的对象)，然后产生一个线程用于处理该请求，具体是进入到容器(Container底层也是个servlet容器)中进行处理。

容器作用:

Container(一个)容器是子容器的父接口，所有的子容器都继承该接口。其下的四个子容器接口为：Engine、Host、Context、Wrapper。(其中配置文件只配置了Engine、Host，其他两个是非必须的)

Engine：接受Connector传来的request和response对象，选择合适的host进行处理。(包含多个host)

Host:	代表一个虚拟主机，也是一个站点,即配置域名和webapps文件夹的地方。(包含多个Context)

Context：代表一个url访问应用，过域名可以直接访问到ROOT根目录里面的应用就是主应用。(包含多个Wrapper)

Wrapper:   每一个Wrapper封装一个servlet.

#### 总结：大概的结构就是上面了。

#### 启动流程：

一：调用bootstrap类启动(其实startup做的事情也就是调用bootstrap类)

bootstrap做的事情：

1.初始化ClassLoader类加载器(没有他无法加载后面代码)

2.类加载生成catlina实例，并通过反射调用catlina三个方法。

具体:通过init()方法进行initClassLoader()进行初始化类加载器。得到类加载器后，通过反射调用catlina的setAwait(true)，load(args)，start();这三个方法。	

所以总的来说：bootstrap主要完成了init()方法，调用catlina的setAwait(true)，load(args)，start()四个方法。

二：Catlina的启动(因为上面调用了start)

顺序调用setAwait(true)，load(args)，start()方法。

setAwait(true)：设置一个标记，为后面的start()使用。当通过后面的start()方法启动完服务器后，会检查这个值为true还是false，如果为true，调用Catlina的await()方法，tomcat继续开着。如果为false，则调用Catlina的stop()方法，关闭tomcat。（反正就是通过设置这个参数，来判断是启动还是关闭）

load(args)：创建Digester去解析server.xml，并根据server的配置文件创建server实例。(加载配置文件就是这里)

然后调用server实例的init()方法，进行server的初始化。Server初始化时，又会调用其内部的Service的init方法。

start():主要调用了server的start()方法来启动服务器，然后server的start()方法再调用多个service的start()方法。

（其实后面所有的初始化都是在load()读取了配置文件后进行初始化了）

三:Server的启动(之前catlina调用了server的init和start方法)

一个server含有多个service。service也是个接口，默认实现类是StandardService，所以我们后面谈论的service都是StandardService。每一个service都是一个对外提供服务的组件，每一个Service中都包含：一个Container+多个Connector，所以service中的方法，都是用来操作这两种类的。

四:之后就是service下一系列的初始化。比如:service再调用Connector的init方法创建并初始化.如下图:

![1575524926863](/home/badribbit/.config/Typora/typora-user-images/1575524926863.png)

五：Lifecycle接口

这里有必要提下Lifecycle(生命周期)接口，他的实现类是LifecycleBean类。就上面提到的service和server都继承自LifecycleBean，都含有生命周期。实际上凡是拥有“生命周期”的组件都实现了Lifecycle接口，Lifecycle接口的作用就是进行“生命周期的规定”。

内部主要内容:

1.定义了13个String常量，这些常量被用于定义“LifecycleEvent事件”的type属性中 。这样设计的好处是，只需要发送一种类型事件LifecycleEvent，而根据里面type属性的值的不同，来判定是什么事件。（否则13种事件就得定义13中事件类）。

2.定义了三个方法用于管理监听器LifecycleListener。分别是增加，查找，删除监听器。

3.定义了四个生命周期方法：init(),start(),stop(),destory()

LifecycleBean正是对上面方法进行了实现。

六:Connector内部构成：

Connector的主要任务是负责处理浏览器发送过来的请求，并创建一个Request和Response的对象用于和浏览器交换数据，然后产生一个线程用于处理请求，Connector会把Request和Response对象传递给该线程，该线程的具体的处理过程是Container容器的事了。

Connector中使用ProtocolHandler来具体处理请求，不同的ProtocolHandler代表不同协议的请求类型。

ProtocolHandler中又包含三个重要的组件：

Endpoint，用于处理底层socket网络连接。

Processor，用于将socket接收到的socket封装成request。

Adapter，用于将requst(转成servletReqest)交给Container进行处理。(采用适配器，和servlet容器适配)

(在Connector进行初始化init时，主要就是用来初始化ProtocolHandler)

Connector的初始化：在standServie(即service)中有个startInternal()方法,里面包含了Connector.start()方法。

在他的start()方法中又有startInternal()对ProtocolHandler进行初始化。

七:tomcat优化

1.内存空间优化:默认空间很小，当项目很大时，会将空间占满，抛出异常。修改启动时JVM参数。

2.连接器优化:

优化参数配置:最大线程数，最大空闲线程，连接超时等

```
<Connector port="8080" 
protocol="org.apache.coyote.http11.Http11NioProtocol" //配置方式
        redirectPort="8443"
        URIEncoding="UTF-8"
        minSpareThreads="100"
        maxSpareThreads="500"
        maxThreads="1000"
        acceptCount="700"
        connectionTimeout="30000"
        enableLookups="false"/>
        
```

3.线程池优化:

Executor代表了一个线程池，可以在Tomcat组件之间共享。使用线程池的好处在于减少了创建销毁线程的相关消耗，而且可以提高线程的使用效率。

配置完，则tomcat所有组件可以共享，则大家的线程被统一管理。(使用时记得配置到Connector连接器)

```
 <Executor name="tomcatThreadPool"   
         namePrefix="catalina-exec-"   
         maxThreads="1000"   
         minSpareThreads="100"  
         maxIdleTime="60000"  
         maxQueueSize="Integer.MAX_VALUE"  
         prestartminSpareThreads="false"  
         threadPriority="5"  
         className="org.apache.catalina.core.StandardThreadExecutor"/>  
```

4.IO优化:处理连接请求，BIO/AIO/APR(需要安装APR库，从操作系统级别解决异步IO问题，质的飞跃，就是安装比较麻烦)的选择。一般情况就选择BIO，如果可以，选择APR库再好不过了，大幅提升性能。

