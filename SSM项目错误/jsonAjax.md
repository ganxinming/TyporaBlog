# 前后端类型问题

使用原始的Ajax方式

![1553781875804](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553781875804.png)

服务端：

![1553782026761](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553782026761.png)

现在来说说问题啊，就这个@RequestBody 只能封装json字符串，即application/json类型。所以前面需要将json对象转成jaon字符串。

现在出现了几个问题:

1.什么是json对象，什么是json字符串?

json对象:{username:  "张飞"};

json字符串: "{username: ''张飞''}"  

json对象是可以通过对象.属性访问的，json字符串是用来传输的，不能直接.访问.

2.JSON.stringify()的作用是将 JavaScript 值转换为 JSON 字符串，**而**JSON.parse()可以将JSON字符串转为一个对象,两个方法正好相对。

#### 现在的问题是:

![1553782883938](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553782883938.png)

此时如果Controller还是用上面的@ResponseBody，会出现415不支持媒体类型。因为@ResponseBody只能将前端的json字符串进行封装。

但是如果我用了JSON.stringify(),还是封装不了，后来才发现了个问题，在原始的Ajax中，我们是设置了application/json格式的，但是在post中就不行了，目前我没有找到更改contentType的方法，但此时令人意外的是，AJAX如果不改格式，默认就是application/x-www-form-urlencoded。这种格式就是大多数使用的格式，他会把数据改成key=value&key=value的形式。所以我没能变成json字符串格式，也用不了@ResponseBody.

#### 但是别忘了，springmvc会自动帮我们封装application/x-www-form-urlencoded格式数据，所以controller如下：

![1553783443798](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553783443798.png)

就是普通封装，不能要@ResponseBody

其他不能更改ContentType的Ajax方式也是如此.

# 表单序列化

![1553848448648](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553848448648.png)

表单序列化的作用就是将表单的值变成key=value&key=value的形式,又因为咱们表单默认的contentype就是

application/x-www-form-urlencoded，因此前端不能用@ResponseBody封装。

所以controller是这样

![1553848681870](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553848681870.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------------

#### 那如果想把表单转成json怎么办?

可能会说，把contenType值改成applicaton/json，这样改没用啊。咱们第一个可以是因为，那个本身就是json对象，通过JSON.Stringify,可以转成json字符串。但是这里，他本身不是json对象啊。即使将序列化后的值放到

Json.Stringify中也是不行。

### 百度后发现，将表单值进行json化的方法

1.![1553849334726](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553849334726.png)

需要导入serializejson.js

2.使用serializeJson方法，将表单转成json对象，后将json对象转成json字符串.

![1553849454488](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1553849454488.png)

3.此时后端可以用@ResponseBody接受参数了.

(但是这里出了点问题，其他类型都可以封装，但是数组类型就是封装不了，不知道是serializeJson封装的问题，还是其他原因.)