# TypeScript

## 简单上手

- 全局安装

```ch
 npm install -g typescript
```

- 简单构建项目

```ts
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

- 我们使用了`.ts`扩展名，但是这段代码仅仅是JavaScript而已。 你可以直接从现有的JavaScript应用里复制/粘贴这段代码。

- 在命令行上，运行TypeScript编译器：

```js
tsc greeter.ts
```

- 接下来让我们看看TypeScript工具带来的高级功能。 给 `person`函数的参数添加`: string`类型注解

```js
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

## 接口

- 这里我们使用接口来描述一个拥有`firstName`和`lastName`字段的对象。 在TypeScript里，只在两个类型内部的结构兼容那么这两个类型就是兼容的。 这就允许我们在实现接口时候只要保证包含了接口要求的结构就可以，而不必明确地使用 `implements`语句。

```js
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```

## 类

- TypeScript支持JavaScript的新特性，比如支持基于类的面向对象编程。

- 在构造函数的参数上使用`public`等同于创建了同名的成员变量。
-  TypeScript里的类只是JavaScript里常用的基于原型面向对象编程的简写。

```js
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```

## 待续。。

