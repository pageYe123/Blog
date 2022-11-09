## Array.prototype

`arr.find((element, index, array) => { /* … */ } )` 在数组中找到符号要求的元素，返回这个元素

`arr.filter((element, index, array) => { /* … */ } )` 过滤出符合要求的元素，返回一个数组

```js
// find
let usersArray=[{name:'Bob'},{name:'Smith'},{name:'jeffrey'},{name:'frank'}]
let user = usersArray.find((user) => user.name === 'jeffrey')

// filter
let usersArray=[{age:17},{age:20},{age:19},{age:14}]
let user_arr = usersArray.filter((user) => user.age <= 18)
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
