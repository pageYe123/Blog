# JS 新语法

## JS 模块

变量、函数可以作为模块导出导入。

```js
// 默认导出给外部使用
export default c
import c from './xxx.js' /*注意导入时不能加花括号*/
/*等价于*/ import anotherName from './xxx.js' 

// 命名导出与导入，模块名称必须一致，或用重命名导出
export { c }
import { c } from './xxx.js' 
/*等价于*/ import { c as anotherName } from './xxx.js' 

// 重命名导出与导入
export { c }
import { c as x } from './xxx.js'
```

## new Map()

```js
// 语法
new Map(iterable)

// 示例
const myMap = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);
```

JS 内建的可迭代对象：

- 非 weak 的数据结构，包括 Array，Set，Map
- String 对象
- 类数组
    - DOM 中的 NodeList 对象、HTMLCollection 对象
    - 函数的 arguments 属性，以及函数内部 arguments 局部变量。

## Map 和 WeakMap 的区别

1. Map 对象的键可以是任何类型，但 WeakMap 对象中的键只能是对象引用。
2. WeakMap 不能包含无引用的对象，否则会被自动清除出集合（垃圾回收机制）。 
3. WeakMap 对象是不可枚举的，无法获取大小。也无法清空。

```js
let weakmap = new WeakMap();
(function () {
    let o = { n: 1 };
    weakmap.set(o, "A");
})();  // here 'o' key is garbage collected
```

```js
let obj1 = {}
let obj2 = {}
let weakmap_1 = new WeakMap([[obj1, 2], [obj2, 5]]); // obj1 作为 key，2 作为 value
console.log(weakmap_1.get(obj2)); // 5
// 等价于
let weakmap_2 = new WeakMap()
weakmap_2.set(obj1, 2)
weakmap_2.set(obj2, 5)
```



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

## 属性的简洁表示法

ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。

```js
let foo = 'bar';
let baz = {foo};
baz // {foo: "bar"}

// 等价于
const baz = {foo: foo};
```

例二：

```js
let name = 'jeffrey'
let age = 18
let str = JSON.stringify({name,age})
// 等价于
JSON.stringify({name: name, age: age})
```



## 对象的解构赋值

语法：`let {对象的属性名} = 对象`

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
let { log } = console;
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