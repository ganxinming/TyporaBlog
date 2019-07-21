SSO单点登录：

### 1.登录过程：

![img](file:///C:\Users\12714\AppData\Local\Temp\ksohtml2336\wps1.png)

1.登录页面提交用户名密码。

2.登录成功后生成token。	把用户信息保存到redis。Key就是token，value就是TbUser对象转换成json。

3、使用String类型保存Session信息。可以使用“前缀:token”为key。

4.设置key的过期时间。模拟Session的过期时间。一般半个小时。

5、把token写入cookie中。

6、Cookie需要跨域。例如www.e3.com\sso.e3.com\order.e3.com，可以使用工具类（使用CookieUtils）。

7、Cookie的有效期。关闭浏览器失效。



### 2.退出登录

删除tooken就行了。

### 3.难点：其他服务器怎么知道是否登录

方案一：在Controller中取cookie中的token数据，调用sso服务查询用户信息。

方案二：当页面加载完成后使用js取token的数据，使用ajax请求查询用户信息。



方案一太麻烦了，而且每次都得重写一个controller。

方案二：简单，使用jsonp。Jsonp不是新技术，跨域的解决方案。使用js的特性绕过跨域请求。Js可以跨域加载js文件。

![1562576145376](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1562576145376.png)

## 具体js代码：

```
$(function () {
    var _ticket = $.cookie("token");
    alert("token是"+_ticket);
    if(!_ticket){
      return ;
    }
    $.ajax({
      url : "http://localhost:8088/user/token/" + _ticket,
      dataType : "jsonp",
      type : "GET",
      success : function(data){
        if(data.status == 200){
          var username = data.data.username;
          var html = username + "，欢迎来到宜立方购物网！<a href=\"http://www.e3mall.cn/user/logout.html\" class=\"link-logout\">[退出]</a>";
          $("#loginbar").html(html);
        }
      }
    });
  });
```

### 服务端：

```
@Autowired
	private TokenService tokenService;
	
	@RequestMapping(value="/user/token/{token}",
			produces= MediaType.APPLICATION_JSON_UTF8_VALUE+"application/json;charset=utf-8")
	@ResponseBody
	public String getUserByToken(@PathVariable String token, String callback) {
		E3Result result = tokenService.getUserByToken(token);
		//响应结果之前，判断是否为jsonp请求
		if (StringUtils.isNotBlank(callback)) {
			//把结果封装成一个js语句响应
			return callback + "(" + JsonUtils.objectToJson(result)  + ");";
		}
		return JsonUtils.objectToJson(result);
	}
```

### 思考：

主要两个难点：1.生成的token存放cookie中，需要解决cookie跨域的问题。(使用cookieUtils)

​							2.其他服务模块使用js发送jsonp，根据token查用户是否登录。