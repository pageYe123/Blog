# JS 代码优化经验

## 抽象思维

### 1）事不过三

- 同样的代码写三遍，就应该抽成一个函数
- 同样的属性写三遍，就应该做成**共用属性（原型或类）**
- 同样的原型写三遍，就应该用**继承**；
    不同类有同样的方法，就应该用**继承**

代价：
有时候会造成继承层级太深，无法一下看懂代码，可以通过写文档、画类图解决。

### 2）表驱动编程

表指的是哈希表。

表驱动法就是一种编程模式(scheme)——从表里面查找信息而不使用逻辑语句(if 和 case)。对简单的情况而言，使用逻辑语句更为容易和直白。但**随着逻辑链的越来越复杂，查表法会更具吸引力**。

```js
// 表驱动编程示例
function translate(number) {
  let numberList = {
  '1': '一',
  '2': '二',
  '3': '三'
  }
  return numberList[number];
}
```

表驱动编程可以减少重复代码，只将重要的信息放在表里，然后利用表来编程。当你看到大批类似但不重复的代码，看看哪些是不同的数据，把这些重要数据做成哈希表，代码就简单了。

示例：在`autobindEvents`函数中根据`events`哈希表自动绑定事件。

![示例](https://cdn.nlark.com/yuque/0/2022/png/29373291/1667298874991-a70373a4-1503-4b9a-80bc-afada89d65fb.png)

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

更换为 hashMap

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

  - `default:`需要用短路逻辑实现，代码不是很好读，对新手来说不是很好理解。
  - 引入一个新变量 `appendContent`，增加代码阅读的复杂性。