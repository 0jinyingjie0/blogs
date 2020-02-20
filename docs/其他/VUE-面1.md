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

