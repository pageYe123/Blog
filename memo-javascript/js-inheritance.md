## 术语

1） 有一个对象 obj，`obj.__proto__`叫做「obj 对象的原型」，简称**「obj 的原型」**。

2） `obj.__proto__`所指向的对象叫做「**obj 的原型对象**」，也就是 obj 的构造函数的 prototype 属性所指向的对象。

## 继承

对象的继承。A 对象通过继承 B 对象，就能直接拥有 B 对象的所有属性和方法。

![JS继承中实例的原型链](https://upload-images.jianshu.io/upload_images/1231311-de565e9734417325.png)

问：子类 A 的实例 a 怎么去找方法（属性）？

答：实例 a 先从自身找方法。没有，就去自身的「原型」，也就是`构造函数.prototype`，上找方法。`构造函数.prototype`上没有，就去「原型」的原型，也就是`构造函数.prototype.__proto__`，也就是`父类.prototype`上找方法。直到原型链的顶端。

## 一、基于原型的继承

1.1）不同于基于`class`的继承，基于原型的继承，子类的原型指向`Function.prototype`。

1.2）`子类.prototype.__proto__ = 父类.prototype`

实践中正规写法：

`子类.prototype = Object.create(父类.prototype)`

`子类.prototype` 是个对象，它的原型指向 `父类.prototype`。

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
(Cat.prototype = Object.create(Animal.prototype)) && (Cat.prototype.constructor = Cat)
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

## 二、基于 class 的继承

2.1）通过 extends 关键字实现继承。

2.2）不同于基于原型的继承，基于`class`的继承，子类的原型指向父类。

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

### 2.1）在子类中错误使用 `super` 产生报错

**错误用法一：**

```javascript
console.log(super)
```

> 'super' can only be used with function calls (i.e. super()) or in property accesses (i.e. super.prop or super[prop])

`super`调用只有两种形式：

- `super()` 。可以接受全部参数：`super(...arguments)`
- `super.prop`（或 `super[prop]`）

**错误用法二：**

 ```javascript
 super.apply(this, arguments)
 ```

> Must call super constructor in derived class before accessing 'this' or returning from derived constructor

子类中不调用 `super()`，也不返回对象，也报这个错。子类中要么调用 `super()`，要么返回一个对象，否则报错。

## 三、继承语法对比

|                 对比                  |                        基于原型的继承                        |                      基于 class 的继承                       |
| :-----------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|                 语法                  | ![基于原型的继承](https://upload-images.jianshu.io/upload_images/1231311-4521d78f0f1bc7d2.png) | ![基于 class 的继承](https://upload-images.jianshu.io/upload_images/1231311-84e4edf4c9f32477.png) |
|                   -                   |                              -                               |                              -                               |
| 子类的`__proto__`<br />（子类的原型） | Function.prototype<br />（`B.__proto__ === Function.prototype`） |              父类<br />（`B.__proto__ === A`）               |

