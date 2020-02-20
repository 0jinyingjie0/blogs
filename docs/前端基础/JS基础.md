# JS基础

## JS组成

- ECMA+DOM+BOM

## 变量

- 计算机内存中存储数据的标识符

## 数据类型

### 基本数据类型

- number，string，boolean，undefined，null

### typrof

- 使用typrof获取数据类型

### 转换为字符串类型

- num.toString()
- string(num)
- 通过字符串拼接

### 转换为数字类型

- Number（）
- parseInt（）
- parseFloat（）
- 取负或者取正

### 转换为布尔类型

- boolean（a）

## 三元表达式

- 表达式1？值1：值2

## 作用域

### 全局作用域

- 在任何地方都可以获取变量

### 局部作用域

- 只能在函数内部获取变量

### 预解析

- 通过var变量或者function函数的方法，提升到当前作用域的最顶端

## 方法对象

### Math方法

- Math.round()：四舍五入取正
- Math.ceil()：哪一个浮点数向上取正
- Math.abs()：去绝对值

### 数组

- arr.push：数组中从后推入一个数，返回数组长度
- arr.pop：数组中删除一个数，返回被删除的元素
- arr.unshift：数组中前面加入一个元素，返回数组长度
- arr.shift：数组中从前删除一个元素，返回被删除的元素
- arr.splice(3,1)：从第一个参数开始删除，第二个参数是要被删除的个数
- arr.join('|')：以特定字符连接成一个字符串
- arr.split('*')：以特定字符吧字符串分割开
- arr.indexOf(a)：查找元素，返回索引
- concat：拼接数组，返回新数组
- slice：截取数组，返回新数组
  - 第一个参数下标开始
  - 第二个参数下标结束
  - 左包右不包
- substring：截取字符串，返回截取出来的字符串
  - 第一个参数下标开始
  - 第二个参数下标结束
  - 左包右不包
- substr：截取字符串，返回被截取的字符串
  - 第一个参数下标开始
  - 第二个参数表示截取的个数

### 数组遍历

- forEach：遍历数组，没有返回值

```html
arr.forEach(function(item,index,arr)){}
```

- filter：遍历数组，返回符合条件的新数组

```html
arr.filter(function(item,index,arr)){
	return item>200
}
```

