# JS 面向对象编程 API

## 一、术语

### 1.1）原型与原型对象

- 有一个对象 obj，`obj.__proto__`叫做「obj 对象的原型」（instance's prototype），简称**「obj 的原型」**。

- `obj.__proto__`所指向的对象叫做「**obj 的原型对象**」，也就是 obj 的构造函数的 prototype 属性所指向的对象。

```js
function A(){}
let a = new A()
console.log(a.__proto__ === A.prototype) // true

a.__proto__ // a 对象的原型
a.__proto__ = A.prototype // 将 a 对象的原型指向 A.prototype，由 V8 自动完成
A.prototype // 此时，叫做 a 对象的原型对象
```

### 1.2）new 操作符的四步骤

```js
function A(){}
let a = new A()
// 等价于
// 1. 创建一个空对象 obj={}
// 2. obj.__proto__ = A.prototype
// 3. A.call(obj)
// 4. return obj 或其他对象
```

## 二、对象的继承

### 2.1）对象的继承

B 对象通过继承 A 对象，就能直接拥有 A 对象的所有属性和方法。

![什么是继承](https://upload-images.jianshu.io/upload_images/1231311-7f8bb1046a2588f6.png)

### 2.2）类的继承——图解

![JS继承中实例的原型链](https://upload-images.jianshu.io/upload_images/1231311-de565e9734417325.png)

### 2.3）类的继承的两种实现方式

**2.3.1）继承语法对比**

| 对比                                  |                        基于原型的继承                        |                      基于 class 的继承                       |
| :------------------------------------ | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 语法                                  | ![基于原型的继承](https://upload-images.jianshu.io/upload_images/1231311-4521d78f0f1bc7d2.png) | ![基于 class 的继承](https://upload-images.jianshu.io/upload_images/1231311-84e4edf4c9f32477.png) |
| -                                     |                              -                               |                              -                               |
| 子类的`__proto__`<br />（子类的原型） | Function.prototype<br />（`B.__proto__ === Function.prototype`） |              父类<br />（`B.__proto__ === A`）               |



**2.3.2）基于原型的继承**

不同于基于`class`的继承，基于原型的继承，子类的原型指向`Function.prototype`。

```js
function Animal(name, age) {
    this.name = name
    this.age = age
}
Animal.prototype.sayName = function () {
    console.log(this.name);
}
Animal.prototype.sayAge = function () {
    console.log(this.age);
}

function Cat(age) {
    Animal.call(this, 'cat', age)
}
Cat.prototype = Object.create(Animal.prototype)
Cat.prototype.constructor = Cat
// 等价于 Cat.prototype = Object.create(Animal.prototype, { constructor: { value: Cat } })
// 等价于 Object.setPrototypeOf(Cat.prototype, Animal.prototype) // 不必设置 Cat.prototype.constructor = Cat
// 等价于 Cat.prototype.__proto__ = Animal.prototype

Cat.prototype.jump = function () {
    console.log('cat jump');
}

let cat = new Cat(1)
console.log(cat);
cat.sayName() // 'cat'
cat.sayAge()  // 1
cat.jump()    // 'cat jump'
```



**2.3.3）基于 class extends 的继承**

不同于基于原型的继承，基于`class`的继承，子类的原型指向父类。

```js
class Animal {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    sayName() {
        console.log(this.name)
    }
    sayAge() {
        console.log(this.age)
    }
}

class Cat extends Animal {
    constructor(age) {
        // 想用 Class 实现类的继承，那么必须在子类的构造函数中使用 super()
        super('cat', age) // 或者 super('cat', ...arguments)
    }
    jump() {
        console.log('cat jump');
    }
}

let cat = new Cat(1)
cat.sayName() // 'cat'
cat.sayAge()  // 1
cat.jump()    // 'cat jump'
```



#### 2.4）多重继承

子类`S`同时继承了父类`M1`和`M2`。如果 `M2.prototype` 有 `M1.prototype` 的同名方法，会覆盖它。

```js
function M1() {
    this.hello = 'hello';
}

function M2() {
    this.world = 'world';
}

function S() {
    M1.call(this);
    M2.call(this);
}

// 继承 M1
S.prototype = Object.create(M1.prototype);
// 继承链上加入 M2
Object.assign(S.prototype, M2.prototype);
// 指定构造函数
S.prototype.constructor = S;

var s = new S();
// console.log(s.hello); // 'hello'
// console.log(s.world); // 'world'
```



## 三、与原型相关的方法

### 3.1）Object.prototype.isPrototypeOf()

| 对比 | **X instanceof Y**              | **Y.isPrototypeOf(X)**                                       |
| ---- | ------------------------------- | ------------------------------------------------------------ |
| 定义 | Y.prototype 是否在 X 的原型链上 | Y 是否在 X 的原型链上                                        |
| 作用 | 检测对象是否为类的实例          | 一般通过检测「原型对象」是否在另一个「原型对象」的原型链上，来确定类的继承关系。 |
| 代码 | `cat instanceOf Animal`         | `Animal.prototype.isPrototypeOf(Cat.prototype)`              |

```js
X instanceof Y // Y.prototype 是否在 X 的原型链上
Y.isPrototypeOf(X) // Y 是否在 X 的原型链上

// 原型链 prototype chain
// cat → Cat.prototype → Animal.prototype → Object.prototype

cat instanceof Cat                          // true
cat instanceof Animal                       // true
cat instanceof Object                       // true

Animal.prototype.isPrototype(Cat.prototype) // true 这个经常用，判断类的继承关系
Cat.prototype.isPrototypeOf(cat)            // true 以下不常用
Animal.prototype.isPortotypeOf(cat)         // true 
Object.prototype.isPrototypeOf(cat)         // true 
```

应用：判断两个类是否是继承关系

```js
class A { }
class B extends A { }
class C extends B { }

// 判断类 X 是否继承 Y
function isInherit(X, Y) {
    return Y.prototype.isPrototypeOf(X.prototype)
}
console.log(isInherit(C, A)); // true
```



### 3.2）Object.create(xPrototype)

以 xPrototype 为原型，创建一个新对象。

```js
function Aminal(){}
function Cat(){}
Cat.prototype = Object.create(Animal.prototype)
Cat.prototype.constructor = Cat
```

### 3.3）Object.setPrototypeOf(B.prototype, A.prototype)

定义：设置`B.prototype`的原型指向`A.prototype`

应用：实现类的父子继承关系

```js
class A {}
class B {}

// 类 B 继承类 A
class B extends A{}
// 等价于 Object.setPrototypeOf(B.prototype, A.prototype);
// 等价于 B.prototype.__proto__ = A.prototype
// 不等价于 B.prototype = Object.create(A.prototype)，因为 class 声明的类的prototype 属性不能赋值。如果换成 function 声明的类就可以。

A.prototype.isPrototypeOf(B.prototype) // true

```

### 3.4）Object.getPrototypeOf(B)

定义：获取 B 对象的原型，即`B.__proto__`

应用：判断两个类是否是父子继承关系

```js
function A() { }
function B() { }

B.prototype = Object.create(A.prototype)
B.prototype.constructor = B

let b = new B()

// 判断类 Y 是否是 X 的父类
function isParent(X, Y) {
    return Object.getPrototypeOf(X.prototype) === Y.prototype
}
console.log(isParent(B, A)); // true
```
