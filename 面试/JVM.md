#### 1.JVM常用工具

1.jconsole工具(直接cmd jconsole打开)：可查看线程使用情况，虚拟机内存，堆栈等。

2.jvisualvm工具(直接cmd jvisualvm打开)：比jconsole更好用，集合多种小工具。

3.javap -c :对.class文件进行反编译。直接在terminal中就可以使用。

会生成很多助记符:

1.ldc:将常量final值从常量池推送至栈顶(即马上就要使用)

#### 2.JVM参数，三种格式：

-XX:+<option>表示开启某个参数

-XX:-<option>表示关闭某个参数

-XX:-<option>=<value>表示某个参数等于某个值

参数:TraceClassLoading:查看类的加载信息。

-XX:+TraceClassLoading

3.初始化

初始化类和接口的区别：当初始化一个类时必须保证其父类进行初始化，并不保证其父接口初始化。即保证父类初始化，不保证父接口初始化。