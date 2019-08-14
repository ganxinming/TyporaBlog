### java三个特性：

**封装**：尽可能地隐藏内部的细节，只保留一些对外接口使之与外部发生联系。比如：将变量private修饰，通过并生成get/set方法来进行访问。

**继承**：使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能。继承所描述的是“is-a”的关系。

1、子类拥有父类非private的属性和方法。

2、子类可以拥有自己属性和方法，即子类可以对父类进行扩展。

3、子类可以用自己的方式实现父类的方法。（重写）。

4、向上转型：是从一个叫专用类型向较通用类型转换，所以它总是安全的

5、向下转型：不安全，从通用类型转到专用类型，有失败的风险。**如果向下转型想要编译和运行都成功，必须使用强制转型语法，还必须要求运行起来父类引用确实指向该子类的对象，不然一个猫类型转成的父类型转成狗类型，不是很荒唐吗**。

**多态**：多态是同一个行为具有多个不同表现形式或形态的能力。

##### 多态存在的三个必要条件

- 继承
- 重写
- 父类引用指向子类对象：提高了代码的维护性和扩展性。向上转型 ，动态绑定（动态链接） 

**实现多态的技术称为**：

动态绑定（dynamic binding），是指在运行期间判断所引用对象的实际类型，根据其实际的类型调用其相应的方法。比如：方法参数是个父类型，传入子类型，调用方法会判断实际类型调用。

Java中除了final，static，private和构造方法是静态绑定。(绑定：方法和类绑定)其他都是动态绑定，所以说，只有方法才可能是动态绑定，变量是静态绑定。

主要表现：

1.重写：发生在父子类中，方法名称相同，参数列表相同，方法体不同，遵循“运行期绑定”，看对象的类型的调用方法。

两同：方法名称相同，参数列表相同
两小：子类方法的返回值类型小于或等于父类的。void时，必须相等，基本类型时，必须相等
引用类型时，小于或等于，子类抛出的异常小于或等于父类的。
一大：子类方法的访问权限大于或等于父类的。

2.重载：在一个类中，方法名称相同，参数列表不同，方法体不同。遵循“编译期绑定”，看引用的类型来绑定方法

### 接口和抽象类区别：

1.接口中所有的方法隐含的都是抽象的，而抽象类则可以同时包含抽象和非抽象的方法。

2.接口中所有抽象方法必须实现，抽象类可以部分实现。

3.接口可以实现多个，抽象类只能实现继承一个。

4.接口中方法默认public abstract，而抽象类可以有其他访问修饰符和非abstract方法。

5.接口中变量默认是public final，抽象类默认是非final变量。

### 访问修饰符：

​			访问权限   类   包  子类  其他包

  　　　　  public     ∨   ∨    ∨     ∨          （对任何人都是可用的）

   　　　　 protect    ∨   ∨   ∨     ×　　　 （同包下，和继承的类可以访问）

   　　　　 default    ∨   ∨   ×     ×　　　 （包访问权限，即在整个包内均可被访问）

   　　　　 private    ∨   ×   ×     ×　　　 （除类型创建者和类型的内部方法之外的任何人都不能访问的

### java异常：

顶级异常：Throwable，Throwable又派生出Error类和Exception类。

Error：多为JVM错误，该错误不能被程序员通过代码处理。

Exception：检查异常（checked exception）：需要手动处理(try catch和throws)，和运行时异常(RuntimeException)运行时可能发生的异常，如分母为0，数组越界，空指针等。

### java泛型：

相当于一颗语法糖，在编译过程中，泛型会确保类型安全，正确检验泛型结果后，会将泛型的相关信息擦除（泛型擦除）。

好处：对于ArrayLIst使用泛型，可以免去转换类型，确保集合是同一类型。

**有两种限定通配符**：一种是<? extends T>它通过确保类型必须是T的子类来设定类型的上界，另一种是<? super T>它通过确保类型必须是T的父类来设定类型的下界

泛型类和泛型方法：

```
泛型类：是在实例化类的时候指明泛型的具体类型；
public class Pair<T> { //T is type parameters
	private T first;
	private T second;
	Pair<Integer> p=new Pair<Integer>()
	//初始化的时候一定要指定泛型
泛型方法：是在调用方法的时候指明泛型的具体类型 。
//在泛型类中声明了一个泛型方法，使用泛型E，这种泛型E可以为任意类型。可以类型与T相同，也可以不同。
//由于泛型方法在声明的时候会声明泛型<E>，因此即使在泛型类中并未声明泛型，编译器也能够正确识别泛型方法中识别的泛型。
        public <E> void show_3(E t){
            System.out.println(t.toString());
        }
```

#### java类型：

##### 基本类型：默认整型是int，默认的浮点是double。所以对于float要加1.1f。

- byte/8  boolean 1
- char/16 short/16
- int/32 float/32 
- long/64 double/64

包装类型：

```
Integer x = 2;     // 装箱  调用了 Integer.valueOf(2);如果数值在[-128,127]之间，便返回指向缓冲池中已经存在的对象的引用；否则创建一个新的Integer对象。
int y = x;         // 拆箱  调用了 Integer.intValue(x);
intValue()是把Integer对象类型变成int的基础数据类型；
parseInt()是把String 变成int的基础数据类型；
Valueof()是把给定的String参数转化成Integer对象类型；
```

##### 引用类型：

**String：**

String 被声明为 final，因此它不可被继承，不可重写，不可更改。(Integer等包装类也不能被继承)

在 Java 8 中，String 内部使用 char 数组存储数据。

```
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```

在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 `coder` 来标识使用了哪种编码。

```
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```

##### 不可变好处：

**1.可以缓存 hash 值**

对于map的键值，不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

**2. String Pool 的需要**

如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。

**3.安全性**

线程可以安全访问，网络传输可以安全。

##### String Pool：

字符串常量池（String Pool）保存着所有字符串字面量（literal strings），这些字面量在编译时期就确定。不仅如此，还可以使用 String 的 intern() 方法在运行过程中将字符串添加到 String Pool 中。

当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

在 Java 7 之前，String Pool 被放在运行时常量池中，它属于永久代。而在 Java 7，String Pool 被移到堆中。这是因为永久代的空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。

#### 常问方法：

#### 1.==和equals（）比较：

- 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。
- 对于引用类型，== 判断两个变量是否是同一引用地址，而 equals() 判断引用的对象内容是否等价。

##### 在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等。

##### 因为：equals如果不重写，默认还是==，如果重写了，里面是用hashcode来判断是否是同一个对象值。

##### hashcode必须重写，因为hashcode默认是取对象的地址，这样的话两个新new的对象永远不可能相等。

自反性：对于任意的对象x，x.equals(x)返回true(自己一定等于自己)；
对称性：对于任意的对象x和y，若x.equals(y)为true，则y.equals(x)亦为true；
传递性：对于任意的对象x、y和z，若x.equals(y)为true且y.equals(z)也为true，则x.equals(z)亦为true；
一致性：对于任意的对象x和y，x.equals(y)的第一次调用为true，那么x.equals(y)的第二次、第三次、第n次调用也均为true，前提条件是没有修改x也没有修改y

对于非空引用x，x.equals(null)永远返回为false。

### 2.toString()：

默认返回 类名@4554617c 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示。

### 3.clone():

clone() 是 Object 的 protected 方法，它不是 public，一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。(需重写 如下：return (复制的类型名)super.clone();)

注意：1、Object类虽然有这个方法，但是这个方法是受保护的（被protected修饰），所以我们无法直接使用。2、使用clone方法的类必须实现Cloneable接口，否则会抛出异常CloneNotSupportedException。

浅拷贝：只用重写上面的colne方法就行。只进行值拷贝，所以对于基本类型只是复制了值，对于引用类型，只复制地址。地址还是指向对象，并且可以修改，所以这样不安全。

深拷贝：对象也复制一份。通过对象序列化实现深拷贝，默认会将该对象的整个对象图进行序列化，再通过反序列即可完美地实现深拷贝。

### 关键字：

#### final：

##### 1.数据：声明数据为常量，不能被改变的常量。

- 对于基本类型，final 使数值不变；
- 对于引用类型，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。

##### 2.方法：声明方法不能被子类重写。且private 方法默认为final。

##### 3.类：声明类不允许被继承。

#### static：

##### 1.静态变量：

- 静态变量：又称为类变量，也就是说这个变量属于类的，类所有的实例都共享静态变量，可以直接通过类名来访问它。静态变量在内存中只存在一份。
- 实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。

##### 2.静态方法：

在类加载的时候就存在了,只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字。

**3. 静态语句块**

静态语句块在类初始化时运行一次。

**4. 静态内部类**

非静态内部类依赖于外部类的实例，而静态内部类不需要。

#### 初始化顺序：

- 父类（静态变量、静态语句块）
- 子类（静态变量、静态语句块）
- 父类（实例变量、普通语句块）
- 父类（构造函数）
- 子类（实例变量、普通语句块）
- 子类（构造函数）

### 反射：

每个类都有一个 **Class** 对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。 

作用：在运行时获得某些未知的类。

##### 为啥JDBC使用class.forName("com.mysql.jdbc.Driver")?

在Java中如果想要使用一个类，则必须要求该类已经被加载到Jvm中，加载的过程实际上就是通过类的全限定名来获取定义该类二进制字节流，然后将这个字节流所表示的静态存储结构转换为方法去的动态运行时数据结构。同时在在内存中实例化一个java.lang.Class对象，作为方法区中该类的数据访问入口(供我们使用)。

##### class.forName作用就是：将类加载到JVM中，可是并没有初始化对象啊？且看他的代码

```
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    public Driver() throws SQLException {
    }
    static {
        try {
        //在静态代码块中，有初始化对象。
            DriverManager.registerDriver(new Driver());
        } catch (SQLException var1) {
            throw new RuntimeException("Can't register driver!");
        }
    }
}
```

所以他被加载到JVM,然后执行了静态代码块。如果new一个对象，会相当于new了两次。

使用：Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：

- **Field** ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的属性；
- **Method** ：得到Method method=clazz.getMethod();对象关联的方法；可以使用 method.invoke() 方法调用该对象方法。
- **Constructor** ：可以用 Constructor con =clazz.getConstructor();和con.newInstance() 创建新的对象。

优点：可扩展性强，增加程序的灵活性。

缺点：安全性不可靠，会暴露内部，效率低（因为是在已加载的代码基础上得到的）。

应用：jdbc，spring的IOC，代理模式，Cglib字节码增强。