---
title: 二、JavaScript-内置常用的方法
date: 2025-04-11 22:32:50
tags: JavaScript
---
### 1. JSON

#### 1.JSON.parse

将 JSON 字符串解析为 JavaScript 对象。

```javascript
const jsonString = '{"name":"Alice","age":25}';
const obj = JSON.parse(jsonString);
console.log(obj.name); // 输出: Alice
```

#### 2. JSON.stringify()

将 JavaScript 对象转换为 JSON 字符串。

```javascript
const obj = { name: "Bob", age: 30 };
const jsonString = JSON.stringify(obj);
console.log(jsonString); // 输出: {"name":"Bob","age":30}
```

#### 3. JSON.stringify(obj, replacer, space)

`replacer`：用于过滤或转换属性
 `space`：格式化缩进，常用于打印美观的 JSON 字符串

```javascript
const obj = { name: "Charlie", age: 28, password: "secret" };

// 过滤 password 字段
const json1 = JSON.stringify(obj, ["name", "age"]);
console.log(json1); // {"name":"Charlie","age":28}

// 缩进格式化（常用于开发调试）
const prettyJson = JSON.stringify(obj, null, 2);
console.log(prettyJson);
/*
{
  "name": "Charlie",
  "age": 28,
  "password": "secret"
}
*/
```

#### 4. JSON.parse() （try...catch）

解析非法 JSON 字符串时会抛出错误，建议包裹在 try...catch 中：

```javascript
const badJson = '{ name: "Alice" }'; // 非法 JSON（属性名未加双引号）

try {
  const obj = JSON.parse(badJson);
} catch (err) {
  console.error("解析失败:", err.message);
}
```

- JSON 中 key 和 string 值都必须是双引号（"），不能用单引号
- undefined、函数、Symbol 类型无法被序列化

### 2.String

#### 1. String.prototype.split

将字符串按指定分隔符拆分为数组。

```javascript
const str = "apple,banana,orange";
const fruits = str.split(",");
console.log(fruits); // 输出: ["apple", "banana", "orange"]
```

#### 2. String.prototype.replace

替换字符串中的部分内容。

```javascript
const str = "Hello, World!";
const newStr = str.replace("World", "JavaScript");
console.log(newStr); // 输出: Hello, JavaScript!
```

#### 3. String.prototype.trim

去除字符串两端的空白字符。

```javascript
const str = "   Hello, World!   ";
const trimmedStr = str.trim();
console.log(trimmedStr); // 输出: "Hello, World!"
```

### 3.Array

#### 1. Array.prototype.map

对数组中的每个元素执行回调函数，并返回一个新数组。

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map(num => num * 2);
console.log(doubled); // 输出: [2, 4, 6]
```

#### 2. Array.prototype.filter

根据条件过滤数组，返回满足条件的新数组。

```javascript
const numbers = [1, 2, 3, 4, 5];
const evens = numbers.filter(num => num % 2 === 0);
console.log(evens); // 输出: [2, 4]
```

#### 3. Array.prototype.reduce

将数组中的元素累积为一个值。

```javascript
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 输出: 10
```

#### 4. Array.prototype.forEach

遍历数组，对每个元素执行回调函数（无返回值）。

```javascript
const numbers = [1, 2, 3];
numbers.forEach(num => console.log(num)); // 依次输出: 1, 2, 3
```

#### 5. Array.prototype.includes

检查数组是否包含某个值，返回布尔值。

```javascript
const numbers = [1, 2, 3];
console.log(numbers.includes(2)); // 输出: true
console.log(numbers.includes(4)); // 输出: false
```

#### 6. Array.prototype.find

查找数组中满足条件的第一个元素。

```javascript
const numbers = [1, 2, 3, 4];
const found = numbers.find(num => num > 2);
console.log(found); // 输出: 3
```

#### 7. Array.prototype.some

检查数组中是否有至少一个元素满足条件。

```javascript
const numbers = [1, 2, 3, 4];
const hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven); // 输出: true
```

#### 8. Array.prototype.every

检查数组中的所有元素是否都满足条件。

```javascript
const numbers = [2, 4, 6, 8];
const allEven = numbers.every(num => num % 2 === 0);
console.log(allEven); // 输出: true
```

### 3. Object

#### 1. Object.keys

获取对象的所有键，返回一个数组。

```javascript
const obj = { name: "Alice", age: 25 };
const keys = Object.keys(obj);
console.log(keys); // 输出: ["name", "age"]
```

#### 2. Object.values

获取对象的所有值，返回一个数组。

```javascript
const obj = { name: "Alice", age: 25 };
const values = Object.values(obj);
console.log(values); // 输出: ["Alice", 25]
```

#### 3. Object.entries

获取对象的键值对，返回一个二维数组。

```javascript
const obj = { name: "Alice", age: 25 };
const entries = Object.entries(obj);
console.log(entries); // 输出: [["name", "Alice"], ["age", 25]]
```

#### 4. Object.assign

将一个或多个对象的属性合并到目标对象。

```javascript
const obj1 = { a: 1 };
const obj2 = { b: 2 };
const merged = Object.assign({}, obj1, obj2);
console.log(merged); // 输出: { a: 1, b: 2 }
```

### 4. Function.prototype.bind

#### 1. Function.prototype.bind

创建一个新函数，绑定指定的 `this` 值和参数。

```javascript
const person = {
  name: "Alice",
  greet: function() {
    console.log(`Hello, ${this.name}`);
  }
};

const greet = person.greet.bind(person);
greet(); // 输出: Hello, Alice
```

### 5. Promise

用于处理异步操作。

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Success!"), 1000);
});

promise.then(result => console.log(result)); // 1秒后输出: Success!
```

### 6. async/await

简化异步操作的语法。

```javascript
async function fetchData() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();
  console.log(data);
}

fetchData();
```

### 7. Set

用于存储唯一值的集合。

```javascript
const numbers = [1, 2, 2, 3, 4, 4];
const uniqueNumbers = [...new Set(numbers)];
console.log(uniqueNumbers); // 输出: [1, 2, 3, 4]
```

### 8. Map

用于存储键值对，键可以是任意类型。

```javascript
const map = new Map();
map.set("name", "Alice");
map.set("age", 25);

console.log(map.get("name")); // 输出: Alice
console.log(map.size); // 输出: 2
```

### 9. Date

用于处理日期和时间。

```javascript
const now = new Date();
console.log(now.toISOString()); // 输出当前时间的 ISO 格式字符串
```

### 10. Math

提供数学运算的属性和方法。

```javascript
console.log(Math.max(1, 2, 3)); // 输出: 3
console.log(Math.random()); // 输出: 0到1之间的随机数
```

### 11. RegExp

用于处理正则表达式。

```javascript
const regex = /hello/i;
console.log(regex.test("Hello, World!")); // 输出: true
```

### 总结

JavaScript 提供了丰富的内置方法，涵盖了数据处理、异步操作、集合操作等多个方面。掌握这些方法可以极大地提高开发效率！