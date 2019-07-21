[TOC]

### 1.发布dubbo的过程以及遇到的问题。

过程很简单，按照文档来，进行项目改造。

第一步：把e3-manager的pom文件中删除e3-manager-web模块。.将web层分离出来，和manager同等级。（删除其他配置文件，只保留springMvc配置文件，因为web模块只需要做控制层，和显示。所以只需要配置springMVC的东西就可以了）****

第二步：把e3-manager-web文件夹移动到e3-manager同一级目录。

第三步：e3-manager-service的pom文件修改打包方式，为war包

第四步：在e3-manager-service工程中添加web.xml文件

第五步：把e3-manager-web的配置文件复制到e3-manager-service中。删除springmvc.xml

第六步：service层的web.xml 中只配置spring容器。删除前端控制器（因为他不需要配置web层的）

第七步：service层发布服务（添加dubbo和zookeeper依赖，在application-service中发布dubbo服务，配置直接复制）然后在manager层中配置tomcat插件8080.

配置：

```
<!-- 提供方应用信息，用于计算依赖关系 -->
	<dubbo:application name="SSM-manager" />
	<dubbo:registry protocol="zookeeper" address="120.79.189.242:2181" />
	<!-- 用dubbo协议在20880端口暴露服务 -->
	<dubbo:protocol name="dubbo" port="20880" />
	<!-- 声明需要暴露的服务接口，注意ref 中的是小写 -->
	<dubbo:service interface="cn.e3mall.service.ItemService" ref="itemServiceImpl" timeout="600000"/>
```

第八步：web层添加spring和springmvc的jar包的依赖，dubbo和zookeeper依赖，删除e3-mangager-service的依赖。修改springmvc.xml，在springmvc的配置文件中添加服务dubbo服务。添加tomcat插件8081。

### 完！

出现的问题：1.连不上服务器，zookeeper端口打开，防火墙关闭。

​						2.TbItem  implements Serializable ，调用需要用到的poji类，需要序列化。

​						3.web.xml加载spring容器，记得加载全部application-*.xml.

​						4.该加的注释一定要加，比如@Controller,@Service,@Autwired

各模块之间的依赖关系：

1.common: 无依赖

2.web :(因为被分离出来了，所以只有依赖interface，因为这个模块时调用的service的接口) 依赖interface

3.manager ：依赖common模块。含有四个模块service，dao，interface ，pojo。

   --service :接替了web的功能，所以需要依赖dao层，interface层

  --dao：依赖pojo层

  --interface:依赖pojo层



常见错误：

1.

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'itemCatController': 
```

一般都是忘记注解@Service,记得pojo类seralizeable。

2。在service模块引入了服务，在web模块也要引入服务。



