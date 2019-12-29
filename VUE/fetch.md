1.可以认为是ajax的升级版，也是发送请求

使用方法:

```
<script type="text/javascript">
  //普通请求
  fetch('http://localhost:3000/jj').then(function (data) {
      return data.text();
      //第一只是返回一个promise对象，并不能直接打印返回值，把promise传递给下个在使用then读出
  }).then(function (data) {
    console.log(data); //
  });
  //接受json返回值
  fetch('http://localhost:3000/gg').then(function (data) {
    return data.json();
  }).then(function (data) {
    console.log(data.username);
  });
  
  //参数
  fetch('http://localhost:3000/kk',{
   method: "post",
   body: "size=2&page=10",
   headers: {
     'Content-Type' : 'application/x-wwww-form'
   },
  }
  ).then(function (data) {
    return data.text();
    //第一只是返回一个promise对象，并不能直接打印返回值，把promise传递给下个在使用then读出
  }).then(function (data) {
    console.log(data); //
  });

//传输json数据
  fetch('http://localhost:3000/mm',{
      method: "post",
      body: JSON.stringify({
        username : 'abc',
        age : 12
      }),
      headers: {
        'Content-Type' : 'application/json'
      },
    }
  ).then(function (data) {
    return data.text();
    //第一只是返回一个promise对象，并不能直接打印返回值，把promise传递给下个在使用then读出
  }).then(function (data) {
    console.log(data); //
  });
</script>
```

