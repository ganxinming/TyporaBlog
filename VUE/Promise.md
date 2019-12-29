1.了解promise语法:它是异步编程的一种解决方法，它可以获取异步操作的消息。(例如：ajax就是异步的)

好处：1.解决回调地狱。(即：ajax中调用ajax，保证异步有序(即保证两个ajax是顺序的，如果不嵌套就不一定是顺序的)，因为这样套用调用多了就很难看了)

(其实就是通过处理两套逻辑(成功或失败)，将嵌套改成线性结构变成if走成功，else走失败)

**发送多个ajax可以使用多个then来接受返回值，即p.then(function...).then(function..).then(function)。**

```
<script type="text/javascript">
  // console.log(456);
  // console.log(typeof  Promise);
  // console.dir(Promise);
  var p=new Promise(function (resolve,reject) {
    var flag=false;
    var item;
    //根据flag判断两种逻辑
    if (flag){
      item=resolve1();/执行成功的函数
      resolve(item);//成功时调用
    }
    else{
      item=reject1();//执行失败的函数
      reject(item);
    }
  });
  //获取两种逻辑的结果
  p.then(function (result) {
    console.log(result);
  },function (info) {
    console.log(info);
  });

  function resolve1() {
    return "java";
  }
  function reject1() {
    return "C";
  }
</script>
```

