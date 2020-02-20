# HTML和CSS基础

## Web标准

- 结构标准：对网页元素进行整理和
- 样式标准：设置网页元素的样式
- 行为标准：网页的交互行为

## 为什么标签语义化

- 代码结构清晰，方便阅读，有益于团队开发
- 让浏览器或者网页爬虫可以很好的解析，从而可以更好的分析内容
- 有助于搜索引擎的优化
- 方便其他设备解析

## 文字阴影

- text-shadow:水平位置  垂直位置  模糊位置  阴影位置  阴影颜色

## 表单

- 输入框

```js
<input type="text"/>
```

- 密码输入框

```js
<input type="password" />
```

- 下拉列表框

```html
<select>
    <optyoup label="中国">
        <option>北京</option>
	</optyoup>
</select>
```

- 复选框

```js
<input type="checkbox" />
```

- 单选控件

```js
<input type="radio" />
```

- 上传组件

```js
<input type="file" />
```

- 文本域

```js
<textarea></textarea>
```

- 表单提交

```js
<input type="submit" />
```

- 普通按钮

```html
<input type="button" />
```

## css三大特性

- 层叠性
- 继承性
- 特殊性（权重）

## 清除浮动的方法

- 在浮动元素末尾添加一个空的标签

```js
<div style="clear:both"></div>
```

- 父级元素overflow
- 使用单伪元素
- 使用双伪元素

- 给父元素加高度

## overflow属性

- visible：不剪切内容也不添加滚动条
- auto：超出自动显示滚动条
- hidden：不显示传出内容，超出内容隐藏
- scroll：一直存在滚动条

## 鼠标样式

- cursor：default	小白
  - cursor：pointer	小手
    - cursor：move	移动
      - cursor：text	文本

## 2D转化

### 位移

- transform:translate(值1，值2)

### 旋转

- transform：rotate(角度deg)

### 缩放

- transform：scale(宽度，高度)

### 倾斜

- transform：skew(角度deg)

## 动画

- animation：动画名称  持续时间  速度曲线  等待时间  执行次数  执行方向  动画状态

## 弹性布局

- flex-direction:row	设定主轴方向为row(默认)（column）
- flex-wrap:wrap 	换行
- justify-content：center
- align-content:center
- aligin-items:flex-start

## 设置渐进色

background：linear-gradient(90deg,#ccc,#fff )

## 响应式布局

媒体查询+栅格