# ECharts

## 基础概念

### 实例

- 一个网页中可以创建多个 `echarts 实例`。每个 `echarts 实例` 中可以创建多个图表和坐标系等等（用 `option` 来描述）。准备一个 DOM 节点（作为 echarts 的渲染容器），就可以在上面创建一个 echarts 实例。每个 echarts 实例独占一个 DOM 节点。

### 系列

- `系列`（[series](https://www.echartsjs.com/zh/option.html#series)）是指：一组数值以及他们映射成的图。而在 echarts 中取其扩展的概念，不仅表示数据，也表示数据映射成为的图。

- echarts 里系列类型（`series.type`）就是图表类型。系列类型（`series.type`）至少有：[line](https://www.echartsjs.com/zh/option.html#series-line)（折线图）、[bar](https://www.echartsjs.com/zh/option.html#series-bar)（柱状图）、[pie](https://www.echartsjs.com/zh/option.html#series-pie)（饼图）、[scatter](https://www.echartsjs.com/zh/option.html#series-scatter)（散点图）、[graph](https://www.echartsjs.com/zh/option.html#series-graph)（关系图）、[tree](https://www.echartsjs.com/zh/option.html#series-tree)（树图）、...

### 组件

- 在系列之上，echarts 中各种内容，被抽象为“组件”。例如，echarts 中至少有这些组件：[xAxis](https://www.echartsjs.com/zh/option.html#xAxis)（直角坐标系 X 轴）、[yAxis](https://www.echartsjs.com/zh/option.html#yAxis)（直角坐标系 Y 轴）、[grid](https://www.echartsjs.com/zh/option.html#grid)（直角坐标系底板）、[tooltip](https://www.echartsjs.com/zh/option.html#tooltip)（提示框组件）、[toolbox](https://www.echartsjs.com/zh/option.html#toolbox)（工具栏组件）、[series](https://www.echartsjs.com/zh/option.html#series)（系列）、...

### 用 option 描述图表

- `option` 表述了：`数据`、`数据如何映射成图形`、`交互行为`。

- 示例：

```js
// 创建 echarts 实例。
var dom = document.getElementById('dom-id');
var chart = echarts.init(dom);

// 用 option 描述 `数据`、`数据如何映射成图形`、`交互行为` 等。
// option 是个大的 JavaScript 对象。
var option = {
    // option 每个属性是一类组件。
    legend: {...},
    grid: {...},
    tooltip: {...},
    toolbox: {...},
    dataZoom: {...},
    visualMap: {...},
    // 如果有多个同类组件，那么就是个数组。例如这里有三个 X 轴。
    xAxis: [
        // 数组每项表示一个组件实例，用 type 描述“子类型”。
        {type: 'category', ...},
        {type: 'category', ...},
        {type: 'value', ...}
    ],
    yAxis: [{...}, {...}],
    // 这里有多个系列，也是构成一个数组。
    series: [
        // 每个系列，也有 type 描述“子类型”，即“图表类型”。
        {type: 'line', data: [['AA', 332], ['CC', 124], ['FF', 412], ... ]},
        {type: 'line', data: [2231, 1234, 552, ... ]},
        {type: 'line', data: [[4, 51], [8, 12], ... ]}
    }]
};

// 调用 setOption 将 option 输入 echarts，然后 echarts 渲染图表。
chart.setOption(option);
```

### 组件的定位

- 多数组件和系列，都能够基于 `top` / `right` / `down` / `left` / `width` / `height` 绝对定位。 这种绝对定位的方式，类似于 `CSS` 的绝对定位（`position: absolute`）。绝对定位基于的是 echarts 容器 DOM 节点。

## 简单上手

### 获取

- 从 [Apache ECharts (incubating) 官网下载界面](https://www.echartsjs.com/download.html) 获取官方源码包后构建。
- 在 ECharts 的 [GitHub](https://github.com/apache/incubator-echarts/releases) 获取。
- 通过 npm 获取 echarts，`npm install echarts --save`，详见“[在 webpack 中使用 echarts](https://www.echartsjs.com/tutorial.html#%E5%9C%A8%20webpack%20%E4%B8%AD%E4%BD%BF%E7%94%A8%20ECharts)”

### 引入

- 新建标签然后引入

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- 引入 ECharts 文件 -->
    <script src="echarts.min.js"></script>
</head>
</html>
```

### 简单绘图

- 先为图标准备一个容器，可以写上样式

```html
<body>
    <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

- 然后就可以通过 [echarts.init](https://www.echartsjs.com/zh/api.html#echarts.init) 方法初始化一个 echarts 实例并通过 [setOption](https://www.echartsjs.com/zh/api.html#echartsInstance.setOption) 方法生成一个简单的柱状图

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ECharts</title>
    <!-- 引入 echarts.js -->
    <script src="echarts.min.js"></script>
</head>
<body>
    <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
        // 基于准备好的dom，初始化echarts实例
        var myChart = echarts.init(document.getElementById('main'));

        // 指定图表的配置项和数据
        var option = {
            //标题展示
            title: {
                text: 'ECharts 入门示例'
            },
            tooltip: {},
            //顶部解释
            legend: {
                data:['销量']
            },
            //X轴坐标，顺序
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            //Y轴坐标，默认
            yAxis: {},
            //当滑动上时显示的数据
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        };

        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);
    </script>
</body>
</html>
```

## webpack 中使用 ECharts

### 安装 ECharts

```html
npm install echarts --save
```

### 引入 ECharts

- 通过 npm 上安装的 ECharts 和 zrender 会放在`node_modules`目录下。可以直接在项目代码中 `require('echarts')` 得到 ECharts。

```js
var echarts = require('echarts');

// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 绘制图表
myChart.setOption({
    title: {
        text: 'ECharts 入门示例'
    },
    tooltip: {},
    xAxis: {
        data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
});
```

### 按需引入 ECharts 图表和组件

- 默认使用 `require('echarts')` 得到的是已经加载了所有图表和组件的 ECharts 包，因此体积会比较大，如果在项目中对体积要求比较苛刻，也可以只按需引入需要的模块。

- 优化示例

```js
// 引入 ECharts 主模块
var echarts = require('echarts/lib/echarts');
// 引入柱状图
require('echarts/lib/chart/bar');
// 引入提示框和标题组件
require('echarts/lib/component/tooltip');
require('echarts/lib/component/title');

// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 绘制图表
myChart.setOption({
    title: {
        text: 'ECharts 入门示例'
    },
    tooltip: {},
    xAxis: {
        data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
});
```

## 待续

- 详细教程参考官网   [ECharts](<https://www.echartsjs.com/zh/tutorial.html#5%20%E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B%20ECharts>)

