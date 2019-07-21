# 1.会话变量和全局变量

1.查看会话变量：  show session variables;  

2.模糊查询出需要的：show session variables like 'auto%';

3.设置会话变量： 1.set session autocommit='on';（设置autocommit变量）2. set @@session.autocommit ='off';

**会话变量只适用当前用户。（例如A用户和B用户设置的变量互不相干）.**

**查看全局变量：show global variables like 'auto%';**

设置全局变量：.set global autocommit='on';（设置autocommit变量）2. set @@global.autocommit ='off';

顾名思义和会话不同的就是个关键字，global。

全局的改动（用户A,B都会影响）

#### 总结：会话变量就是当时连接上去有效，全局变量就一直有效.

# 2.存储过程

一组为了完成特定功能的SQL语句集，经编译后存储在数据库中，用户通过指定存储过程的名字并给定参数（如果该存储过程带有参数）来调用执行它（SQL语句+控制语句）

### 优点：

(1).**增强SQL语言的功能和灵活性**：存储过程可以用控制语句编写，有很强的灵活性，可以完成复杂的判断和较复杂的运算。

(2).**标准组件式编程**：存储过程被创建后，可以在程序中被多次调用，而不必重新编写该存储过程的SQL语句。而且数据库专业人员可以随时对存储过程进行修改，对应用程序源代码毫无影响。

(3).**较快的执行速度**：如果某一操作包含大量的Transaction-SQL代码或分别被多次执行，那么存储过程要比批处理的执行速度快很多



