动态代理

### 1.1.1 **JDK动态代理**

l JDK动态代理 对“装饰者”设计模式 简化。使用前提：必须有接口

1.目标类：接口 + 实现类

2.切面类：用于存通知 MyAspect

3.工厂类：编写工厂生成代理

4.测试

![1553415118274](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553415118274.png)

![1553415131508](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553415131508.png)

![1553415173668](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553415173668.png)

![1553415193146](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553415193146.png)



### 1.1.1 **CGLIB字节码增强**

l 没有接口，只有实现类。

l 采用字节码增强框架 cglib，在运行时 创建目标类的子类，从而对目标类进行增强。

l 导入jar包：

​	自己导包（了解）：

​		核心：hibernate-distribution-3.6.10.Final\lib\bytecode\cglib\cglib-2.2.jar

​		依赖：struts-2.3.15.3\apps\struts2-blank\WEB-INF\lib\asm-3.3.jar

​	spring-core..jar 已经整合以上两个内容

![1553415233945](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553415233945.png)

![1553415264026](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553415264026.png)

