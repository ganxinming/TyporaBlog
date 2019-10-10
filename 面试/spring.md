jdk代理:

java动态代理是利用反射机制生成一个实现代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理。

JDK动态代理只能对实现了接口的类生成代理，而不能针对类.

实现: 

```
public class TransactionHandler implements InvocationHandler {

    //需要代理的目标对象
    //这里设计为可以为任意对象添加事务控制, 所以将目标对象声明为Object
    private Object target;

    //构造TransactionHandler时传入目标对象
    public TransactionHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //调用目标方法前的处理
        System.out.println("开启事务控制...");
        //调用目标对象的方法
        Object result = method.invoke(target, args);
        //调用目标方法后的处理
        System.out.println("关闭事务控制...");
        //放回方法调用结果
        return result;
    }

}
---------------------------------------------------------------------------
public class Main {

    public static void main(String[] args) {

        //新建目标对象
        Object target = new UserDaoImpl();

        //创建事务处理器
        TransactionHandler handler = new TransactionHandler(target);

        //生成代理类并使用接口对其进行引用
        UserDao userDao = (UserDao)Proxy.newProxyInstance(target.getClass().getClassLoader(),
                                                          target.getClass().getInterfaces(),
                                                          handler);
        //针对接口进行方法调用
        userDao.save();

    }

}
```

**Proxy.newProxyInstance()方法得到的是代理类，即上面的UserDao，虽然看上去是普通类，其实是代理类**

**前面两个参数，就是确定这个类是什么类型的。因为是动态代理，所以得到的代理类是动态改变的咯。**

**后面的handler，这是生成这个代理类的关键，他决定了方法调用前后执行什么方法。**

**handler这个类实现InvocationHandler接口，实现invoke方法，这里面决定了方法执行前后执行的代码。**

cglib:

cglib动态代理是对代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。CGLIB是针对类实现代理，主要是对指定的类生成一个子类，覆盖其中的方法,因为是继承，所以该类或方法最好不要声明成final .



注解使用:

1  认识注解(Annotation)
注解相当于一种标记，在程序中加了注解就等于为程序打上了某种标记，没加，则等于没有某种标记。

以后，javac编译器、开发工具和其他程序可以用反射来了解你的类及各种元素上有无何种标记，看你有什么标记，就去干相应的事。

注解可以加在包，类，字段，方法，方法的参数以及局部变量上。

注解的存在不影响程序的编译和执行。

javac编译器看到注解，就会去做相应的动作。

 

注解是JDK1.5的新特性。

一个注解就是一个类，使用注解，就相当于创建了一个对象。



2  注解的分类
注解按照使用的方式和用途，注解可以分为三大类。

（1）内建注解。

          内建注解也称为基本注解，位于java.lang包下。
    
          内建注解有三个：
    
          1，检验重写父类方法：@Override
    
          2，标识方法已经过时：@Deprecated
    
          3，取消编译器警告：@SurppressWarnings

（2）元注解。

          元注解就是在注解上添加的注解。
    
          位置：元注解位于java.lang.annotation子包中。
    
          作用：用于修饰其他注解。
    
          元注解有四个：
    
         @Retention，@Target，@Documented，@Inherited。

（3）自定义注解。

         需要用到关键字@interface来定义。
3  注解的生命周期
注解的生命周期有三种，注解驻留在源文件阶段，字节码文件阶段和内存字节码阶段。

（1）注解被保留到源文件阶段

           当javac把.java源文件编译成.class时，就将相应的注解去掉。这种注解的生命周期就维持到源文件阶段。

（2）注解被保留到字节码文件阶段

          在JVM通过ClassLoader向内存中加载字节码文件时候，JVM会去掉相应的注解。这种注解的生命周期就维持到字节码文件阶段。

注意：生命周期到源文件阶段和字节码文件阶段的注解，由于JVM执行内存中的字节码时候，相应的注解已经被Javac或者JVM去除，所以无法使用反射来访问相应的注解。

（3）注解被保留到内存中的字节码阶段

         JVM运行内存的字节码时候，仍然可能会保留并且执行的某些注解。这种注解的生命周期就维持到内存字节码阶段。
    
         注意：这个阶段，程序可以通过反射访问生命周期到内存字节码阶段的注解。

 


三个阶段简单表示为：java源文件-->class文件-->内存中的字节码

@Retention(RetentionPolicy.RUNTIME) *//此注解运行到内存时才消除*

@Target({ElementType.METHOD,ElementType.TYPE}) *//此注解可修饰方法和类*

public @interface MyAnnotation {}

