---
title: 二、css伪类选择器
date: 2024-07-08 22:50:50
tags: css
---
### css 伪类选择器

css 伪类选择器是一种用于选择元素的特定状态的方式，常见的伪类选择器包括 `:hover`、`:active`、`:nth-child` 等。以下是一些常用的 css 伪类选择器及其使用示例。

#### 常见的 css 伪类选择器

1. `:hover`：用于选择用户鼠标悬停在其上的元素。
2. `:active`：用于选择用户激活（比如点击）元素。
3. `:nth-child(n)`：用于选择属于其父元素的第 n 个子元素，n 可以是关键字（如 odd、even）或公式。
4. `:first-child`：用于选择属于其父元素的第一个子元素。
5. `:last-child`：用于选择属于其父元素的最后一个子元素。
6. `:nth-of-type(n)`：用于选择属于其父元素特定类型的第 n 个子元素。
7. `:not(selector)`：用于选择非某个选择器的元素。

#### 示例代码

以下是一个使用不同伪类选择器的综合示例：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>css 伪类选择器示例</title>
    <style>
        .container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            width: 320px;
            margin: 50px auto;
        }
        .box {
            width: 100px;
            height: 100px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: white;
        }
        .box:nth-child(odd) {
            background-color: #FF6347; /* 番茄色 */
        }
        .box:nth-child(even) {
            background-color: #4682B4; /* 钢蓝色 */
        }
        .box:hover {
            background-color: #FFD700; /* 金色 */
            color: black;
        }
        .box:active {
            background-color: #6A5ACD; /* 板岩蓝色 */
        }
        .box:first-child {
            border: 5px solid #000; /* 黑色边框 */
        }
        .box:last-child {
            border: 5px solid #fff; /* 白色边框 */
        }
        .box:nth-of-type(3) {
            font-size: 30px;
        }
        .box:not(:nth-child(5)) {
            opacity: 0.8;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="box">1</div>
        <div class="box">2</div>
        <div class="box">3</div>
        <div class="box">4</div>
        <div class="box">5</div>
        <div class="box">6</div>
        <div class="box">7</div>
        <div class="box">8</div>
        <div class="box">9</div>
    </div>
</body>
</html>
```

### 解释

- **奇数格子** (`.box:nth-child(odd)`): 背景色设置为番茄色。
- **偶数格子** (`.box:nth-child(even)`): 背景色设置为钢蓝色。
- **悬停效果** (`.box:hover`): 当鼠标悬停在格子上时，背景色变为金色，文字颜色变为黑色。
- **激活效果** (`.box:active`): 当格子被点击时，背景色变为板岩蓝色。
- **第一个子元素** (`.box:first-child`): 为第一个格子添加黑色边框。
- **最后一个子元素** (`.box:last-child`): 为最后一个格子添加白色边框。
- **特定类型的第 n 个子元素** (`.box:nth-of-type(3)`): 将第三个格子的字体大小设置为30px。
- **非某个选择器的元素** (`.box:not(:nth-child(5))`): 将除第5个格子以外的所有格子的透明度设置为0.8。