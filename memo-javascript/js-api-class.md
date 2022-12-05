# JS Class API

## 一、类的语法

```js
class Foo {
    #field = "classFoo" // 私有属性
    #method() { }       // 私有方法
    publicProperty = "foo's public property"
    static isFoo(obj) { // 静态方法。this 指向类 Foo
        return #field in obj
    }
    isFoo() {           // 实例方法。this 指向实例
        return #field in this
    }
    getField() {
        console.log(this.#field);
    }
    getMethodName() {
        console.log(this.#method.name); // 获取私有方法的名字
    }
}

let f = new Foo()
console.log(f);
// console.log(f.isFoo());    // true
// console.log(Foo.isFoo(f)); // true
```

### 1.1 `classFoo.prototype`

class 语法声明的类 Foo，它的 prototype 属性，**默认不能赋值、不能枚举、不能修改属性描述符。**

```js
{
    configurable: false
    enumerable: false
    value: { constructor: Foo }
    writable: false
}
```

因此，`classFoo.prototype = Object.create(SuperClass.prototype)`是无效的。

### 1.2 `super()` 报错

在子类中错误调用 `super()` 产生报错

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

子类可以不写`constructor`，一旦写了`constructor`，要么在其中调用 `super()`，要么返回一个对象，否则报错。

## 

## 二、类的继承

### 例 1：`extends`语法

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



### 例 2：用`Class`实现类的继承，子类 `constructor` 中必须调用 `super()`

如果想用 `Class` 实现类的继承，子类要么不写 `constructor`；要么写了 `constructor`，然后必须在其中调用 `super()`。

---

![JS继承中实例的原型链](https://upload-images.jianshu.io/upload_images/1231311-de565e9734417325.png)

---

2.1）如果子类不写 `constructor`，JS 引擎会自动补全：

```js
// 如果子类不写 constructor，JS 引擎会自动写
class Animal {
    constructor(name) {
        this.name = name || 'animal'
    }
    sayName() {
        console.log(this.name)
    }
}

class Cat extends Animal {
    jump() {
        console.log('cat jump');
    }
}

let cat = new Cat()
console.log(cat);
cat.sayName() // 'animal'
cat.jump() // 'cat jump'
```

2.2）子类一旦写了 `constructor`，那么必须在子类的构造函数中调用 `super()`：

```js
class Animal {
    constructor(name) {
        this.name = name || 'animal'
    }
    sayName() {
        console.log(this.name)
    }
}

class Cat extends Animal {
    // 子类一旦写了 constructor，那么必须在其中调用 super()
    constructor() {
        super('cat')
        this.canFly = false
        this.canMeow = true
    }
    jump() {
        console.log('cat jump');
    }
}

let cat = new Cat()
console.log(cat)
cat.sayName() // 'cat'
cat.jump() // 'cat jump'
```

2.3）更复杂些的例子：子类的构造器需要传参，而且参数数量不确定。

```js
class Animals extends Array {
    constructor() {
        // 子类写了 constructor，应该用扩展操作符给 super() 传参
        // super.apply(this, arguments) //Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        super(...arguments)
        this.canBreathe = true
        this.canDrive = false
    }
    jump() {
        console.log('we cat jump');
    }
}

let animals = new Animals('duck', 'rabbit', 'chicken', 'fish')
console.log(animals);
animals.jump()
```
