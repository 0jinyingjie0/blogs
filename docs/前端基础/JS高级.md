# JS高级

## 三大特点

- 封装性
- 继承性
- 多态性

## 类

### 语法

- class 类名{}

### 类的继承

- extends
  - class Father{}
  - class Son extends Father{}
- super关键字
  - 用于访问和调用对象父类上的函数
  - super(name,age)
  - super.user()

## 构造函数

```js
class star {
    constructor(name,age){
        this.name="11"
        this.age=22
    }
}
```

### new的过程

- 在内存中创建一个新对象
- 让this指向这个新对象
- 执行构造函数的代码
- 返回这个新的对象

### 成员

- 静态成员：在构造函数本上添加的成员称为静态成员，只能由构造函数本身来访问
- 实例成员：在构造函数内部创建的对象成员称为实例成员，只能由实例化的对象来访问

### constructor

- 记录哪个构造函数创建出来的
- 原型和原型对象的属性

### 组合继承

- 通过构造函数和原型对象实现继承
- 属性继承

```js
function Father (name) {
    this.name=name
}
function Son (name,age) {
    Father.call(this.name)
    this.age=age
}
```

- 方法的继承
  - 将子类的prototype原型对象=new 父类（）
  - 本质：子类原型对象等于实例化父类
  - 实现继承后，让son指回原构造函数

```js
Son.prototype=new Father()
var obj=new Son()
obj.change()
Son.prototype.constructor=Son
```

### this指向问题

- 构造函数的this指向实例对象

- 普通函数的this是调用者，谁调用this是谁

- 对象方法调用指向该方法所属对象

- 事件绑定方法指向绑定事件对象

- 定时器函数指向window

- 立即执行函数指向window

## 数组的方法

- forEach()：遍历数组没有返回值
- filter()：遍历数组，返回一个新数组，筛选数组
- some()：遍历数组返回一个布尔值，找到一个元素就停止

## 闭包

- 一个函数访问另一个函数内部的局部变量
- 起到延伸变量的作用范围

## 拷贝

- 浅拷贝：只拷贝最外面一层
- 深拷贝：自己写方法

```js
	function kaobei (newObj,obj) {
			for (key in obj) {
				if (obj[key] instanceof Array) {
					newObj[key] = [];
					kaobei(newObj[key],obj[key]);
				} else if (obj[key] instanceof Object) {
					newObj[key] = {};
					kaobei(newObj[key],obj[key])
				} else {
					newObj[key] = obj[key];
				}
			}
		}
```

