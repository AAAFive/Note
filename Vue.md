# `Vue`

## 下载

[开发版](https://cn.vuejs.org/js/vue.js)

[生产板](https://cn.vuejs.org/js/vue.min.js)

[axios](https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js)



## 入门

### 声明式渲染

```html
<div id="app">
	{{ msg }}
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        msg: '你好 世界!'
    }
})
```



### 指令

`vue`的指令以`v-`开头，常用的指令有：

- `v-bind:属性名`
- `v-model`
- `v-if`
- `v-for`
- `v-html`
- `v-noce`
- `v-on:事件名`





### 绑定元素属性

使用`v-bind`指令将数据给元素属性

```html
<div id="app">
  <span v-bind:title="message">
    鼠标悬停在此，显示信息！
  </span>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```



### 条件

使用`v-if`指令判断条件是否成立

```html
<div id="app">
    <p v-if="show">
        show 的值为 true，该消息显示
    </p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    show: true
  }
})
```



### 循环

使用`v-for`指令循环遍历数据

```html
<div id="app">
    <ul>
        <li v-for="user in list"> {{ user }} </li>
    </ul>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    list: [ 'five','admin','root' ]
  }
})
```



### 事件监听

使用`v-on:事件名`指令进行事件监听

```html
<div id="app">
    <button v-on:click="sayHi">click me!</button>
</div>
```

```js
var app = new Vue({
  el: '#app',
  methods: {
 	sayHi: function(){
        alert('你好 世界!')
    }     
  }
})
```



### 数据双向绑定

使用`v-model`指令进行数据双向绑定

```html
<div id="app">
    <input type="text" placeholder="姓名" v-model="name">
   	<p> 你好 {{ name }} </p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
      name: ""
  }
})
```





## 模板语法

### 插值

#### 文本

```html
<span>消息: {{ msg }}</span>
```

`v-once`指令，执行一次性地插值，当数据改变时，插值处的内容不会更新

```html
<span v-once>{{ msg }}</span>
```



#### HTML

`v-html`指令将不转义 `HTML`标签

```html
<div id="app">
    <p v-html=""></p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
      html: "<h1>你好 世界!</h1>"
  }
})
```



#### 属性

`v-bind`指令将值绑定在元素属性上

```html
<div id="app">
    <input v-bind:type="type">
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
      type:"text"
  }
})
```



#### `JavaScript`表达式

```html
{{ number + 1 }}

{{ ok ? 'yes' : 'no' }}

{{ alert('你好 世界') }}

<div  v-html=" '<h1>你好</h1>' "></div>
```



### 指令缩写

#### `v-bind`缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```

#### `v-on`缩写

```html
<!-- 完整语法 -->
<a v-on:click="sayHi">...</a>

<!-- 缩写 -->
<a @click="sayHi">...</a>
```





## 计算属性和侦听属性

### 计算属性 `computed`

```php+HTML
<div id="app">
    <input type="text" v-model="msg">
    <p>UpperCase: {{ upperMsg }} </p>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
       msg: '' 
    },
    computed: {
        upperMsg: function(){
            return this.msg.toUpperCase();
        }
    }
})
```

#### 计算属性的 `getter` 和`setter`

```js
var app = new Vue({
    el: '#app',
    data:{
       msg: '' 
    },
    computed: {
        upperMsg: {
            get: function(){
            	return this.msg.toUpperCase();
        	},
            set: function(val){
                return this.val.toUpperCase()
            }
        }
    }
})
```



### 侦听属性 `watch`

```html
<div id="app">
    <input type="text" v-model="msg">
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
       msg: '' 
    },
    watch:{
        msg:function(newVal,oldVal){
            alert(`msg{ oldVal:${oldVal}, newVal:${newVal} }`)
        }
    }
})
```





## `class`与 `sytle`绑定

### 绑定 `class`

#### 基于对象的语法

```css
.active{
    color: pink;
}
.mouse-over:hover{
    color: pink;
}
.default{}
```

```html
<div id="app">
    <a href="#" v-bind:class="{ active: isActive }" >click me!</a>
    <a href="#" class=".default" v-bind:class="{ active: isActive, 'mouse-over': hover }" >click me!</a>
    <a href="#" v-bind:class="classList" >click me!</a>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
        isActive: true,
        hover: false,
        classList:{
            isActive: true,
            hover: false,
        }
    },
})
```

#### 基于数组的语法

```css
.active{
    color: pink;
}
.mouse-over:hover{
    color: yellow;
}
.default{}
```

```html
<div id="app">
    <a href="#" v-bind:class="[ active, mouseover ]" >click me!</a>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
        active: 'active',
        mouseover: 'mouse-over'
    }
})
```



### 绑定 `style`

#### 基于对象的语法

```html
<div id="app">
    <a href="#" v-bind:style="{ color: 'pink', 'font-size':'18px' }" >click me!</a>
    <a href="#" v-bind:style="{ color: color, 'font-size':fontsize }" >click me!</a>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
        color: 'red',
        fontsize: '18px',
    }
})
```

#### 基于数组的语法

```html
<div id="app">
    <a href="#" v-bind:style="[color,font]" >click me!</a>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
        color: {
            color: 'pink',
            background: 'blue'
        },
        font: {
           	fontSize: '20px'
        }
    }
})
```





## 条件渲染 `v-if`

### `v-if`

```html
<h1 v-if="show"> show == true，该元素将会显示 </h1>
```

### `v-else`

```html
<h1 v-if="show"> show == true，该元素将会显示 </h1>
<h1 v-else> show == false，该元素将会显示 </h1>
```

### `v-else-if`

```html
<h1 v-if="number==1"> number == 1，该元素将会显示 </h1>
<h1 v-else-if="number==2"> number == 2，该元素将会显示 </h1>
<h1 v-else-if="number==3"> number == 3，该元素将会显示 </h1>
<h1 v-else>  以上条件不成立，该元素将会显示 </h1>
```

### 用 `key` 管理可复用的元素

`Vue`会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。

```html
<div v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username">
</div>
<div v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="Email">
</div>
```

### `v-show`

`v-show` 只是简单地切换元素的 `CSS` 的`display`。

```html
<h1 v-show="show">你好 世界!</h1>
```





##  列表循环 `v-for`

### 遍历数组

```html
<div id="app">
    <ul>
        <li v-for='user in users'>{{ user }}</li>
    </ul>
</div>
```

```js
var app = new Vue({
    el: "#app",
    data:{
        users:[ 'five','root','admin' ]
    }
})
```

可选的第二个参数，元素的下标

```html
<div id="app">
    <ul>
        <li v-for='(user,index) in users'>[ {{ index }} ]: {{ user }} </li>
    </ul>
</div>
```

```js
var app = new Vue({
    el: "#app",
    data:{
        users:[ 'five','root','admin' ]
    }
})
```



### 遍历对象

```html
<div id="app">
    <ul>
        <li v-for="info in user">{{ info }}</li>
    </ul>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
        user:{
            name: 'five',
            sex: '男',
            age: 18
        }
    }
})
```

可选的第二个参数，对象的key

```html
<div id="app">
    <ul>
        <li v-for="(info,key) in user">{{key}}: {{ keyinfo }}</li>
    </ul>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
        user:{
            name: 'five',
            sex: '男',
            age: 18
        }
    }
})
```

可选的第三个参数，对象的下标

```html
<div id="app">
    <ul>
        <li v-for="(info,key,index) in user">{{index}} - {{key}}: {{ keyinfo }}</li>
    </ul>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data:{
        user:{
            name: 'five',
            sex: '男',
            age: 18
        }
    }
})
```

### 维护状态

当 `Vue` 正在更新使用 `v-for` 渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，`Vue` 将不会移动 `DOM` 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染

为了给 `Vue` 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key`

```html
<li v-for="user in users" v-bind:key="user.id">
  <!-- 内容 -->
</li>
```

### 在 `v-for`迭代数字

```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```





## 事件处理 `v-on`

### 事件绑定

```html
<div id="app">
    <button v-on:click="sayHi">click me</button>
    <button v-on:click="say('Five')">click me</button>
    <button v-on:click="getE">click me</button>
</div>
```

```js
var app = new Vue({
    el:"#app",
    method: {
        sayHi: function(){
            alert("你好")
        },
        say:function(name){
            alert("你好 "+name)
        },
        getE: function(e){
            console.log(e)
        },
        
    }
})
```

### 事件修饰符

- `.stop `阻止单击事件冒泡
- `submit.prevent` 提交事件不再重载页面
- `.capture`
- `.self`
- `.once` 只能激活一次
- `scroll.passive` 滚动事件的默认行为 (即滚动行为) 将会立即触发

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

### 按键修饰符

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">

<input v-on:keyup.page-down="onPageDown">
```

### 系统修饰键

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

```html
<!-- Alt + C -->
<input v-on:keyup.alt.k="clear">

<!-- Ctrl + Click -->
<div v-on:click.ctrl="doSomething">Do something</div>
```

### `.exact`修饰符

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button v-on:click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button v-on:click.exact="onClick">A</button>
```

### 鼠标修饰符

- `.left`
- `.right`
- `.middle`







## 数据双向绑定 `v-model`

### 文本

```html
<input type="text" v-model="msg">
<p> {{ msg }} </p>
```

### 多行文本

```html
<text-area v-model="msg" ></text-area>
<p> {{ msg }} </p>
```

### 复选框

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">flag: {{ checked }}</label>
```

#### 多个复选框绑定到一个数组

```html
<div id="app">
    <input type="checkbox" id="jack" value="five" v-model="users">
    <label for="five">five</label>

    <input type="checkbox" id="john" value="root" v-model="users">
    <label for="root">root</label>

    <input type="checkbox" id="mike" value="admin" v-model="users">
    <label for="admin">admin</label>
    <br>
    <span> userinfo: {{ users }}</span>
</div>
```

```js
new Vue({
  el: "#app",
  data: {
    users: []
  }
})
```

#### 选中值和未选中值

```html
<!--
	当选中时 toggle == 'yes'
	未选中时 toggle == 'no'
-->
<input
  type="checkbox"
  v-model="result"
  true-value="yes"
  false-value="no"
>
```

### 单选按钮

```html
<div id="app">
  <input type="radio" id="man" value="man" v-model="sex">
  <label for="man">man</label>
  <br>
  <input type="radio" id="wuman" value="wuman" v-model="sex">
  <label for="wuman">wuman</label>
  <br>
  <span>sex: {{ sex }}</span>
</div>
```

```js
new Vue({
  el: '#app',
  data: {
    sex: ''
  }
})
```

### 下拉列表框

#### 单选时

```html
<div id="app">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

```js
new Vue({
  el: '#app'
  data: {
    selected: ''
  }
})
```

#### 多选时 (绑定到一个数组)

```html
<div id="app">
  <select v-model="selected" multiple>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
```

```js
new Vue({
  el: '#app',
  data: {
    selected: []
  }
})
```

### 修饰符

#### `.lazy`

`change` 事件之后进行同步

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

#### `.number`

自动将输入的值转为数值类型

```html
<input v-model.number="age" type="number">
```

#### `.trim`

自动过滤首尾的空白字符

```html
<input v-model.trim="msg">
```



##  组件基础

### 组件注册

#### 全局注册

```html
<div id="app">
  <button-counter></button-counter>
</div>
```

```js
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">点击了 {{ count }} 次</button>'
})

var app = new Vue({
    el:"#app"
})
```

#### 局部注册

```html
<div id="app">
  <button-counter></button-counter>
</div>
```

```js
var app = new Vue({
    el:"#app",
    components:{
        'button-counter':{
            data: function () {
                return {
                  count: 0
                }
 			},
            template: '<button v-on:click="count++">点击了 {{ count }} 次</button>'
        }
    }
})
```



### 通过`props`向组件传递数据

```html
<div id="app">
  <say name="five"></say>
  <say name="root"></say>
  <say name="admin"></say>
</div>
```

```js
Vue.component('say', {
  props: ['name'],
  template: '<h3> 你好 {{ name }}</h3>'
})

var app = new Vue({
    el: "#app"
})
```

### 监听子组件事件

```html
<div id="app">
	<click v-on:say="say"></click>
</div>
```

```js
Vue.component('click', {
  template: `<button v-on:click="$emit('say')">click me</button>`
})

var app = new Vue({
    el: "#app",
    methods: {
        say: function(){
            alert("你好")
        }
    }
})
```

### 事件抛值

```html
<div id="app">
	<click v-on:say="say($event)"></click>
</div>
```

```js
Vue.component('click', {
  template: `<button v-on:click="$emit('say','ok')">click me</button>`
})

var app = new Vue({
    el: "#app",
    methods: {
        say: function(e){
            alert(e)
        }
    }
})
```

### 传递标签的内容 `<slot></slot>`

```html
<div id="app">
	<click>click me</click>
</div>
```

```js
Vue.component('click', {
  template: `<button><slot></slot></button>`
})

var app = new Vue({
    el: "#app"
})
```

### 动态组件

```html
<div id="app">
    <component v-bind:is="component"></component>
</div>
```

```js
Vue.component('click', {
    template: `<button>click me</button>`
})

var app = new Vue({
    el: "#app",
    data:{
        component: 'click'
    }
})
```









