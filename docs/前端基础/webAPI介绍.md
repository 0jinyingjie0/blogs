# web API

## 获取dom元素

### 获取元素

- document.getElementById('id名')
- document.getElementByTagName('标签名')
- document.getElementByClassName('类名')
- document.querySelector('#id')
- document.querySelectorAll('div')

### 事件三要素

- 事件源
- 事件类型
- 事件处理函数

## 自定义属性

- getAttribute()：获取属性
- setAttribute()：设置属性
- removeAttribute()：移除属性

## 样式操作

### style样式

- 元素.style.样式=‘样式值’

### 通过classname设置样式

- 元素.className='类名'

### classList

- 元素.classList.add('c2')：添加样式不覆盖
- 元素.classList.remove('c2')：删除样式
- 元素.classList.toggle('c2')：切换样式

## 节点关系

- 父节点：parentNode
- 子节点：children
- 第一个子节点：firstElementChild
- 最后一个子节点：lastElementChild
- 下一个兄弟节点：nextElementChild
- 上一个兄弟节点：previousElementChild
- 节点属性：
  - 节点类型：nodeType，元素1，文本3
  - 节点名称：nodeName，元素：大写的标签名，文本#text
  - 节点值：nodeValue，元素：null，文本：文本内容

## 动态创建元素

- innerHTML

```html
ul.innerHTML=str+'<li>新的</li>'
```

- 追加元素

```html
document.createElement('li')
ul.appendChild(newli)
newli.innerHTML='新的'
```

## 元素方法

- 追加元素：父元素.appendChild(子元素)
- 移除元素：父元素.removeChild(子元素)
- 插入元素：父元素.insertChild(新，旧)
- 替换元素：父元素.replaceChild(新，旧)
- 克隆元素：元素.cloneNode(true)

## 事件监听

- 事件源.addEventListener(‘事件类型’，事件处理函数，是否捕获)
- 移除事件：事件源.removeEventListener(‘事件类型’，事件处理函数)

## 鼠标位置信息

### 获取事件对象

- 事件源.事件类型=function(e)

### 对象相关属性

- 事件对象.client X/Y，参照是浏览器
- 事件对象.page X/Y，参照是文档
- 事件对象.offset X/Y，参照是当前元素

## 事件对象

- 最先触发的元素
  - e.target，代表最先被触发的元素

- 阻止默认行为
  - e.preventDefault

- 阻止冒泡传播
  - e.stopPropagetion

## 事件委托

- 把子孙的事件注册完全交给子孙元素的上级元素代理
- 步骤：
  - 给子孙元素的上级注册事件
  - 在事件处理程序中，通过 事件对象.target 获取最先触发的元素
  - 通过 事件对象.target的nodeName 属性检测最先触发的指定元素

## 定时器

- 一次性定时器
  - window.setTimeout(callback,time)

- 反复性定时器
  - window.setInterval(callback,time)

## touch事件

### 事件

- 执行效率高，用户体验好，事件监听

- touchstart：手指按下
- touchmove：手指移动
- touchend：手指松开

### touch事件对象

- 事件对象.targetTouches：位于屏幕上的所有手指的列表
- 事件对象.changedTouches：被改变的手指列表

## 本地存储技术

- localStorage.setItem(键，值)
- localStorage.getItem(键)

## JSON转换

- 字符串转数组：JSON.parse()
- 数组转字符串：JSON.Stringify()