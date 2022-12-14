## RegExp instance

### 1) lastIndex

只有当正则表达式使用全局检索`g`或粘性检索`y`标志时，该属性才会起作用，否则值永远为 0。

## RegExp.prototype

### 1) test

作用：查看正则表达式和字符串之间是否有匹配项

返回值：如果有匹配项返回 true，否则返回 false。

示例：

```js
// 检测正则表达式是否在字符串中有匹配项
let str='test66t6s'
let reg=/\d/g
reg.test(str)       // 输出的结果：true
reg.lastIndex       // 5。如果不加 g，则为 0

// 通过 ^、$ 等正则语法，检测全部字符串是否符合要求
let str='a1234878'
let reg=/^\d+$/g
reg.test(str)       // 输出的结果：false。把开头的 'a' 去掉，则为true
```





## Array.prototype

### 1） find

`arr.find((element, index, array) => { /* … */ } )` 在数组中找到符号要求的元素，返回这个元素

```js
let usersArray = [{name:'Bob'}, {name:'Smith'}, {name:'jeffrey'}, {name:'frank'}]
let user = usersArray.find((user) => user.name === 'jeffrey') // {name: 'jeffrey'}
```

### 2) filter

`arr.filter((element, index, array) => { /* … */ } )` 过滤出符合要求的元素，返回一个数组

```js
let usersArray = [{age:17}, {age:20}, {age:19}, {age:14}]
let user_arr = usersArray.filter((user) => user.age <= 18) // [{age: 17}, {age: 14}]
```

## call、apply、bind 的用法分别是什么？

使用 apply、call、bind 可以改变函数内部的 this 的指向。

但它们的语法不同。

- apply 和 call 的区别是 call 方法接受的是若干个参数列表，而 apply 接收的是一个包含多个参数的数组或类数组。
- bind 方法创建一个新的函数，可以提前给定参数序列。必须手动调用函数。

```js
let fn = function () {
    console.log(this);
    console.log(arguments);
}
fn.call('0', 1, 2, 3)
fn.apply('0', [1, 2, 3]);
(fn.bind('0'))(1, 2, 3)
// 等价于
(fn.bind('0', 1))(2, 3)
```
