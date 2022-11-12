## 概念

- 字面量

    字面量：literal，用于表达源代码中一个固定值的表示法（notation）。JS 中对象字面量的概念。

	```js
	let foo = {}
	```
	
- 类型检查
	JS 提供两种类型检查操作符：
  
	- `typeof` 用于检查基本数据类型。`typeof null`返回`object`是怪异的。
	- `instanceof` 用于检查对象是否是类的实例。基本数据类型不能使用`instanceof`操作符。
	
	参考资料：[Is there a way to use instanceof for primitive JavaScript values?](https://www.30secondsofcode.org/articles/s/javascript-primitive-instanceof)

## 循环、递归、迭代、遍历

while 语句中有两种方式跳出循环：①圆括号中的表达式 ②break语句
跳出循环的代码一般往上写：

```javascript
while (!ele.matches(selector)) {
    if (ele === eleDelegator) { // 判断是否跳出循环，一般往上写，不写在下面。
        ele = null
        break;
    }
    ele = ele.parentNode
}
```

## JS 的对象体系

- 类和实例

    > 典型的面向对象编程语言（比如 C++ 和 Java），都有“类”（class）这个概念。所谓“类”就是对象的模板，对象就是“类”的实例。实例可以访问类的「实例方法」和「成员变量」。
    >
    > 但是，JavaScript 语言的对象体系，不是基于“类”的，而是基于构造函数（constructor）和原型链（prototype chain）。JavaScript 语言使用构造函数（constructor）作为对象的模板。所谓”构造函数”，就是专门用来生成实例对象的函数。它就是对象的模板，描述实例对象的基本结构。一个构造函数，可以生成多个实例对象，这些实例对象都有相同的结构。

- instanceof 操作符用于检验「构造器」的 prototype 属性是否在「对象」的原型链上。构造器（constructor）也被称作构造函数。
    语法：`<object> instanceof <constructor>`

- JS 中所有的函数都是对象，我们平常说「函数」相当于「函数对象」的简称。
    之所以强调这一点，是想说明 JS 中除 7 种基本数据类型以外，都是对象。换句话说，数组、函数、Date、RegExp 都是对象。
    由于 Object.prototype 位于所有原型链的最顶端，~~所以用 instanceof 操作符可以检验，这些对象要么是 Object 的实例（由 Object 构造出来），要么对象的构造函数继承 Object~~。但有一个例外：Function。

    ```js
    let arr = [1,2,3]
    let f1 = function(){}
    // instance 用以判断构造函数的 prototype 属性是否出现在一个对象的原型链上。
    console.log(arr instanceof Object) // true
    console.log(f1 instanceof Object) // true
    ```

- Function 是谁构造出来的？所谓「构造」就是相当于做了 new 做的事情。为什么 Function 的 constructor 不指向 Object ，而指向它自己？**

    ```js
    console.log(Function instanceof Object) // true
    console.log(Function.constructor === Function) // true
    ```

    实际上，Function 由浏览器构造的，它是最顶层的构造器，它构造了 Object。浏览器指定 Function.constructor 值为 Function。不仅如此它还构造了 Array，普通函数

- 



## 对象方法的定义

从 ES6 开始，在初始化对象的时候，可以用更简短的语法定义对象的方法。这种方法比箭头函数还简介

```js
const obj = {
  foo() {						//对比旧方法   //foo:()=>{}   // foo:function(){}
    return 'bar';
  }
}

console.log(obj.foo());
// expected output: "bar"
```

参考资料：[MDN：Method definitions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions)

## this

```javascript
// 任何情况下，不可以给 this 赋值
// Uncaught SyntaxError: Invalid left-hand side in assignment
this = 1 

// 但可以给 this 的属性赋值
function Square(width){
	this.width = width  
}
```

## 原型链

### 内置对象的原型链

有一张图很厉害。

参考资料：

[stackoverflow：What is in `Object.__proto__`?](https://stackoverflow.com/questions/40920909/what-is-in-object-proto)



### prototype 属性

<u>一般只有函数才有 prototype 属性。</u>

所有函数一出生就有一个 prototype 属性<u>（除了箭头函数、内置对象的方法如`Object.create()`）</u>。Object，Array 也都是函数，都有 prototype。

所有的 prototytpe 一出生就有 constructor 属性，并且属性值指向函数自身。

如果一个对象不是函数，那么这个对象一般来说没有 prototype 属性，但这个对象一般来说会有 `__proto__`属性。比如`[1,2,3]`、`{foo:'bar'}`都没有 prototype 属性。

### `__proto__`并不在对象自身上

提问：为什么`foo.hasOwnProperty('__proto__')`等于 false？

回答：`__proto__`根本就不在对象的自身上，**它是`Object.prototype`的自身属性，其他对象的`__proto__`都继承自这个属性。**`__proto__`对应有 getter/setter 方法，其他对象查看`__proto__`时，会通过 getter 方法返回各自不同的值。

```js
var foo = {}
foo.hasOwnProperty('__proto__')              // false
Object.prototype.hasOwnProperty('__proto__') // true
```

提问：为什么 Chrome 上通过`console.log(obj)`或`console.dir(obj)`明明看到`__proto__`属性在对象自身上？

回答：这只是 Chrome 给出的提示，提示我们对象有「原型」的，可以通过「原型链」访问对象的共有属性。

同理：

```js
// arguments getter/setter 只在 Function.prototype 上
Function.prototype.hasOwnProperty('arguments') // true

// caller getter/setter 只在 Function.prototype 上
Function.prototype.hasOwnProperty('caller') // true
```



参考资料：

stackoverflow: [Why is foo.hasOwnProperty(`__proto__`) equal to false?](https://stackoverflow.com/questions/24295785/why-is-foo-hasownproperty-proto-equal-to-false)



## 属性描述符

下面实验的结论：对象的属性名最好不要和原型链上的属性名冲突。冲突的话，可能会覆盖掉原型链上的共有属性，有用的功能就用不上；另一方面，属性名冲突，有可能出现无法新增属性名的情况。

```js
let obj = {}
let common = function () { }
// common 中含有的属性：arguments、caller、length、name
// 前两个属性不可 configurable，后两个可以 configurable

// 发现问题：当把一个对象的原型指向一个函数时，obj 无法新增 name 属性
obj.__proto__ = common
obj.name = 123
console.log(obj.name) // common，修改 name 属性怎么会不生效呢？

// 问题原因及解决办法
Object.getOwnPropertyDescriptor(obj.__proto__, 'name')
Object.defineProperty(obj.__proto__, 'name', {
    writable: true
})
obj.name = 124
console.log(obj.name) // 124，终于可以修改了
```

新发现：对象的某些属性没有属性描述符。

```js
Object.getOwnPropertyDescriptor(Object.prototype.hasOwnProperty, '__proto__') // undefined
```

## JS 引擎

### JS 引擎包括 JS 解释器

> `eval()` is also slower than the alternatives, since it has to invoke the JavaScript interpreter, while many other constructs are optimized by modern JS engines.

`eval()`的速度也比其他方法慢，因为它必须调用 JavaScript 解释器，而其他许多结构都被现代 JS 引擎优化。

参考资料：[MDN：never-use-eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval!)

## 其他

`function fn(){}`的 name 属性的属性描述符：

```js
configurable: true
enumerable: false
writable: false
```



