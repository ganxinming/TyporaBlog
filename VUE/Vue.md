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

不同于jquery，他不是创建dom对象，而是vue对象。通过vue，现在数据和 DOM 已经被建立了关联，所有东西都是**响应式的**（更改完data属性，立即改变。跟jq不同，jq需要获得节点然后通过val()方法改值。）

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

Mustache 语法:{{里面的值可以进行计算和字符串拼接等}}

所以只有在文本中使用{{}}Mustache 语法,而指令中可以直接写属性。那其他html怎么使用这个属性？

使用v-bind和v-on:相当于绑定属性

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

虽然url一样，但是方式不一样，一个是获取书一个是删除书。在于请求方式不同。但是传统的url就是路径后面带参数进行访问，所以可以封装。 所以restful是根据请求方式处理不同请求。(且参数就是路径 ）

组件:全局组件和局部组件

1.全局组件:即定义在vue实例外面的。

1.定义的data可以认为是个方法，但是返回值是属性，主要作用隔离每个模板属性，相互独立。就相当于count属性是隔离的，每个模板独立。

2.定义的props:相当于自定义属性，在使用模板时可以为属性赋值。

data和props不同的是:data中的可能我们需要计算操作的值保存在各自的模板。props是传入时自定义的属性值。

```
Vue.component('zhujian', {
    data: function () {
      return {
        count: 0
      }
    },
    props:['tit'], //可以自定义无限个属性，在使用该模板时可以给属性赋值,超强
    template: '<button v-on:click="count++">赋值属性 {{tit}}</button>'
  });
```

2.局部组件:即定义在vue里面的。(挂载vue元素以外的不能使用)

模板底层工作原理:

1.底层有extend()自定义模板方法。

2.注册组件，给予标签名。

3.使用标签。

## 注意:

1.data属性返回一个对象，里面封装了属性。目的是为了隔离不同模板不同域。

2.template:因为里面主要是html代码，可以抽取出来，用template代替。



模块化开发:在早期，每个人写js代码都会抽取来成为js文件。但是在引入时，可能会造成各个js文件中变量名冲突和覆盖。那早期怎么解决呢？通过闭包，使每个用到的变量在闭包作用域中有效。那么即使变量冲突也造不成影响。但是使用闭包又有其他问题，假如闭包中的变量又想要了呢？这又取不出来了，所以ES5的做法是在闭包中返回一个对象，对象中存了所有的变量。又因为闭包方法会返回这个对象，通过对象可以取到需要的值，而且不用担心变量名覆盖重复等问题。

当然了，那是ES5之前的做法，所以后来才会衍生出模块化开发。

#### 就是将自己写的代码封装成一个模块，然后导出 export ，别人导入 import 需要的变量。(其实原理和原来的ES5差不多)

Vue常用指令:

v-once :只显示开始的值，不随后面改变。

v-text:直接将data里的属性显示进标签理。

v-html:以html的形式显示。(所以那个属性需要有标签的字符)

v-pre:忽略Mustache语法，直接显示值

v-bind：被设置的属性中的值不在是普通字符串，而是data中的值

v-if ,v-if else ,v-else : 就相当于if else

v-for ：遍历对象(含有唯一标识id)数组，最好加上:key,保证唯一，提高速度。有了id就可以更改每个的值了。

v-show：展示元素

v-model:双向绑定，无论修改input还是data的属性，两者都会影响。