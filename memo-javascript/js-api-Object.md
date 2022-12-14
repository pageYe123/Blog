阅读资料：https://www.jianshu.com/p/c5aa1eee8dfd

## 一、`Object` 静态方法

方法位于`Object`上。

```js
console.dir(Object)
```



### 1.1）Object.assign()

方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。拷贝的属性是有限制的，只拷贝源对象的自身属性，不拷贝继承属性，也不拷贝不可枚举属性（enumerable: false）

语法：

```js
Object.assign( target, source, source1 ) 
```

示例：

```js
let target = { a: 1, b: 1 };
let source1 = { b: 2, c: 2 };
let source2 = { c: 3 };

Object.assign(target, source1, source2); // 属性会被后续参数中的相同属性覆盖。
target // {a:1, b:2, c:3}
```



### 1.2）Object.create()

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



### 1.3）Object.defineProperties()

### 1.4）Object.defineProperty()

定义对象的属性

语法：

```js
Object.defineProperty(obj, prop, descriptor)
// 第一个参数是对象
// 第二个参数是属性名，字符串类型
// 第三个参数是属性描述符
// 注意：属性描述符中（value writable）和（get set）二选一。
```

示例

```js
let obj = {};

// 1. 添加一个属性
Object.defineProperty(obj, "newDataProperty", {
    value: 101,
    writable: true,
    enumerable: true,
    configurable: true
});

obj.newDataProperty    // 101

// 2.修改属性的属性描述符
// 注意：如果 configurable 为 false，则修改属性描述符会失败
Object.defineProperty(obj, "newDataProperty", {
    writable:false
});

// 3. 添加 getter/setter 访问器属性
Object.defineProperty(obj, "newAccessorProperty", {
    set: function (x) {
        // 只能赋值给其他属性，不能再赋值给 newAccessorProperty，否则死循环
        this.otherProperty = x;
    },
    get: function () {
        return this.otherProperty;
    },
    enumerable: true,
    configurable: true
});

obj.newAccessorProperty = 1;
console.log(obj);
```



### 1.5）Object.entries()

### 1.6）Object.freeze()

### 1.7）Object.fromEntries()

### 1.8）Object.getOwnPropertyDescriptor()

### 1.9）Object.getOwnPropertyDescriptors()

### 1.10）Object.getOwnPropertyNames()

### 1.11）Object.getOwnPropertySymbols()

### 1.12）Object.hasOwn()

作用：检测对象是否有某个属性名

语法：

```
Object.hasOwn(obj, propName)
```

注意：

官方推荐使用 `Object.hasOwn()` 代替 `Object.prototype.hasOwnProperty()`

- 它可以用于那些重新实现 hasOwnProperty() 的对象
    ```JavaScript
    const foo = {
      hasOwnProperty() { // 重新实现 hasOwnProperty() 并不影响 Object
        return false;
      },
      bar: 'bar',
    };
    
    if (Object.hasOwn(foo, 'bar')) {
      console.log(foo.bar); // bar
    }
    ```

- 它也可以用来测试用`Object.create(null)`创建的对象。这些对象没有继承自`Object.prototype`，也就无法访问`hasOwnProperty()`。
    ```js
    const foo = Object.create(null);
    foo.prop = 'exists';
    if (Object.hasOwn(foo, 'prop')) {
      console.log(foo.prop); // 'exists'
    }
    ```

    

    

### 1.13）Object.is()

### 1.14）Object.isExtensible()

### 1.15）Object.isFrozen()

### 1.16）Object.isSealed()

### 1.17）Object.keys()

### 1.18）Object.preventExtensions()

### 1.19）Object.seal()

### 1.20）Object.setPrototypeOf()

将一个指定的对象的原型设置为另一个对象

```js
class Animal { }
class Cat { }
Object.setPrototypeOf(Cat.prototype, Animal.prototype); // 等价于 B extends A

Animal.prototype.isPrototypeOf(Cat.prototype) // true
```



### 1.21）Object.values()

## 二、`Object` 实例方法

方法位于`Object.prototype`上。

```js
console.dir(Object.prototype)
```

### 2.1）hasOwnProperty()

语法：

```js
obj.hasOwnProperty(propName)
```

示例：

```js
let o = {a: 1 }

o.hasOwnProperty('a')         // true
o.hasOwnProperty('b')         // false 对象自身没有属性 b
o.hasOwnProperty('toString'); // false 不能检测对象原型链上的属性
```

官方推荐使用 [`Object.hasOwn()`](#1.12）Object.hasOwn()) 代替 `Object.prototype.hasOwnProperty()`

### 2.2）isPrototypeOf()

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



### 2.3）propertyIsEnumerable()

### 2.4）toLocaleString()

### 2.5）toString()

### 2.6）valueOf()