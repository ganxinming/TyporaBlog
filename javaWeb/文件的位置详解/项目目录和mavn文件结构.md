### 普通项目结构

项目|--src ---(里面就是包了写java代码)

​        |--webRoot   | -- WEB-INF--|        --classes--(这个目录在项目中看不到，是发布到tomcat之后发生的) 

​					                |	         ---lib--

​					                |		--web.xml		

​		                | --各种前端库--      

​		                | --JSP 文件等 （但是放在这不安全，一般在WEB-INF下）

![1549509474743](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1549509474743.png)

![1549509483199](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1549509483199.png)

![1549509527088](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1549509527088.png)

#### src编译完的代码放在classes中

##### 在发布到tomcat中，原来src和WebRoot平起平坐，现在被整合到classes中，所以最后只剩下WebRoot一条线了，而src加入到了WEB-INF中的classes旗下。



# MAVN项目结构

![1549510250937](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1549510250937.png)

mavn项目结构发生了些变化，其中src，target，pom.xml属于同级目录，而我们主要编写src目录下的文件。

如图，所有的工作都在main下或者test下。其中，我们主要代码在main的java中。resources和java属于同一等级。其中webapp就是我们上面说的webRoot，里面包含WEB-INF。

![1549510515425](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1549510515425.png)

当发布项目之后，这个java和resources是同级的，两个文件夹都被发布到target下面的classes下

（**注意这里不是在发布到WEB-INF下的classes，这里也没有lib文件了，因为这是mavn，不要包文件夹**）

![1549511024018](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1549511024018.png)

可以看到，我们得java和resouces里面的文件夹直接发布到classe里，默认就去除了java和resouces文件名，一起扔在了里面。

我们常说的classPath就是这里，所以如下 classpath : conf/db.properities。直接找到了。

```
<context:property-placeholder location="classpath:conf/db.properties" />
```