# JS 代码优化经验

## 用 hashMap 代替 switch 语句

以随机选择东南西北为例，看看`switch`语句和`hashMap`实现的差异。

switch 语句

```javascript
let foo = Math.floor(Math.random()*4); // 随机产生一个 1 到 4 的整数
let output = 'Output: ';
switch (foo) {
  case 0:
    output += 'east';
  case 1:
    output += 'south';
  case 2:
    output += 'west';
  case 3:
    output += 'north';
  default:
    console.log('Please pick a number from 0 to 3!');
}
```

更换为 HashMap

```javascript
let foo = Math.floor(Math.random()*4);
let output = 'Output: ';
// 对象的键名如果是数值，会被自动转为字符串
let hashMap = {
  0:'east',
  1:'south',
  2:'west',
  3:'north'
}
// 虽然传入键名 foo 是数字，但会被自动转换为字符串
let appendContent = hashMap[foo] || 'Please pick a number from 0 to 3!'
// 不引入新变量 appendContent
// output += hashMap[foo] || 'Please pick a number from 0 to 3!'
```

总结：用 hashMap 代替 switch 语句

- 优点：少写了很多`case`和`output`
- 缺点：

- - `default:`需要用短路逻辑实现，代码不是很好读，对新手来说不是很好理解。
    - 引入一个新变量 `appendContent`，增加代码阅读的复杂性。