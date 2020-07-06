1.ajax进行跨域请求，默认情况就是可以进行跨域请求，但是后端需要进行使用注解。

```
@CrossOrigin(origins = "*")
```

```
function saveGoCart(id) {
    $.ajax({
        type: 'post',
        url: 'http://localhost:9097/addCartByUserId',
        data: data,
        success: function (result) {
            if (result.status == 200) {
            	alert("成功");
                ).then((flag) => {
                    window.location.href="http://localhost:9097/getAllCartByUserId";
            }
            } else {
                
            }
        },
        error: function () {
            swal("操作失败", {
                icon: "error",
            });
        }
    });
}
```

2.在sso单点登录时，可能需要携带cookie(ajax默认是不允许携带的)，取token。需要添加以下，携带cookie

crossDomain:true, //设置跨域为true

xhrFields: {
 withCredentials: true //默认情况下，标准的跨域请求是不会发送cookie的
  },

但是会出现问题，即使ajax有返回结果，也不会进入success，进入error。

所以后端需要添加如此@CrossOrigin(origins = "*",allowCredentials="true")。

```
function saveGoCart(id) {
    $.ajax({
        type: 'post',
        crossDomain:true, //设置跨域为true
        xhrFields: {
            withCredentials: true //默认情况下，标准的跨域请求是不会发送cookie的
        },
        url: 'http://localhost:9097/addCartByUserId',
        data: data,
        success: function (result) {
            if (result.status == 200) {
            	alert("成功");
                ).then((flag) => {
                    window.location.href="http://localhost:9097/getAllCartByUserId";
            }
            } else {
                
            }
        },
        error: function () {
            swal("操作失败", {
                icon: "error",
            });
        }
    });
}
```

