# VUE-面1

## 如何理解MVVM原理

### 特点

- 实现数据和视图的分离
- 通过数据来驱动视图，开发者只需关心数据变化，dom操作就被封装了

### 原理

- **响应式**：**vue如何监听data的属性变化**
  - data一般是放在一个对象中的
  - 当访问或修改obj的属性时，通过Object.defineProperty将data里的每一个属性的访问与修改都变成了一个函数，在函数get和set中我们即可监听到data的属性发生了改变。
- **模板解析：vue的模板是如何被解析的**
  - 模板在vue中必须转换为JS代码，原因在于：在前端环境下，只有JS才是一个图灵完备语言，才能实现逻辑运算，以及渲染为html页面。这里就引出了vue中一个特别重要的函数——**render**
  - render函数中的核心就是with函数。
  - with函数将某个对象添加到作用域链的顶部，如果在 statement中有某个未使用命名空间的变量，跟作用域链中的某个属性同名，则这个变量将指向这个属性值。
  - with将obj对象数据放在了自己函数的作用域链的顶部，当执行下列函数时，就会自动到obj这个对象去寻找同名的属性。
- **渲染：vue模板是如何被渲染成HTML的**
  - 模板渲染为html分为两种情况，第一种是初次渲染的时候，第二种是渲染之后数据发生改变的时候
  - 首先读取当前的虚拟DOM——vm._vnode,判断其是否为空，若为空，则为初次渲染，将虚拟DOM全部渲染到所对应的容器当中（vm.$el），若不为空，则是数据发生了修改，通过响应式我们可以监听到这一情况，使用diff算法完成新旧对比并修改。

## 响应式数据的原理是什么

- 详情请参考[vue响应式原理](<https://www.jianshu.com/p/4dff7c2cdaaa>)

### 基本概念

- Vue 的响应式，核心机制是 **观察者模式**
- Vue 通过在 data 和 watcher 间创建一个 dep 对象，来记录这种依赖关系：

```js
data - dep -> watcher
```

- dep 的结构很简单，除了唯一标识属性 id，另一个属性就是用于记录所有观察者的 subs：

- Vue 中  watcher 的观察对象，确切来说是一个求值表达式，或者函数。这个表达式或者函数，在一个 Vue 实例的上下文中求值或执行。这个过程中，使用到数据，也就是 watcher 所依赖的数据。用于记录依赖关系的属性是 deps，对应的是由 dep 对象组成的数组，对应所有依赖的数据。而表达式或函数，最终会作为求值函数记录到 getter 属性，每次求值得到的结果记录在 value 属性

- 还有一个重要的属性 cb，记录回调函数，当 getter 返回的值与当前 value 不同时被调用

```js
var vm = new Vue({
  data: {
    name: 'luobo',
    age: 18
  }
})

var userInfo = function () {
  return this.name + ' - ' + this.age
}

var onUserInfoChange = function (userInfo) {
  console.log(userInfo)
}

vm.$watch(userInfo, onUserInfoChange)
```

- 回调函数 onUserInfoChange 只是打印出新的 watcher 得到的新的值，由 userInfo 执行后生成

- 通过 `vm.$watch(userInfo, onUserInfoChange)`，将 vm、getter、cb 集成在一起创建了新的 watcher
- watcher.deps 中记录了 vm 的 name、age 对应的 dep 对象（因为 userInfo 中使用了这两个数据）。
- 修改 vm.name 后，dep1 通知相关的 watcher，然后 watcher 执行 getter，得到新的 value，再将新的 value 传给 cb：

```js
var vm = new Vue({
  data: {
    name: 'luobo',
    age: 18
  },
  computed: {
    userInfo() {
      return this.name + ' - ' + this.age
    }
  }
})
```

- 计算属性对应的 watcher 与直接通过 `vm.$watch()` 创建的 watcher 略有不同，毕竟如果没有地方使用到这个计算属性，数据改变时都重新进行计算会有点浪费

### 建立依赖关系

```js
// new Vue() -> ... -> initState() -> initData()
observe(data)
```

- 函数 `observe()` 的目的是让传入的整个对象成为响应式的，它会遍历对象的所有属性，然后执行：

```js
// observe() -> new Observer() -> observer.walk()
defineReactive(obj, key, value)
```

- `defineReactive()` 就是用于定义响应式数据的核心函数

- 新建一个 dep 对象，与当前数据对应
- 通过 `Object.defineProperty()` 重新定义对象属性，配置属性的 set、get，从而数据被获取、设置时可以执行 Vue 的代码

### 数据变更同步

- 数据发生改变会触发通过 `defineReactive()` 配置的 set 方法
- 通过 dep 对象来通知所有的依赖方法，于是 dep 遍历内部的 subs 执行：



- 通过 `Object.defineProperty()` 替换配置对象属性的 set、get 方法，实现“拦截”
- watcher 在执行 getter 函数时触发数据的 get 方法，从而建立依赖关系
- 写入数据时触发 set 方法，从而借助 dep 发布通知，进而 watcher 进行更新

- ![](https://upload-images.jianshu.io/upload_images/37341-26521b6eb96b6e21.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

## vue中是如何检测数组的变化的

- 我们知道通过Object.defineProperty()劫持数组为其设置getter和setter后，调用的数组的push、splice、pop等方法改变数组元素时并不会触发数组的setter，这就会造成使用上述方法改变数组后，页面上并不能及时体现这些变化，也就是数组数据变化不是响应式的
- 通过vue源码可以看出，vue重写了数组的方法

```js
// 获取数组的原型Array.prototype，上面有我们常用的数组方法
const arrayProto = Array.prototype
// 创建一个空对象arrayMethods，并将arrayMethods的原型指向Array.prototype
export const arrayMethods = Object.create(arrayProto)

// 列出需要重写的数组方法名
const methodsToPatch = [
  'push',
  'pop',
  'shift',
  'unshift',
  'splice',
  'sort',
  'reverse'
]
// 遍历上述数组方法名，依次将上述重写后的数组方法添加到arrayMethods对象上
methodsToPatch.forEach(function (method) {
  // 保存一份当前的方法名对应的数组原始方法
  const original = arrayProto[method]
  // 将重写后的方法定义到arrayMethods对象上，function mutator() {}就是重写后的方法
  def(arrayMethods, method, function mutator (...args) {
    // 调用数组原始方法，并传入参数args，并将执行结果赋给result
    const result = original.apply(this, args)
    // 当数组调用重写后的方法时，this指向该数组，当该数组为响应式时，就可以获取到其__ob__属性
    const ob = this.__ob__
    let inserted
    switch (method) {
      case 'push':
      case 'unshift':
        inserted = args
        break
      case 'splice':
        inserted = args.slice(2)
        break
    }
    if (inserted) ob.observeArray(inserted)
    // 将当前数组的变更通知给其订阅者
    ob.dep.notify()
    // 最后返回执行结果result
    return result
  })
})
```

- 从上面可以看出array.js中重写了数组的push、pop、shift、unshift、splice、sort、reverse七种方法，重写方法在实现时除了将数组方法名对应的原始方法调用一遍并将执行结果返回外，还通过执行ob.dep.notify()将当前数组的变更通知给其订阅者，这样当使用重写后方法改变数组后，数组订阅者会将这边变化更新到页面中。

- 在Observer构造函数中，可以看到在监听数据时会判断数据类型是否为数组。当为数组时，如果浏览器支持`__proto__`，则直接将当前数据的原型`__proto__`指向重写后的数组方法对象arrayMethods，如果浏览器不支持`__proto__`，则直接将arrayMethods上重写的方法直接定义到当前数据对象上；当数据类型为非数组时，继续递归执行数据的监听。
- 对于响应式数组，当浏览器支持__proto__属性时，使用push等方法时先从其原型arrayMethods上寻找push方法，也就是重写后的方法，处理之后数组的变化会通知到其订阅者，更新页面，当在arrayMethods上查询不到时会向上在Array.prototype上查询；当浏览器不支持__proto__属性时，使用push等方法时会先从数组自身上查询，如果查询不到会向上再Array.prototype上查询

## 为何vue采用异步渲染

- 使用vm.$nextTick方法异步获取DOM，
- 所有状态都修改好了之后再进行渲染就可以减少一些无用功。
- 组件内部使用VIrtualDOM进行渲染，也就是说，组件内部其实是不关心哪个状态发生了变化，它只需要计算一次就可以得知哪些节点需要更新。也就是说，如果更改了N个状态，其实只需要发送一个信号就可以将DOM更新到最新
- Vue的变化侦测机制决定了它必然会在每次状态发生变化时都会发出渲染的信号，但Vue会在收到信号之后检查队列中是否已经存在这个任务，保证队列中不会有重复。如果队列中不存在则将渲染操作添加到队列中。
- 之后通过异步的方式延迟执行队列中的所有渲染的操作并清空队列，当同一轮事件循环中反复修改状态时，并不会反复向队列中添加相同的渲染操作。

## nextTick实现原理

- nextTick（）：在下次DOM更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的DOM

- 先简单介绍下`MutationObserver`：MO是HTML5中的API，是一个用于监视DOM变动的接口，它可以监听一个DOM对象上发生的子节点删除、属性修改、文本内容修改等。
- 调用过程是要先给它绑定回调，得到MO实例，这个回调会在MO实例监听到变动时触发。这里MO的回调是放在`microtask`中执行的

```js
// 创建MO实例
const observer = new MutationObserver(callback)

const textNode = '想要监听的Don节点'

observer.observe(textNode, {
    characterData: true // 说明监听文本内容的修改
})
```

- `next-tick.js` 对外暴露了`nextTick`这一个参数，所以每次调用`Vue.nextTick`时会执行：
  - 把传入的回调函数`cb`压入`callbacks`数组
  - 执行`timerFunc`函数，延迟调用 `flushCallbacks` 函数
  - 遍历执行 `callbacks` 数组中的所有函数

- 这里的 `callbacks` 没有直接在 `nextTick` 中执行回调函数的原因是保证在同一个 `tick` 内多次执行`nextTick`，不会开启多个异步任务，而是把这些异步任务都压成一个同步任务，在下一个 `tick` 执行完毕。

### 语法

- Vue.nextTick([callback, context])
  - `{Function} [callback]`：回调函数，不传时提供promise调用
  - `{Object} [context]`：回调函数执行的上下文环境，不传默认是自动绑定到调用它的实例上。

### 小结

- 使用`Vue.nextTick()`是为了可以获取更新后的DOM 。
- 触发时机：在同一事件循环中的数据变化后，DOM完成更新，立即执行`Vue.nextTick()`的回调。

## vue组件的生命周期

- ![](https://img2018.cnblogs.com/blog/986768/201908/986768-20190829195354331-66343648.png)



## ajax请求放到哪个生命周期中

- 不考虑服务器端渲染，一般选在 `mounted` 周期内请求数据，因为这个周期开始时，当前组件已经被挂载到真实的元素上了

## 何时使用beforeDestroy

- 在实例销毁之前调用 

```js
 search(){
      let time = { 
        start: this.formSearch.beginSearchTime,
        end: this.formSearch.endSearchTime,
        timeRange: this.formSearch.timeRange,
        page: this.formSearch.page
      }
      localStorage.setItem('initTime',JSON.stringify(time))
    }
```

```js
created () {  
    let searchCarTime = JSON.parse(localStorage.getItem('initTime'))
    if (searchCarTime) {
      this.formSearch.beginSearchTime = searchCarTime.start
      this.formSearch.endSearchTime = searchCarTime.end,
      this.formSearch.timeRange = searchCarTime.timeRange 
      this.formSearch.page = searchCarTime.page 
    }
  }
```

```js
 beforeDestroy(){
    localStorage.removeItem('initTime')
  }
```

## vue父子组件生命周期调用顺序

- 加载渲染过程

```text
　父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted
```

- 子组件更新过程

```js
父beforeUpdate->子beforeUpdate->子updated->父updated
```

- 父组件更新过程

```js
　父beforeUpdate->父updated
```

- 销毁过程

```js
　父beforeDestroy->子beforeDestroy->子destroyed->父destroyed
```

## vue中computed的特点

- 计算属性是基于它们的响应式依赖进行缓存的。

