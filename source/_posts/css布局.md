---
title: 一、css布局
date: 2024-07-08 22:32:50
tags: css
---

### css布局

​		在CSS中，常用的布局方法有多种，每种方法都有其独特的优点和适用场景。下面是一些常见的布局方法及其主要属性：

#### 1. Display属性

`display` 属性决定了一个元素是如何显示在文档中的。常见的值包括：

- `display: block;`：块级元素，`block-item`占据父容器的整个宽度，所以元素会自动换行，同时也可以设置`block-item`的高度。
- `display: inline;`：行内元素，`inline-item`只占据内容所需的宽度，宽度自适应，无法修改`inline-item`的宽度和高度。
- `display: inline-block;`：行内块元素，`inline-block-item`像行内元素那样排列，但可以设置宽度和高度。
- `display: none;`：隐藏元素，不占据任何空间。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Display 属性示例</title>
    <style>
        .container {
            margin-bottom: 20px;
        }
        .block-item {
            display: block;
            background-color: lightcoral;
            margin: 5px 0;
            padding: 10px;
            color: white;
        }
        .inline-item {
            display: inline;
            background-color: lightseagreen;
            margin: 5px;
            padding: 10px;
            color: white;
        }
        .inline-block-item {
            display: inline-block;
            background-color: lightblue;
            margin: 5px;
            padding: 10px;
            width: 100px;
            color: white;
        }
        .none-item {
            display: none;
        }
    </style>
</head>
<body>

    <!-- Display Block -->
    <div class="container">
        <div class="block-item">Block Item 1</div>
        <div class="block-item">Block Item 2</div>
    </div>

    <!-- Display Inline -->
    <div class="container">
        <div class="inline-item">Inline Item 1</div>
        <div class="inline-item">Inline Item 2</div>
        <div class="inline-item">Inline Item 3</div>
    </div>

    <!-- Display Inline-Block -->
    <div class="container">
        <div class="inline-block-item">Inline-Block Item 1</div>
        <div class="inline-block-item">Inline-Block Item 2</div>
        <div class="inline-block-item">Inline-Block Item 3</div>
    </div>

    <!-- Display None (Hidden) -->
    <div class="container">
        <div class="none-item">This item is hidden</div>
        <div>This item is visible</div>
    </div>

</body>
</html>
```

#### 2. Float属性

`float` 属性用于让元素在其容器中左右浮动。

**Float Left**:

- `float-left` 元素使用 `float: left;` 属性，元素左浮动，后面的内容环绕在右侧。
- `clear-left` 元素使用 `clear: left;` 属性，清除左侧浮动，这样 `clear-left` 元素就不会受到前面浮动元素的影响。

**Float Right**:

- `float-right` 元素使用 `float: right;` 属性，元素右浮动，后面的内容环绕在左侧。
- `clear-right` 元素使用 `clear: right;` 属性，清除右侧浮动，这样 `clear-right` 元素就不会受到前面浮动元素的影响。

**Clear Both**:

- `float-left` 和 `float-right` 元素分别使用 `float: left;` 和 `float: right;` 属性，使元素分别左浮动和右浮动。
- `clear-both` 元素使用 `clear: both;` 属性，清除左右两侧浮动，确保它不会受到前面所有浮动元素的影响。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Float 属性示例</title>
    <style>
        .container {
            margin-bottom: 20px;
            background-color: #f0f0f0;
            padding: 10px;
        }
        .float-left {
            float: left;
            background-color: lightcoral;
            margin: 5px;
            padding: 10px;
            width: 100px;
            color: white;
        }
        .float-right {
            float: right;
            background-color: lightseagreen;
            margin: 5px;
            padding: 10px;
            width: 100px;
            color: white;
        }
        .clear-left {
            clear: left;
            background-color: lightblue;
            margin: 5px;
            padding: 10px;
            color: white;
        }
        .clear-right {
            clear: right;
            background-color: lightpink;
            margin: 5px;
            padding: 10px;
            color: white;
        }
        .clear-both {
            clear: both;
            background-color: lightgoldenrodyellow;
            margin: 5px;
            padding: 10px;
            color: black;
        }
    </style>
</head>
<body>

    <!-- Float Left -->
    <div class="container">
        <div class="float-left">Float Left 1</div>
        <div class="float-left">Float Left 2</div>
        <div class="clear-left">Clear Left</div>
    </div>

    <!-- Float Right -->
    <div class="container">
        <div class="float-right">Float Right 1</div>
        <div class="float-right">Float Right 2</div>
        <div class="clear-right">Clear Right</div>
    </div>

    <!-- Clear Both -->
    <div class="container">
        <div class="float-left">Float Left 3</div>
        <div class="float-right">Float Right 3</div>
        <div class="clear-both">Clear Both</div>
    </div>

</body>
</html>
```

#### 3. Flexbox布局

`flex` 布局是一种一维布局模型，可以非常方便地在容器中分配空间并对齐内容。主轴（项目的排列方向），交叉轴（垂直主轴），常见的属性包括：

- `display: flex;`：将容器设置为弹性布局容器。
- `flex-direction`：定义主轴方向（项目的排列方向），常见值有 `row`、`row-reverse`、`column`、`column-reverse`。
- `justify-content`：定义主轴上的对齐方式，常见值有 `flex-start`、`flex-end`、`center`、`space-between`、`space-around`。
- `align-items`：定义交叉轴上的对齐方式，常见值有 `flex-start`、`flex-end`、`center`、`baseline`、`stretch`。
- `flex-wrap`：定义项目是否换行，常见值有 `nowrap`、`wrap`、`wrap-reverse`。
- `flex-grow`：定义项目的扩展比例。
- `flex-shrink`：定义项目的收缩比例。
- `flex-basis`：定义项目的初始大小。

**Flex Direction**:

- `flex-direction: row;`：主轴方向为横向，从左到右排列。
- `flex-direction: column;`：主轴方向为纵向，从上到下排列。

**Justify Content**:

- `justify-content: space-between;`：项目在主轴上均匀分布，第一个项目与最后一个项目贴边对齐。
- `justify-content: center;`：项目在主轴上居中对齐。

**Align Items**:

- `align-items: flex-start;`：项目在交叉轴上起点对齐。
- `align-items: center;`：项目在交叉轴上居中对齐。

**Flex Wrap**:

- `flex-wrap: wrap;`：项目在容器中换行，超出父容器宽度时会自动换行排列。

**Flex Grow**:

- `flex: 1;`、`flex: 2;` 和 `flex: 3;`：分别定义项目的扩展比例。`flex: 2` 的项目扩展比例是 `flex: 1` 的两倍，以此类推。

**Flex Shrink**:

- `flex: 1 1 200px;`：定义项目的收缩比例为1，初始大小为200px。当父容器宽度不足时，项目会按比例收缩。

**Flex Basis**:

- `flex-basis: 100px;`、`200px;` 和 `300px;`：分别定义项目的初始大小为100px、200px和300px。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flexbox 布局示例</title>
    <style>
        .container {
            border: 1px solid #ddd;
            margin-bottom: 20px;
            padding: 10px;
        }
        .flex-container {
            display: flex;
            border: 1px solid #ccc;
            padding: 10px;
            margin-bottom: 20px;
        }
        .flex-item {
            background-color: lightcoral;
            margin: 5px;
            padding: 10px;
            color: white;
        }
        .flex-item-1 { flex: 1; }
        .flex-item-2 { flex: 2; }
        .flex-item-3 { flex: 3; }
    </style>
</head>
<body>

    <!-- Flex Direction -->
    <div class="container">
        <h3>Flex Direction (row)</h3>
        <div class="flex-container" style="flex-direction: row;">
            <div class="flex-item">Item 1</div>
            <div class="flex-item">Item 2</div>
            <div class="flex-item">Item 3</div>
        </div>
        <h3>Flex Direction (column)</h3>
        <div class="flex-container" style="flex-direction: column;">
            <div class="flex-item">Item 1</div>
            <div class="flex-item">Item 2</div>
            <div class="flex-item">Item 3</div>
        </div>
    </div>

    <!-- Justify Content -->
    <div class="container">
        <h3>Justify Content (space-between)</h3>
        <div class="flex-container" style="justify-content: space-between;">
            <div class="flex-item">Item 1</div>
            <div class="flex-item">Item 2</div>
            <div class="flex-item">Item 3</div>
        </div>
        <h3>Justify Content (center)</h3>
        <div class="flex-container" style="justify-content: center;">
            <div class="flex-item">Item 1</div>
            <div class="flex-item">Item 2</div>
            <div class="flex-item">Item 3</div>
        </div>
    </div>

    <!-- Align Items -->
    <div class="container">
        <h3>Align Items (flex-start)</h3>
        <div class="flex-container" style="align-items: flex-start; height: 200px;">
            <div class="flex-item">Item 1</div>
            <div class="flex-item">Item 2</div>
            <div class="flex-item">Item 3</div>
        </div>
        <h3>Align Items (center)</h3>
        <div class="flex-container" style="align-items: center; height: 200px;">
            <div class="flex-item">Item 1</div>
            <div class="flex-item">Item 2</div>
            <div class="flex-item">Item 3</div>
        </div>
    </div>

    <!-- Flex Wrap -->
    <div class="container">
        <h3>Flex Wrap (wrap)</h3>
        <div class="flex-container" style="flex-wrap: wrap;">
            <div class="flex-item" style="width: 30%;">Item 1</div>
            <div class="flex-item" style="width: 30%;">Item 2</div>
            <div class="flex-item" style="width: 30%;">Item 3</div>
            <div class="flex-item" style="width: 30%;">Item 4</div>
            <div class="flex-item" style="width: 30%;">Item 5</div>
            <div class="flex-item" style="width: 30%;">Item 6</div>
        </div>
    </div>

    <!-- Flex Grow -->
    <div class="container">
        <h3>Flex Grow</h3>
        <div class="flex-container">
            <div class="flex-item flex-item-1">Item 1 (flex: 1)</div>
            <div class="flex-item flex-item-2">Item 2 (flex: 2)</div>
            <div class="flex-item flex-item-3">Item 3 (flex: 3)</div>
        </div>
    </div>

    <!-- Flex Shrink -->
    <div class="container">
        <h3>Flex Shrink</h3>
        <div class="flex-container" style="width: 300px;">
            <div class="flex-item" style="flex: 1 1 200px;">Item 1 (flex: 1 1 200px)</div>
            <div class="flex-item" style="flex: 1 1 200px;">Item 2 (flex: 1 1 200px)</div>
            <div class="flex-item" style="flex: 1 1 200px;">Item 3 (flex: 1 1 200px)</div>
        </div>
    </div>

    <!-- Flex Basis -->
    <div class="container">
        <h3>Flex Basis</h3>
        <div class="flex-container">
            <div class="flex-item" style="flex-basis: 100px;">Item 1 (flex-basis: 100px)</div>
            <div class="flex-item" style="flex-basis: 200px;">Item 2 (flex-basis: 200px)</div>
            <div class="flex-item" style="flex-basis: 300px;">Item 3 (flex-basis: 300px)</div>
        </div>
    </div>

</body>
</html>
```

#### 4. Grid布局

`grid` 布局是一种二维布局模型，可以在行和列中同时定义布局。常见的属性包括：

- `display: grid;`：将容器设置为网格布局容器。
- `grid-template-columns`：定义列的布局，例如 `grid-template-columns: 1fr 1fr;`。
- `grid-template-rows`：定义行的布局，例如 `grid-template-rows: 100px 200px;`。
- `gap`：定义网格间的间距。
- `justify-items`：定义网格项目在行方向上的对齐方式。
- `align-items`：定义网格项目在列方向上的对齐方式。

**Grid Template Columns**:

- `grid-template-columns: 1fr 1fr 1fr;`：将网格容器分为三列，每列占据相同的空间。

**Grid Template Rows**:

- `grid-template-rows: 100px 200px;`：将网格容器分为两行，第一行高度为 100px，第二行高度为 200px。

**Gap**:

- `gap: 10px;`：设置网格项目之间的间距为 10px。

**Justify Items**:

- `justify-items: center;`：设置网格项目在行方向上居中对齐。

**Align Items**:

- `align-items: center;`：设置网格项目在列方向上居中对齐。网格容器高度为 300px，网格项目高度为 50px，因此项目在容器中垂直居中对齐。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grid 布局示例</title>
    <style>
        .container {
            display: grid;
            border: 1px solid #ddd;
            padding: 10px;
            margin-bottom: 20px;
        }
        .grid-container {
            display: grid;
            border: 1px solid #ccc;
            padding: 10px;
            gap: 10px; /* 定义网格间的间距 */
        }
        .grid-item {
            background-color: lightcoral;
            padding: 10px;
            color: white;
            text-align: center;
        }
    </style>
</head>
<body>

    <!-- Grid Template Columns -->
    <div class="container">
        <h3>Grid Template Columns (1fr 1fr 1fr)</h3>
        <div class="grid-container" style="grid-template-columns: 1fr 1fr 1fr;">
            <div class="grid-item">Item 1</div>
            <div class="grid-item">Item 2</div>
            <div class="grid-item">Item 3</div>
            <div class="grid-item">Item 4</div>
            <div class="grid-item">Item 5</div>
            <div class="grid-item">Item 6</div>
        </div>
    </div>

    <!-- Grid Template Rows -->
    <div class="container">
        <h3>Grid Template Rows (100px 200px)</h3>
        <div class="grid-container" style="grid-template-rows: 100px 200px;">
            <div class="grid-item">Item 1</div>
            <div class="grid-item">Item 2</div>
            <div class="grid-item">Item 3</div>
            <div class="grid-item">Item 4</div>
        </div>
    </div>

    <!-- Justify Items -->
    <div class="container">
        <h3>Justify Items (center)</h3>
        <div class="grid-container" style="grid-template-columns: 1fr 1fr 1fr; justify-items: center;">
            <div class="grid-item">Item 1</div>
            <div class="grid-item">Item 2</div>
            <div class="grid-item">Item 3</div>
            <div class="grid-item">Item 4</div>
            <div class="grid-item">Item 5</div>
            <div class="grid-item">Item 6</div>
        </div>
    </div>

    <!-- Align Items -->
    <div class="container">
        <h3>Align Items (center)</h3>
        <div class="grid-container" style="grid-template-columns: 1fr 1fr 1fr; align-items: center; height: 300px;">
            <div class="grid-item" style="height: 50px;">Item 1</div>
            <div class="grid-item" style="height: 50px;">Item 2</div>
            <div class="grid-item" style="height: 50px;">Item 3</div>
            <div class="grid-item" style="height: 50px;">Item 4</div>
            <div class="grid-item" style="height: 50px;">Item 5</div>
            <div class="grid-item" style="height: 50px;">Item 6</div>
        </div>
    </div>

</body>
</html>
```

### 5. Position属性

`position` 属性用于指定元素的定位方式，常见的值包括：

- `position: static;`：默认值，无特殊定位。
- `position: relative;`：相对定位，相对于其正常位置进行偏移。
- `position: absolute;`：绝对定位，相对于最近的定位祖先进行偏移。
- `position: fixed;`：固定定位，相对于浏览器窗口进行偏移。
- `position: sticky;`：粘性定位，在特定的滚动位置时定位。

**Static Position**:

- `position: static;`：这是默认值，元素按正常的文档流进行定位，没有任何偏移。

**Relative Position**:

- `position: relative;`：相对定位，元素相对于其正常位置进行偏移。示例中，元素向下偏移了 20px，向右偏移了 20px。

**Absolute Position**:

- `position: absolute;`：绝对定位，元素相对于最近的定位祖先进行偏移。示例中，元素相对于 `.container` 容器向下偏移了 20px，向右偏移了 20px。

**Fixed Position**:

- `position: fixed;`：固定定位，元素相对于浏览器窗口进行偏移。示例中，元素固定在窗口的右上角，向下偏移了 20px，向右偏移了 20px。

**Sticky Position**:

- `position: sticky;`：粘性定位，元素在特定的滚动位置时定位。示例中，元素在滚动到顶部时会固定在容器顶部。这里使用了 `-webkit-sticky` 以兼容旧版浏览器。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Position 属性示例</title>
    <style>
        .container {
            border: 1px solid #ddd;
            padding: 10px;
            margin-bottom: 20px;
            height: 200px;
            position: relative; /* 为了演示绝对定位 */
        }
        .box {
            background-color: lightcoral;
            padding: 10px;
            color: white;
            text-align: center;
            margin-bottom: 10px;
        }
        .static-box {
            position: static; /* 默认定位 */
        }
        .relative-box {
            position: relative; /* 相对定位 */
            top: 20px;
            left: 20px;
        }
        .absolute-box {
            position: absolute; /* 绝对定位 */
            top: 20px;
            left: 20px;
        }
        .fixed-box {
            position: fixed; /* 固定定位 */
            top: 20px;
            right: 20px;
        }
        .sticky-box {
            position: -webkit-sticky; /* 兼容旧版浏览器 */
            position: sticky; /* 粘性定位 */
            top: 0;
            background-color: lightblue;
        }
    </style>
</head>
<body>

    <!-- Static Position -->
    <div class="container">
        <h3>Static Position</h3>
        <div class="box static-box">Static Box</div>
    </div>

    <!-- Relative Position -->
    <div class="container">
        <h3>Relative Position</h3>
        <div class="box relative-box">Relative Box</div>
    </div>

    <!-- Absolute Position -->
    <div class="container">
        <h3>Absolute Position</h3>
        <div class="box absolute-box">Absolute Box</div>
    </div>

    <!-- Fixed Position -->
    <div class="container">
        <h3>Fixed Position</h3>
        <div class="box fixed-box">Fixed Box</div>
    </div>

    <!-- Sticky Position -->
    <div class="container" style="height: 500px; overflow-y: scroll;">
        <h3>Sticky Position</h3>
        <div class="box sticky-box">Sticky Box</div>
            <p>1MySQL 事务，存储过程，索引，常用的语法，锁机制</p>
        	<p>2Redis用法，redisTemplate常见的用法，封装</p>
        	<p>3RocketMq或者rabbitMq用法</p>
        	<p>4mybatis用法，相关知识</p>
        	<p>5Java常见的集合，用法，Stream流，Optional类的使用</p>
        	<p>6在虚拟机上练习Linux命令，或者直接尝试用来部署项目</p>
        	<p>7vuex的用法，vue2基础框架结构，vue3的用法特性。</p>
        	<p>8Java多线程 锁机制 线程安全</p>
        	<p>9Linux基础命令练习</p>
        	<p>10常见的css布局</p> 
    </div>
</body>
</html>
```

