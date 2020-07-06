1.为interface新增特性:可以有default方法,可以有static方法。

```
public interface Action {
    void shout();
 
    default void jump() {
        System.out.print("default jump()");
    }
 
    static void desc() {
        System.out.print("static desc()");
    }
}
a、default方法访问权限默认就是public（不填写时）default方法通过实现类对象调用，default方法支持实现类重写

b、static方法也是权限默认就是public（不填写时） static方法，只能通过interface名称来调用，static方法不支持实现类重写

c、private、protected均不能修饰defaut与static方法（跟interface原有规则保持一致，哈哈）
```

首次遇见是在sprinboot拦截器中:HandlerInterceptor。都是default，可以不用实现方法。