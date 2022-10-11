## 属性描述符

查看对象某个属性的属性描述符:

```javascript
Object.getOwnPropertyDescriptor(obj, prop)
```

一个属性描述符是一个记录，由下面属性当中的某些组成的：

- configurable 只有当属性描述符可以被改变，并且属性可以被删除时，值为 true。默认为 false。
- enumerable 属性是否可枚举。默认为 false。
- writable 属性值是否可变。默认为 false。
- value 属性的值
- get 访问器函数（getter）
- set 设置器函数（setter）

属性描述符：[MDN：Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)



## 判断对象是否有某属性

```js
// 有属性返回 true
obj.hasOwnProperty(prop) // 元素自身是否有该属性，不论是否可枚举
prop in object           // 元素自身及原型链上是否有该属性，不论是否可枚举
```



|                              | 可枚举，自身 | 可枚举，继承 | 不可枚举，自身 | 不可枚举，继承 |
| :--------------------------- | :----------- | ------------ | -------------- | -------------- |
| obj.**hasOwnProperty**(prop) | ✅            | ❌            | ✅              | ❌              |
| prop **in** object           | ✅            | ✅            | ✅              | ✅              |

说明：✅表示返回值或表达式的值为 true



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







