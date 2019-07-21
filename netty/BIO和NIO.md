# 1.什么是BIO

# BIO：是阻塞IO

核心：面向流（stream）

顶级抽象类：InputStream,OutputStream,Reader,Writer.（所有流的底层都是面向字节的，字符流只是因为更好的操作。）

### 正因为是抽象类，不是接口，注定了一个类要么是输入流，要么是输出流，不可能两个都是。因为只能继承一个类啊，如果是接口的话可能就不同了。与NIO很大区别。

基本流程：围绕一个while循环

输入流:                                                          输出流：

1.open a stream						1.open a stream

2.while  more information				2.while  more information

3.read  information					3.write information

4.close information					4.close information

流分类：

节点流：从特定的地方读写的流，例如磁盘，内存（比如：FileInputstream）

过滤流：过滤流是基于一个已存在的输入输出流运行。（对其他流就行包装的，Buffer）

![1554600769305](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1554600769305.png)

IO流的链接：

流和流之间（比如，buffer）可以嵌套使用，像连在一起。

![1554601516729](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1554601516729.png)



# 什么是NIO？

### 它有三个核心概念，Selector,Channel,Buffer。

#### 核心：面向块(block)或缓冲区（buffer），buffer底层本就是一块内存，他实际上就是一个数组，数据的读写都是通过buffer实现。

八大类型都有各自对应的buffer类型，如：intBuffer,charBuffer等

Channel：指的是既可以写入数据，又可以读取数据的对象。

类似于Stream，但又不一样，它既可以读又可以写，双向的。它必须搭配buffer一起使用，不存在直接在Channel直接读和取数据。数据先存在到buffer中（相当于数据存放的中转站），然后channel可以读。

每次需要读写转换时需要调用方法 byteBuffer.flip();

## flip源码如下：

```
public final Buffer flip() {
    limit = position;
    position = 0;
    mark = -1;
    return this;
}
```

Buffer是特定基本类型元素的线性有限序列。除内容外，Buffer区的基本属性还包括capacity(容量)、limit(限制)和position(位置)：

capacity是它所包含的元素的数量。缓冲区的容量不能为负并且不能更改。(即容量)
limit是第一个不应该读取或写入的元素的索引。缓冲区的限制不能为负，并且不能大于其容量。（即每次需求容量的位置+!,比如写入10个，他就在10. 例如只读7个，他就在7,<位置从0开始>）

position是下一个将要读取或写入的元素的索引（总是比已有数据的位置多1）。缓冲区的位置不能为负，并且不能大于其限制(即当前位置)

将数据读入到buffer开始：position从0开始，capacity和limit在最容量最大地方。

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20171126170354033?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzA5NjA4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 假如读入了5个，将position置0

执行完flip：将limit=position，此时想要把数据写出，limit是最大位置，所以positon不会超过这个位置

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20171126170617503?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzA5NjA4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# Buffer：重要四个属性capacity(容量)、limit(限制)和position(位置)，mark（标记）

capacity(容量)，limit(第一次在capacity，以后都在最大实际容量的前一位)，position（下一个将要读取的位置）

疑问：1.执行两次flip：执行两次作用都是一样，将limit=positon,position=0;

​	   2.无论是读还是写，底层都是对数组进行操作。（写：给数组赋值。读：从数组取值）

​	   3.执行flip，仅仅只是改变position和limit的位置，buffer里面存的值实际都存在，并没有改动。

执行个实验：

一：同一个buffer，两个不同channel对其操作。

两次flip。

```java
FileInputStream in=new FileInputStream("1.txt");
 //通过inputstream获得输入流
 FileChannel fileChannel=in.getChannel();
 //设置buffer的长度
 ByteBuffer byteBuffer=ByteBuffer.allocate(215);
 //将数据读入buffer
 fileChannel.read(byteBuffer);

 byteBuffer.flip();
 //循环读出
 byte[] bytes=new byte[215];
 int i=0;
 while (byteBuffer.remaining()>0){
     bytes[i]=byteBuffer.get();
     i++;
 }
 System.out.println(new String(bytes));
 System.out.println("########");
 in=new FileInputStream("2.txt");
 fileChannel=in.getChannel();
// byteBuffer.clear();
 byteBuffer.flip();
 fileChannel.read(byteBuffer);
 byteBuffer.flip();
 //循环读出
  bytes=new byte[215];
  i=0;
 while (byteBuffer.remaining()>0){
     bytes[i]=byteBuffer.get();
     i++;
 }
 System.out.println(new String(bytes));
```

![1554645662014](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1554645662014.png)

结论：第一次buffer的limit在读入数据后，limit的位置就已经确定了，而flip并不能将limit复原到capacity。

第二次写入数据到buffer，也只能写到limit的位置，后面的都忽略了。那么如果想要写入所有数据怎么办？

原则上，我们只需要将limit复原到capacity就行，clear()就是这个作用（复原）。

buffer里面还有更多对这三个元素进行操作的方法。

mark：将当前位置做个标记，有相应方法将postition复原到mark，而不是0位置,那么就可以从mark处进行操作。

#### 结论：0<=mark<=position<=limit<=capacity







