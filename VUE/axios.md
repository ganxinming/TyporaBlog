1.快速简便的接口调用

2.简单使用

```
<script src="js/axios.js"></script>
<script type="text/javascript">
  axios.get('http://localhost:3000/ss?id=3').then(function (value) {
    console.log(value.data);
    //value不是真的data，需调用data(固定的)属性，其实打印value会发现他就像json，包含很多属性，而我们需要data
  });

  axios.get('http://localhost:3000/ss/3').then(function (value) {
    console.log(value.data);
    //value不是真的data，需调用data(固定的)属性，其实打印value会发现他就像json，包含很多属性，而我们需要data
  });

  //也可以自定义传入的参数
  axios.get('http://localhost:3000/ss',{
    params :{
      id : 3,
      usernme : 'cc'
    }
  }).then(function (value) {
    console.log(value.data);
    //value不是真的data，需调用data(固定的)属性，其实打印value会发现他就像json，包含很多属性，而我们需要data
  });

  //全局配置,配置基本路径
  axios.defaults.baseURL='http://localhost:3000/';
  axios.defaults.timeout=3000;//超时时间
  //post请求，后面可直接封装参数，默认封装成json格式。
  axios.post('/jj',{
    username: 'jj',
    age : 9
  }).then(function (value) {
    console.log(value.data.username);
  });

</script>
```



