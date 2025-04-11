---
title: 一、JavaScript-数组
date: 2024-10-15 22:32:50
tags: JavaScript
---
#### 1 JavaScript 数组常用的方法

##### 前置条件

`值 => 表达式` 代表的是一个箭头函数的定义方式。下面是更详细的解释：

- **值**：这是箭头函数的参数，可以是一个或多个参数。如果只有一个参数，可以省略圆括号；如果有多个参数，则必须使用圆括号包裹起来。

  例如：

  - 单个参数：`num => ...`
  - 多个参数：`(num, num2) => ...`

- **表达式**：这是箭头函数的主体。如果箭头函数只有一个表达式，那么可以省略花括号（`{}`）和 `return` 关键字，直接返回这个表达式的结果。

  例如：

  - `num => num * 2`：返回 `num` 的两倍。
  - `num => { console.log(num); }`：打印 `num`，但因为有花括号，所以需要使用 `return` 才能返回值。

`[...array]`：

- 这是扩展运算符（Spread Operator）的使用，它用于将数组 `numbers` 的所有元素展开成一个新的数组。

  例如：

  - `const array = [1, 9, 6, 3, 7, 4, 5]`; `const sorted = [...array]`; `sorted`是复制原数组得到的新数组。 

##### 1 纯数组

```javascript
const numbers = [1, 9, 6, 3, 7, 4, 5];

// forEach() （循环遍历数组）
numbers.forEach(num => console.log(num)); // 1, 9, 6, 3, 7, 4, 5

// map() （创建新数组，包含每个元素的平方）
const squares = numbers.map(num => num * num);
console.log(squares); // [1, 81, 36, 9, 49, 16, 25]

// filter() （过滤出大于5的元素）
const filtered = numbers.filter(num => num > 5);
console.log(filtered); // [9, 6, 7]

// reduce() （计算数组元素的总和）
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 35

// find() （查找第一个大于5的元素）
const found = numbers.find(num => num > 5);
console.log(found); // 9

// some() （检查是否存在偶数）
const hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven); // true

// every() （检查是否所有元素都是正数）
const allPositive = numbers.every(num => num > 0);
console.log(allPositive); // true

// sort() （按升序排序数组）
const sorted = [...numbers].sort((a, b) => a - b);
console.log(sorted); // [1, 3, 4, 5, 6, 7, 9]

// reverse() （反转数组顺序）
const reversed = [...numbers].reverse();
console.log(reversed); // [5, 4, 7, 3, 6, 9, 1]

// includes() （检查数组是否包含5）
const includesFive = numbers.includes(5);
console.log(includesFive); // true

// indexOf() （查找9在数组中的索引）
const index = numbers.indexOf(9);
console.log(index); // 1

// splice() （删除索引2的元素，并插入100）
const modified = [...numbers];
modified.splice(2, 1, 100);
console.log(modified); // [1, 9, 100, 3, 7, 4, 5]

// slice() （提取索引1到4的片段）
const sliced = numbers.slice(1, 4);
console.log(sliced); // [9, 6, 3]

// push() （向数组末尾添加元素）
numbers.push(10);
console.log(numbers); // [1, 9, 6, 3, 7, 4, 5, 10]

// pop() （删除数组末尾的元素）
const popped = numbers.pop();
console.log(popped); // 10
console.log(numbers); // [1, 9, 6, 3, 7, 4, 5]

```

##### 2 数组对象

```javascript
const data = [
    { id: 232, fnumber: "01030252_V1.0", fmaterialgroupname: "草甘膦水剂" },
    { id: 233, fnumber: "01030252_V1.0", fmaterialgroupname: "妙耕桶签" },
    { id: 234, fnumber: "01030252_V1.0", fmaterialgroupname: "41%草甘膦桶" },
    { id: 255, fnumber: "01030252_V1.1", fmaterialgroupname: "外购半成品" },
    { id: 256, fnumber: "01030252_V1.1", fmaterialgroupname: "妙耕桶签" },
    { id: 257, fnumber: "01030252_V1.1", fmaterialgroupname: "41%草甘膦桶" }
];

// forEach() （循环遍历数组对象）
data.forEach(item => console.log(item)); // 打印每个对象

// map() （提取fnumber属性，创建新数组）
const fnumbers = data.map(item => item.fnumber);
console.log(fnumbers); // ["01030252_V1.0", "01030252_V1.0", "01030252_V1.0", "01030252_V1.1", "01030252_V1.1", "01030252_V1.1"]

// filter() （过滤出唯一的fnumber对象）
const uniqueItems = data.filter((item, index, self) =>
    index === self.findIndex(t => t.fnumber === item.fnumber)
);
console.log(uniqueItems); // 唯一fnumber的对象数组

// reduce() （统计每个fnumber的数量）
const countByFnumber = data.reduce((acc, item) => {
    acc[item.fnumber] = (acc[item.fnumber] || 0) + 1;
    return acc;
}, {});
console.log(countByFnumber); // { "01030252_V1.0": 3, "01030252_V1.1": 3 }

// find() （查找包含“桶”的项目）
const foundItem = data.find(item => item.fmaterialgroupname.includes("桶"));
console.log(foundItem); // 找到的第一个包含“桶”的对象

// some() （检查是否存在“草”的产品）
const hasGrass = data.some(item => item.fmaterialgroupname.includes("草"));
console.log(hasGrass); // true

// every() （检查所有对象是否都有id属性）
const allHaveId = data.every(item => item.id !== undefined);
console.log(allHaveId); // true

// sort() （按id排序数组对象）
const sortedById = [...data].sort((a, b) => a.id - b.id);
console.log(sortedById); // 按id升序排序的对象数组

// reverse() （反转数组对象顺序）
const reversedData = [...data].reverse();
console.log(reversedData); // 反转后的对象数组

// includes() （检查fnumber数组中是否存在某个值）
const fnumberExists = fnumbers.includes("01030252_V1.0");
console.log(fnumberExists); // true

// indexOf() （查找某个fnumber在数组中的索引）
const indexOfFnumber = fnumbers.indexOf("01030252_V1.0");
console.log(indexOfFnumber); // 0

// splice() （删除索引2的对象，并插入新对象）
const modifiedData = [...data];
modifiedData.splice(2, 1, { id: 300, fnumber: "新项目", fmaterialgroupname: "新材料" });
console.log(modifiedData); // 删除后的新数组对象

// slice() （提取数组对象的片段）
const slicedData = data.slice(1, 4);
console.log(slicedData); // [索引1到4的对象数组]
```

