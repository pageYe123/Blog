# JS 新语法&新概念

## `[@@match]`、`[@@iterator]`

`@@` 并不是 JS 中真实的语法，它描述的是一个 symbol 方法名。这些标记实际对应的方法名如下：

| `[@@<methodName>]` | 实际对应的方法名                 |
| ------------------ | -------------------------------- |
| `[@@match]`        | `RegExp.prototype[Symbol.match]` |
| `[@@iterator]`     | `Array.prototype[@@iterator]`    |
| `[@@toPrimitive]`  | `Date.prototype[@@toPrimitive]`  |

以 `RegExp.prototype[Symbol.match]` 为例。只要一个对象实现了 `Symbol.match` 方法，它就可以作为 `String.prototype.match()`的参数。比如：

```js
let str = "Hmm, this is interesting.";
let obj = {
  [Symbol.match](str) {
    return ["interesting."];
  }
}

console.log(str.match(obj)); // ["interesting."]
```

同样地，在正则表达式的内部存在`Symbol.match`方法，接受 str 作为参数。

```js
const re = /[0-9]+/g
const str = "2016-01-02"
const result = re[Symbol.match](str) // 等价于 str.match(re)
console.log(result) // ["2016", "01", "02"]
```



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

注意`import`语法中，相对路径必须有`./`，否则报错。

## 运算符

###  `x ?? y`

空值合并运算符（Nullish coalescing operator）。

当 x 为 `null` 或者 `undefined` 时，返回 y；否则返回 x。

```js
const nullValue = null;
const emptyText = ""; // 空字符串，虽然是 falsy 值，但是不是 null 或 undefined

const valA = nullValue ?? "valA 的默认值"; // "valA 的默认值"
const valB = emptyText ?? "valB 的默认值"; // ""
```

### `x ??= y`

空值合并赋值运算符（Nullish coalescing assignment）

当 x 为 `null` 或者 `undefined` 时，给 x 赋值，否则不赋值。

```js
// 等价于 x ?? (x = y)
// 不等价于 x = (x ?? y)
const a = { duration: 50 };

a.duration ??= 10; console.log(a.duration); // 50
a.speed ??= 25; console.log(a.speed);       // 25
```

### `x ||= y` 

逻辑或赋值运算符 Logical OR assignment。

当 x 为 falsy 值时，对 x 赋值。

```js
const x = 1 // x 不可重新赋值
x ||= 2
// 等价于 x || (x = 2)
// 不等价于 x = x || 2，给 x 重新赋值了，会报错。
```

注意：ES 2021（第 12 版）才引进，低版本的 node.js 不支持该语法。

## 可迭代对象（iterable object）

包括如下构造函数的实例：Array，String，TypedArray，Map，Set，NodeList，DOMcollections，以及 arguments，generator functions 产生的 Generator Object， 还有 user-defined iterables。

**将类数组对象变为可迭代对象：**
对于 Object 来说，是没有部署 Iterator 接口的。类数组对象是一个对象，所以不能像数组一样通过迭代器遍历。

```js
let arrLike = { 0: "z", 1: "x", 2: "y", length: 3 }

// 方法一：借用别人的迭代器
arrLike[Symbol.iterator] = [][Symbol.iterator]
// [][Symbol.iterator] === Array.prototype[Symbol.iterator] // true

// 方法二：创建一个新的迭代器
arrLike[Symbol.iterator] = function* () {
    for (let key in this) {
        if (/^\d+$/.test(key)) { // filter out `length` property
            yield this[key]
        }
    }
}

for (let a of arrLike) {
    console.log(a);
}
```



## for...of

在 ES6 中引入了 `for...of` 循环，遍历[可迭代对象](#可迭代对象（iterable object）)（*iterable object*）。可以作为遍历所有数据结构的统一的方法。

`for...in` VS. `for...of`：

- `for...in` 遍历对象的可枚举属性，包含自身的和原型链上的属性
- `for...of` 遍历「可迭代对象」预先定义好的可迭代的值

```js
Object.prototype.objCustom = function () {};
Array.prototype.arrCustom = function () {};

const iterable = [3, 5, 7];
iterable.foo = "hello";

for (const i in iterable) {
  console.log(i);
}
// "0", "1", "2", "foo", "arrCustom", "objCustom"
// "0" "1" "2" "foo" 为自身属性，后两者为原型链上的属性

for (const i of iterable) {
  console.log(i);
}
// 3 5 7
```



## Map 数据结构

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

## WeakMap 数据结构

WeakMap 和 Map 的区别：

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

**应用场景：**

```js
// 1. 函数的参数是个对象，从函数参数中获取属性值
let obj = { data: { n: 0 } }
function foo({ data }) { // 获取参数中属性 data 的值
    console.log(data) // { n: 0 }
}
foo(obj)

// 2. 对象的解构赋值，可以很方便地将现有对象的方法，赋值到某个变量。
// 将 Math 对象的对数、正弦、余弦三个方法，赋值到对应的变量上，使用起来就会方便很多。
let { log, sin, cos } = Math; // 注意 log 存在命名冲突
let { log } = console;
log('hello') // hello 
```



## 展开语法

**应用场景：**

```js
// 1. 函数的参数数量不确定时，用展开运算符
function foo(...args){ // 此时 args 是真正的数组，而非 arguments
	console.log(args) // [1,2,3,4,5,6]
}
foo(1,2,3,4,5,6)

// 2. 展开运算符复制数组
let result1 = [1, 2, 3, 4]
let result2 = [...result1]

// 3. 展开运算符拼接数组，不改变原数组
let result1 = [1, 2, 3, 4],
    result2 = [5, 6, 7, 8]
let result3 = [
    ...result2,
    ...result3
]
// 等价于 let result3 = result1.concat(result2) // 同样不改变原数组
```



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