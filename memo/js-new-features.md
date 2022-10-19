# JS 新语法

## 箭头函数

```javascript
// 如果用箭头函数，其中的this不指向类的实例，它只会从自己的作用域链的上一层继承 this。
Animal.prototype.sayName = function(){console.log(this.name)}
```

## 数组的方法

**要明确方法是否改变原数组的内容。**

不改变原数组的内容：

```js
arr.concat(newArray)
arr.slice(start, end)
// 待总结....
```

比如 `arr.concat(newArray)`，返回一个新数组，并不改变原数组的内容。

应用场景：将多个数组内容归并到一个数组中，需要赋值！需要赋值！！需要赋值！！！

```javascript
let arr = []
let arr1 = [1,2,3]
let arr2 = [4,5,6]
let arr3 = [7,8,9]
// 必须重新赋值给 arr
arr = arr.concat(arr1).concat(arr2).concat(arr3)
```

## 对象的解构赋值

语法：`const {对象的属性名} = 对象`

作用：相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。所有的键和值，都会拷贝到新对象上面。

```javascript
let obj = { foo: 'aaa' }
let { foo } = obj
console.log(foo) // "aaa"

// 等价于
let foo = obj.foo
```

对象的解构赋值，可以很方便地将现有对象的方法，赋值到某个变量。

```javascript
// 例一
let { log, sin, cos } = Math;

// 例二
const { log } = console;
log('hello') // hello 
```

上面代码的例一将 `Math` 对象的对数、正弦、余弦三个方法，赋值到对应的变量上，使用起来就会方便很多。

## new.target

函数内部用`new.target`探测函数是否用 new 操作符调用

```javascript
function Foo() {
    if (new.target) { throw 'Foo() can\'t be called with new'; }
}
new Foo()
```

参考资料：

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target