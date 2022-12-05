阅读资料：https://www.jianshu.com/p/c5aa1eee8dfd

## 一、`Object` 静态方法

方法位于`Object`上。

```js
console.dir(Object)
```



**1.1）Object.assign()**

**1.2）Object.create()**

以指定对象为原型，创建一个新对象。

语法：

```js
Object.create(proto)
Object.create(proto, propertiesObject)

// 第一个参数为对象，对象为「函数返回新对象」的原型
// 第二个参数为对象的实例方法，默认不能赋值、不能枚举、不能修改属性描述符。
```

示例：

```js
let Cat = function () { }
let Animal = function () { }
Cat.prototype = Object.create(Animal.prototype, {
    // 由于 Cat.prototype 被重写，如果不设置 Cat.prototype.Constructor 指向 Cat，它会指向 Animal。
    constructor: {
        value: Cat,         // 属性值
        writable: true,     // 是否可以赋值
        enumerable: true,   // 是否可枚举
        configurable: true  // 是否可以修改以上几项配置
    }
})
let cat = new Cat()
console.log(cat.constructor); // Cat
```



**1.3）Object.defineProperties()**

**1.4）Object.defineProperty()**

**1.5）Object.entries()**

**1.6）Object.freeze()**

**1.7）Object.fromEntries()**

**1.8）Object.getOwnPropertyDescriptor()**

**1.9）Object.getOwnPropertyDescriptors()**

**1.10）Object.getOwnPropertyNames()**

**1.11）Object.getOwnPropertySymbols()**

**1.12）Object.hasOwn()**

**1.13）Object.is()**

**1.14）Object.isExtensible()**

**1.15）Object.isFrozen()**

**1.16）Object.isSealed()**

**1.17）Object.keys()**

**1.18）Object.preventExtensions()**

**1.19）Object.seal()**

**1.20）Object.setPrototypeOf()**

将一个指定的对象的原型设置为另一个对象

```js
class Animal { }
class Cat { }
Object.setPrototypeOf(Cat.prototype, Animal.prototype); // 等价于 B extends A

Animal.prototype.isPrototypeOf(Cat.prototype) // true
```



**1.21）Object.values()**

## 二、`Object` 实例方法

方法位于`Object.prototype`上。

```js
console.dir(Object.prototype)
```

**2.1）hasOwnProperty()**

**2.2）isPrototypeOf()**

检测一个对象是否在另一个对象的原型链上

```js
X.isPrototypeOf(obj)
```

参数：

obj：在该对象的原型链上查找 X

示例：

```js
class Animal { }
class Cat extends Animal { }
let cat = new Cat()

// 原型链 prototype chain
// cat → Cat.prototype → Animal.prototype

console.log(Cat.prototype.isPrototypeOf(cat));              // true
console.log(Animal.prototype.isPrototypeOf(cat));           // true
console.log(Animal.prototype.isPrototypeOf(Cat.prototype)); // true
```



**2.3）propertyIsEnumerable()**

**2.4）toLocaleString()**

**2.5）toString()**

**2.6）valueOf()**