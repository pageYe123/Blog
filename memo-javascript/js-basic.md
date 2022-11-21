## var、function 声明提前

1）`function`声明。函数提升且赋值提升。

```js
console.log(foo); // f foo(){}
function foo() {}
```

2）`var`声明。**声明提升但赋值不提升**，只有当执行到赋值语句时才赋值。

```js
console.log(a); // undefined
function logA() {
    console.log(a);
}
logA() // undefined
var a = 1 // 变量声明提前但并没有赋值
logA() // 1
```

2.1）`var`经典错误一：

```js
let o = {
    a: a
}
var a = 2
console.log(o.a); // undefined
```

等价于

```js
var a
let o = {
    a: a          // 此时 a 值是 undefined，赋值给 o.a
}
a = 2
console.log(o.a); // undefined
```

解决办法：把 o 变成函数。函数内会保留上层作用域中的变量。o 沿着作用域链逐个查找，在全局作用域中找到 a = 2

```js
let o = function () {
    // 作用域链 Local → Script → Global
    return {
        a: a
    }
}
var a = 2 // 给 Global 作用域中的 a 赋值
console.log(o().a); // 2
```

2.2）`var`经典错误二：

在给对象 o 赋值的时候，不要用自身给自己的属性赋值

```js
var o = {
    a: 2,
    b: o.a // Uncaught TypeError: Cannot read properties of undefined (reading 'a')
}
```

等价于

```js
var o
o = {
    a: 2,
    b: o.a // 此时 o 是 undefined
}
```



3）`let`、`const`声明。无法在变量（初始化）声明之前访问变量。

```js
console.log(b); //Uncaught ReferenceError: Cannot access 'b' before initialization
let b = 2
// 换成 const 也报相同错
```

## 对象的方法不要用箭头函数

想在函数中使用`this`，就不要用箭头函数。因为箭头函数中的`this`是上一作用域中的`this`，而非对象。

## 对象相等的新理解

两个对象，键名一样，键值一样，但两个对象不相等，因为它们存在内存中不同的位置。

这两个对象可以说是浅拷贝。

```js
let a=1
let obj1={a:a}
let obj2={a:a}
console.log(obj1===obj2) // false
```

