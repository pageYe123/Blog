## 属性描述符

查看对象某个属性的属性描述符:

```javascript
Object.getOwnPropertyDescriptor(obj, prop)
```

设置对象某个属性的属性描述符：

```js
Object.defineProperty(obj, prop, descriptor)
```

一个属性描述符有两类：①数据描述符（data descriptor）②属性访问器描述符（accessor descriptor）。

这两个属性描述符都可以包含以下属性：

- configurable 只有当属性描述符可以被改变，并且属性可以被删除时，值为 true。默认为 false。
- enumerable 属性是否可枚举。默认为 false。会影响以下语法：
    - `for...in` 遍历自身及原型链上的可枚举属性。
    - `Object.keys()` 遍历对象可枚举属性名。
        `Object.values()` 遍历对象可枚举属性值。
        `Object.entries()` 拆分对象的属性名和属性值，返回一个由「对象的可枚举属性的键值对」组成的数组。
    - `JSON.stringify()` 只能遍历自身可枚举属性。


数据描述符还可以包含：

- writable 属性值是否可变。默认为 false。
- value 属性的值。默认为 undefined。

属性访问器描述符还可以包含：

- get 属性访问器取值函数（getter），默认为 undefined。
- set 属性访问器存值函数（setter），默认为 undefined。

**注意：「数据描述符」和「属性访问器描述符」不可同时使用。**

官方文档：

- [Object.defineProperty() | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

- [Object.getOwnPropertyDescriptor() | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

    



## 判断对象是否有某属性

```js
// 有属性返回 true
obj.hasOwnProperty(prop) // 元素自身是否有该属性，不论是否可枚举
prop in object           // 元素自身及原型链上是否有该属性，不论是否可枚举
```

4 种**属性性质**与 API：

| API     | 可枚举，自身 | 可枚举，继承 | 不可枚举，自身 | 不可枚举，继承 |
| :--------------------------: | :----------: | :----------: | :------------: | :------------: |
| obj.**hasOwnProperty**(prop) | ✅            | ❌            | ✅              | ❌              |
| prop **in** object           | ✅            | ✅            | ✅              | ✅              |

说明：✅表示函数返回值或语句表达式的值为 true



## 遍历对象属性

不可枚举属性无法通过遍历获得，只能通过 `console.dir` 或 `console.log` 查看。

|                                                    | 可枚举，自身 | 可枚举，继承 | 不可枚举，自身 | 不可枚举，继承                        |
| :------------------------------------------------- | :----------- | ------------ | -------------- | ------------------------------------- |
| Object.keys<br />Object.values<br />Object.entries | ✅            | ❌            | ❌              | ❌                                     |
| for...in                                           | ✅            | ✅            | ❌              | ❌<br />（如：原型链上预先内置的属性） |

说明：✅表示这类属性可以被访问到

参考资料：[MDN：Enumerability and ownership of properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)

### 1）Object.keys(obj)

遍历自身可枚举属性

```js
Object.keys(obj) // 返回数组
```



### 2）for...in（不同于 in 操作符）

只能遍历出自身及原型链中的可枚举属性。内置构造器（Array，Object）的 prototype 中的共有属性是不可枚举属性，for...in 无法遍历出这些属性。

语法：

```js
for (const variable in object) {
  statement
}
```

举例：

```js
let common = { kind: 'human' }
let obj = Object.create(common) // 以 common 为原型创建一个对象 obj
obj.name = 'jeffrey'
console.dir(obj) // 能看到 obj 的原型上有 kind 属性
console.dir(obj.__proto__) //{kind: 'human'}
for (const key in obj) {
    // 此句输出 name, kind。无法遍历 Object.prototype 上的共有属性。
    console.log(key)
}
```







