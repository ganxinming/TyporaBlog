Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用.

1.运行原理:具有编译过程，将vue代码通过vue框架转换成js代码。

2.前端渲染:将后台数据(ajax请求)嵌入前端html代码中。



1.感觉其实和jquery差不多，就是vue没有直接拿dom操作，而jquery都是直接操作dom。

2.他是通过创建一个vue实例完成来进行数据绑定和显示。

一：起步

1.引入js文件,简单使用

```
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<div id="app">
  {{ message }} //插值表达式，将数值插入到html中
</div>

<script>
var vm = new Vue({
  el: '#app',//元素的挂载位置，可以是dom元素或css选择器
  data: {  //需要绑定数据名称，模型数据
    message: 'Hello Vue!'
  }
})
<script>
```

不同于jquery，他不是创建dom对象，而是vue对象。通过vue，现在数据和 DOM 已经被建立了关联，所有东西都是**响应式的**

**并且自定义方法发生变化：**

```
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>

var vm = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

获取属性:直接vm.a(虽然他前面是data，但是还是可以直接通过.得到)

```
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data//其实data就是个key-value结构
})

// 获得这个实例上的属性
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2
//使用这个方法，可以停止数据动态绑定，即data就是原来的值不能改变
Object.freeze(data)
```

生命周期钩子函数:顾名思义在vue对象创建时执行的函数，除了created还有destoryed，updated等。

```
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
```

#### 因此一个Vue对象大概结构如下：

```
new Vue({
  el: '#add',
  data: {
    a: 1
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
```

数据绑定(将data中的属性显示到页面)：Mustache 语法

```
<span>Message: {{ msg }}</span> //直接使用{{}}选中属性即可
```

双大括号会将数据解释为普通文本，而非 HTML 代码，为了输出真正的 HTML，你需要

使用 `v-html` 指令：

```
`<p>Using mustaches: {{ rawHtml }}</p><p>Using v-html directive: 
<span v-html="rawHtml"></span></p>`
```

这里引出一个概念，指令。所有以v-开头都是指令。指令中的值无需{{}}，直接写data属性

所以只有在文本中使用{{}}Mustache 语法,而指令中可以直接写属性。那其他html怎么使用这个属性？

使用v-bind和v-on

比如在普通html中加入:

```
<div v-bind:id="dynamicId"></div> //相当于在id属性用dynamicId代替
<button v-bind:disabled="isButtonDisabled">Button</button>
<a v-bind:href="url">...</a>
```

综上:

v-bind: 原属性=“” 就可以使用

`v-on` 指令，它用于监听 DOM 事件：相当于取代了onclick

```
<a v-on:click="doSomething">...</a>
```

以上可以缩写:

```
`<!-- 完整语法 -->
<a v-bind:href="url">...</a>
<!-- 缩写 -->
<a :href="url">...</a>
<a v-on:click="doSomething">...</a>
<!-- 缩写 -->
<a @click="doSomething">...</a>`
```

### vue绑定class和style

和传统的v-bind不一样，v-bind:class提供了不一样的形式，采用key-value形式，动态绑定属性，可以自由切换

```
`<div class="static"     v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>`
```



调用后台接口:

调用后台接口:原先是使用ajax，由于不在使用jquery操作dom，所以衍生了fetch(可以认为ajax升级版)和axios。

 fetch是一种HTTP数据请求的方式，是XMLHttpRequest的一种替代方案 。

(终于知道restful风格的请求为什么需要从路径拿参数而不是数据封装到属性了

比如:http://www.jax.com/book/23 GET 和 http://www.jax.com/book/23 DELETE

虽然url一样，但是方式不一样，一个是获取书一个是删除书。在于请求方式不同。但是传统的url就是路径后面带参数进行访问，所以可以封装。 所以restful是根据请求方式处理不同请求。(且参数就是路径 )

)

