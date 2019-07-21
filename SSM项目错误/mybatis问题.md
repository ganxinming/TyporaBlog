# 类型不匹配

![1553326714103](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553326714103.png)

因为我存的hobby是数组，然后数据库中类型是字符串，这样直接赋值肯定会报错啊。直接启动不了

目前数组和数据库怎么对接还没想好，这里的话就是不要赋这个值。

# 返回值是list

![1553327475405](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553327475405.png)

如上，返回的是所有user是一个list集合，但是这里返回类型只是list中指定的类型。

## 表的属性

### ![1553393674073](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553393674073.png)

### 思考一个问题，在一对多中，我们用下面这段话来得到一对多

![1553395780703](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553395780703.png)

但是数据库得到的数据视图是这样

![1553396012401](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553396012401.png)

其实这从外面看这不是个Sclass的list集合吗，怎么会封装成一个Sclass对象呢？仔细观察会发现这两列的class对象的属性相同，但是他的Lsit\<student>不同，所以形成了两列，而teacher对象在class，是封装成一个对象，所以他的数据也相同。我猜mybatis肯定是这样封装的，把Sclass类相同的属性封装成了一个Sclass类，而那些不同的属性（student），正好也可以封装成一个list集合。而后面一对一的teacher对象，也可以封装一个对象。

### 但是我们也可以这样想，把这些数据封装成一个list\<Sclass>,只不过现在的他的list\<student>只有一个对象，因为分成两个Sclass对象，一个人一个student.(只是一个想法,感觉实际没什么用途)

# 缓存

一级缓存指的就是sqlsession，在sqlsession中有一个数据区域，是map结构，这个区域就是一级缓存区域。一级缓存中的key是由sql语句、条件、statement等信息组成一个唯一值。一级缓存中的value，就是查询出的结果对象。

二级缓存指的就是同一个namespace下的mapper，二级缓存中，也有一个map结构，这个区域就是一级缓存区域。一级缓存中的key是由sql语句、条件、statement等信息组成一个唯一值。一级缓存中的value，就是查询出的结果对象。（相当于二级缓存中存着多个一级缓存）

一级缓存是默认使用的。

二级缓存需要手动开启。

(两个文件配置在这里就不说了)

其中禁用缓存和刷新缓存需要如下配置.

![1553410833653](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553410833653.png)

# 总结

### 如果扩展类中有单独对象,则resultMAP中必有association，如果有list集合则必有collection

#### 所以association封装单独对象（一对一）

#### collection封装集合对象(一对多)

## 2.主键返回

![1561258362090](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1561258362090.png)

主键返回搭配主键自增使用。

主键返回的意思是，当我们插入一条数据时，数据库的主键会自动加1，但是我们java代码中的插入对象并没有设置id的，这个id正是数据库的自增的，主键返回就是说，将自增后的主键返回到该插入对象中，是这个对象完整，因为这个对象可能还有用，比如作为返回值返回。



