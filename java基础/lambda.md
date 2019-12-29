1.lamba表达式是什么?

我们可以把它看成是一种闭包，它允许把函数当做参数来使用，是面向函数式编程的思想。

使用:() -> {} 就是这么简单使用。(仅当只有1个参数时，可以省略括号，其他情况都不行0,>1都不行)

那什么情况可以使用？假如接口有多个方法呢，他怎么知道实现哪个方法？

参数是一个接口时且里面只有一个方法时，默认就实现那个唯一的方法。

比如new Thread( () -> {} ); 其实我们都知道传入的是Runable接口，这时使用lamba表达式则默认是作为传入的参数实现那里面唯一的方法。

2.简单使用：

```
//List类的forEach方法
items.forEach(c-> System.out.println(c));

//先使用过滤器
items.stream().filter(s->s.contains("B")).forEach(c1-> System.out.println(c1));

//Java8遍历map，这简直就是逆天操作
items.forEach((key,value)-> System.out.println("key:" + key + " value:" + value));

//将一组对象进行分组
Map<String, List<Person>> collect = personList.stream().collect(Collectors.groupingBy(c -> c.getAddress()));
        
```

3.Stream使用:使用的方法按照下面的顺序调用：

```
stream+->filter+-> |sorted+-> |map+-> |collect|
```

stream:可以将集合数据转成流，而后面的那些操作就想管道对流进行操作。

1.foreach：遍历流数据。

2.filter:过滤出符合条件的数据。

3.limit:限制输出的数据量。

4.sorted:对流数据进行排序。

5.map:方法用于映射每个元素到对应的结果

6.collect:将结果集进行聚合，合成其他什么list都行。

