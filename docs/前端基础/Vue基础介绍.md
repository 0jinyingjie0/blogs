## Vue基础介绍

### 特点

- 响应式数据：数据驱动视图，数据变化则视图则一定变化。
- MVVM：双向绑定
- 指令：一种指令对应一种功能
- 组件化开发：抽提封装

## 基础实例选项

### el

- 作用：当前Vue实例所管理的html视图
- 值：通常是id选择器、class选择、dom选择

### data

- 响应式数据的数据变化
- Vue 实例**`vm`**也代理了 data 对象上所有的属性，因此访问 `vm.a` 等价于访问 `vm.$data.a`

### methods

- methods是一个对象，可以直接通过 VM 实例访问这些方法，或者在`指令表达式中使用`
- 方法中的 `this` 自动绑定为 Vue 实例**`vm`**。

### 插值表达式

- 作用:会将绑定的数据实时的显示出来: =>响应式数据
- 形式: 通过 **`{{ 插值表达式 }}`**包裹的形式 

### 指令

- 语法`**  v-指令=“表达式”  如果 表达式想要是一个字符串 就必须这样写（用单引号包裹）  v-指令=**`" '字符串' "`**，否则会被当做一个 data数据中的变量

## 系统指令

### v-text 和 v-html

- v-text:更新标签中的内容
- v-html:更新标签中的内容/标签
- v-text和插值表达式的区别
  - v-text  更新**`整个`**标签中的内容
  - 插值表达式: 更新标签中局部的内容

### v-if 和 v-show

- 使用: v-if 和 v-show 后面的表达式返回的布尔值 来决定 该元素显示隐藏
- `v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。

### v-on绑定事件

- 使用: 绑定 **`v-on:事件名.修饰符="方法名"`**   可使用 **`@事件名="方法名的方式"`**
- `.once` - 只触发一次回调。
- `.prevent` - 调用 `event.preventDefault()`。
- **`input只要内容改变就会触发 change只有离开焦点才会触发`**

###  v-for-数组

-  `v-for` 指令需要使用 `item in items` 或者 `item of items` 形式的特殊语法
- `items` 是源数据数组 /对象
- (item, key, index) in  items //item为当前遍历属性对象的值 key为当前属性名的值  index为当前索引的值

### v-for-key

```js
<li v-for="(item,index) in list" :key="index">{{item}}---{{index}}</li>
```

### v-if和v-for相遇

```js
<p v-if="index>1" v-for="(item,index) in list"></p>
```

- v-for 的优先级大于v-if ,所有v-if才能使用v-for的变量

### v-bind

- 作用:绑定标签上的任何属性

#### v-bind-绑定class-对象语法

- 绑定class对象语法    :class="{ class名称": 布尔值 }"

```js
   <p :class="{left:showClass}">内容</p>
```

#### v-bind-绑定class-数组语法

- 绑定class数组语法 :class="[class变量1,class变量2..]"

```js
<p :class="[activeClass,selectClass]" class="default">内容</p>
```

#### v-bind-绑定style-对象语法

- 语法: :style="{css属性名: 变量}"

```js
<p :style="{fonSize:fontsize}"></p>
```

#### v-bind-绑定style-数组语法

- 语法:  :style="[对象1,对象2...]"

### v-model-基础用法

- **`mvvm`** =>  Model(数据模型) View (视图)  ViewModel(视图与数据绑定关系)
- 作用:表单元素的绑定 input/checkbox/textarea
- 特点: **`双向数据绑定`**
  - 数据发生变化可以更新到界面 
  - 通过界面可以更改数据 =>  表单来修改
  - `v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` 特性的初始值而总是将 Vue 实例的数据作为数据来源。应该在 `data`选项中声明初始值。

- v-model-语法糖原理
  - 复杂语法 简写写法 => v-on:click="方法"  @click="方法"

### 系统指令-v-cloak

- 场景: 解决页面初次渲染时 页面模板闪屏现象

```html
<div v-cloak id="app">
<p>{{ name }}</p>
</div>
```

```css
[v-cloak] {
  display: none;
}
```

### v-once

- 作用: 使得所在元素只渲染一次  

## filter

### 全局过滤器

- 在创建 Vue 实例**`之前`**定义全局过滤器Vue.filter()
- Vue.filter('该过滤器的名字',(要过滤的数据)=>{return 对数据的处理结果});

```js
Vue.filter("toUpper", function(value) {
return value.charAt(0).toUpperCase() + value.substr(1);
});// 过滤器核心代码
```

```js
<p>{{ text | toUpper }}</p>  // 常规用法
```

### 局部过滤器

- 在vm对象的选项中配置过滤器filters:{}
- 过滤器的名字: (要过滤的数据)=>{return 过滤的结果}

### 传参数和串联使用

- 过滤器可以传递参数,第一个参数永远是前面传递过来的过滤值
- 过滤器也可以多个串行起来并排使用

## ref 操作 DOM

- 作用: 通过ref特性可以获取元素的dom对象
- 使用:  给元素定义 ref属性, 然后通过$refs.名称 来获取dom对象

```js
<input type="text" ref="myInput" /> // 定义ref
```

```js
focus() {
  this.$refs.myInput.focus();
}  // 获取dom对象
```

## 自定义指令

### 全局自定义指令

- 在创建 Vue 实例之前定义全局自定义指令Vue.directive()
- Vue.directive('指令的名称',{ inserted: (使用指令的DOM对象) => { 具体的DOM操作 } } );
- 在视图中通过"v-自定义指令名"去使用指令

```js
Vue.directive("focus", {
        inserted(dom) {
          dom.focus();
        }
      });
```

### 局部自定义指令

```js
directives: {
focus: {
  inserted(dom) {
    dom.focus();
  }
}
} // 局部自定义指令实现
```

## 计算属性

### 应用场景

- 场景:当插值表达式过于复杂的情况下 可以采用计算属性 对于任何复杂逻辑都可以采用计算属性
- 使用: 在Vue实例选项中 定义 computed:{ 计算属性名: **`带返回值`**的函数 }
- 计算属性**`必须有返回值`** 相当于对插值表达式**`逻辑的一次封装`**

### 基本使用

- Vue实例代理了 Data中所有的属性,methods方法,代理了计算属性中所有的计算属性/props
- 定义计算属性时 => 不能data中数据重名,不能和方法中的方法重名

### 计算属性和methods方法的区别

- 计算属性不需要调用形式的写法  而methods方法必须采用 方法() 调用的形式
- 计算属性依赖data中的数据变化,如果data并没有发生变化,则计算属性则会取缓存的结果,
- methods不论data变化与否都会重新调用

## RESTFUL的接口规则

- RESTful是一套接口设计规范
- 用**`不同的请求类型`**发送**`同样一个请求标识`** 所对应的处理是`不同的`
- 同样的请求标识 =>同一个地址
- 不同的类型 => get/post/put/delete
- 通过Http请求的不同类型(POST/DELETE/PUT/GET)来判断是什么业务操作(CRUD ) 
- CRUD => **`增删改查`**
- json-server应用了RESTful规范

## watch

- 场景: 当需要根据数据变化 进行相应业务操作,且该操作是**`异步操作`**时,计算属性不能再使用,可以使用监听watch特性
- 计算属性和watch区别
  - 计算属性 必须要有返回值 所以说不能写异步请求 因为有人用它的返回值(插值表达式)
  - watch选项中可以写很多逻辑 不需要返回值 因为没有人用它的返回值

## 组件

- 组件特点: 组件是一个**`特殊的 Vue实例`** => Vue实例的选项 组件基本都有
- 没有el => 有**`template`**=> HTML模板  => 有且只有一个根节点
- 有 data,但是data是一个函数 => 是一个**`带返回值`**的函数 **`返回值是一个对象`**  => 对象里面是我们需要用的响应式数据

### 全局组件

```js
// 注册组件名称 推荐 小写字母 加横向的结构
      Vue.component("content-a", {
        template: `<div>
        {{count}}
        </div>`,
        data() {
          return {
            count: 1
          };
        }
      });
```

### 局部组件

```js
new Vue({
el: "#app",
data: {},
methods:{ },
components:{
  "content-a": {
      template: `<div>
  {{count}}
  </div>`,
  data() {
    return {
      count: 1
    };
  }
}
})
```

### 组件嵌套

- 在注册自定义组件时,嵌套另一个自定义组件,也就是**`父子组件`**的关系 => 嵌套关系
- 局部组件 => 在谁的组件上注册 就只能在谁的**`组件模板(template)`**中使用

```js
  // vue实例可以看成是 一个最根级的组件
     Vue.component("el-button", {
         template: `<div>
         el-button
         <el-child />
         </div>`
     })  // 在 el-button 中嵌套了 el-child => 父子组件 => el-button (父) el-child(子)

     Vue.component("el-child", {
         template: `<div>el-child
           <el-grand-child />
         </div>`
     })
     Vue.component("el-grand-child", {
         template: `<div>el-grand-child</div>`
     })
     //  Vue实例是父 elbutton子
     var vm = new Vue({
         el: '#app',
         data: {},
         methods: {}
     });

     //  vue实例(父) =>el-button
     // el-button(父) =>  el-child
     // el-child (父) =>el-grand-child
```

## 组件通信

- 父组件 =》 子组件   需要将数据传给子组件 
- 子组件 =》 父组件  如果父组件需要 子组件也可以传数据给父组件
- 兄弟组件1 =》兄弟组件2 => **`eventBus`** => Vuex

### 父组件给子组件传值Props

- props作用: 接收父组件传递的数据

- props就是父组件给子组件标签上定义的属性

- 定义传递的属性 => 标签上定义属性 => **`给谁传值 就在谁的标签上写属性`**

  ```js
  1. props是组件的选项  定义接收属性
  2. props的值可以是字符串数组 props:["list"]
  3. props数组里面的元素称之为prop(属性) 属性=?值
  4. prop的值来源于外部的(组件的外部)
  5. prop(我们这里是lists)是组件的属性->自定义标签的属性
  6. prop的赋值位置(在使用组件时,通过标签属性去赋值)
  7. prop的用法和data中的数据用法一样
  ```

- 用props完成父组件给子组件传值  传值的属性都是定义在 子组件的标签上,可以采用v-bind的形式传递动态值

### 子组件给父组件传值(自定义事件)

- 自定义事件 => **`$emit`** => 触发一个自定义事件
- 监听 $emit的事件 => **`监听谁的事件 就在谁的标签上写监听`** @事件名="方法"

```js
<div id="app">
   <p>{{currentCity}}</p>
   <!-- 监听谁的事件就在谁的标签上写监听 -->
   <city-li @changecity="receiveData" v-for="item in list" :name="item" :current="currentCity"></city-li>
</div>
<script src="./vue.js"></script>
<script>
   // 全局组件
   Vue.component("city-li", {
       template: `<li v-bind:class="{select: selectClass}" @click="clickCity" >{{name}}</li>`,
       props: ["name", "current"],
       methods: {
           clickCity() {
               //  this.属性 this.属性 (props)
               // alert(this.name)
               // 子组件  => 父组件
               // this.$emit(自定义的事件名称,若干参数1......)  自定义事件名 全小写
               this.$emit("changecity", this.name) // 相当于 我们在自己的组件内部 触发了一个事件
               // 触发的这个事件 可以监听到  
           }
       },
       computed: {
           selectClass() {
               // this指向 组件实例
               return this.name === this.current
           }
       }
   })
   var vm = new Vue({
       el: '#app',
       data: {
           list: ["北京", "上海", "天津", "深圳"],
           currentCity: ''
       },
       methods: {
           receiveData(selectCity) {
               this.currentCity = selectCity
           }
       }
   });
</script>
```

## 单页应用-SPA的特点

### 优点

- 用户体验好,因为前段操作几乎感受不到网络的延迟
- 完全组件化开发 ,由于只有一个页面,所以原来属于一个个页面的工作被归类为一个个**`组件`**.

### 缺点

- **`首屏`**加载慢->**`按需加载`** 不刷新页面 只请求js模块
- 不利于SEO->**`服务端渲染`**(node->自己写路由->express-art-template+res.render())
- **`开发难度高`**(框架) 相对于传统模式,有一些学习成本和应用成本

### 实现原理

- 可以通过页面地址的锚链接来实现spa
- hash(锚链接)位于链接地址 **`#`**之后
- hash值的改变**`不会触发`**页面刷新
- hash值是url地址的一部分,会存储在页面地址上 我们可以获取到
- 可以通过**`事件监听`**hash值得改变
- 拿到了hash值,就可以根据不同的hash值进行不同的**`模块切换`**

## 路由

### 文档

- Vue-Router 是 [Vue.js](http://cn.vuejs.org/) 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建**单页面**应用变得易如反掌   它是一个插件
- 实现根据不同的请求地址 而显示不同的组件
- 如果要使用 vue开发项目,前端路由功能**`必须使用`**vue-router来实现

### 使用步骤

- 导入vue和vue-router

```js
<!-- router-link 最终会被渲染成a标签，to指定路由的跳转地址 -->
<router-link to="/users">用户管理</router-link>

<!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

- 创建组件

```js
<!-- router-link 最终会被渲染成a标签，to指定路由的跳转地址 -->
<router-link to="/users">用户管理</router-link>

<!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

- 实例化路由对象

```js
// 配置路由规则
var router = new VueRouter({
routes: [
   { name: 'home', path: '/', component: Home },
   { name: 'users', path: '/users', component: Users }
]
});
```

- vue实例挂载router实例

```js
var vm = new Vue({
el: '#app',
router
});
```

### 动态路由

- 点击**`列表页`** 跳转到**`详情页`**时,跳转的链接需要携带参数,会导致**`path`**不同
- 路由规则中增加参数，在path最后增加 :id

```js
{ name: 'users', path: '/users/:id', component: Users },
```

- 通过 <router-link> 传参，在路径上传入具体的值

```js
 <router-link to="/users/120">用户管理</router-link>
```

- 在组件内部可以使用，**this.$route** 获取当前路由对象

```js
 var Users = {
     template: '<div>这是用户管理内容 {{ $route.params.id }}</div>',
     mounted() {
         console.log(this.$route.params.id);
     }
 };
```

### vue-router-to

- to的多种赋值方式

```js
<!-- 常规跳转 -->
      <!-- <router-link to="/sport">体育</router-link> -->
      <!-- 变量 -->
      <!-- <router-link :to="path">体育</router-link> -->
      <!-- 根据对象name跳转 -->
      <!-- <router-link :to="{name:'abcdefg'}">体育</router-link> -->
      <!-- 根据对象path跳转 -->
      <!-- <router-link :to="{path:'/sport'}">体育</router-link> -->
      <!-- 带参数的跳转 -->
      <router-link :to="{name:'abcdefg',params:{a:1}}">体育</router-link>
```

### 重定向

```js
{
  path: "/sport",
  redirect: "/news", // 强制跳转新闻页
  component: {
    template: `<div>体育</div>`
  }
},
```

### 编程试导航

- (Vue实例)this.$router 可以拿到当前路由对象的实例
- 路由对象的实例方法 有 push  replace, go()  goBack
- push 方法 相当于往历史记录里推了一条记录 如果点击返回 会回到上一次的地址
- replace方法 想相当于替换了当前的记录  历史记录并没有多 但是地址会变
- go(数字) 代表希望是前进还是回退,当数字大于0 时 就是前进 n(数字)次,小于0时,就是后退n(数字)次

### vue-router-routerlink-tag-激活样式

- 当前路由在导航中是拥有激活class样式的

```js
<a href="#/news" class="router-link-exact-active router-link-active">新闻</a>
```

- router-link-active是一个固定的class => 该class默认值就是 router-link-active,可以变,

### 嵌套路由

- 如果存在**`组件嵌套`**,就需要提供多个视图容器<router-view></router-view>
- 同时,router-link和router-view 都可以添加类名、设定样式

```js
{
   path:'/music',
   children:{
       path:'/pop'  //此时该条路由 就是 /pop
   }
}
// 如果想使用 /music/pop 可以这样
{
   path:'/music',
   children:{
       path:'/music/pop'  //此时该条路由 就是 /music/pop
   }
}
// 或者
{
   path:'/music',
   children:{
       path:'pop'  //此时该条路由 就是 /music/pop
   }
}
```

## Vue中的动画过渡

- Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡
- 6种状态：
  - v-enter：定义进入过渡的开始状态。
  - v-enter-active：定义进入过渡生效时的状态。
  - v-enter-to: 2.1.8版及以上 定义进入过渡的结束状态。
  - v-leave: 定义离开过渡的开始状态。
  - v-leave-active：定义离开过渡生效时的状态。
  - v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。

```js
<transition name="fade"> 
<div v-show="isShow" class="box"></div>
</transition>
```

