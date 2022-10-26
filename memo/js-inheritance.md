## 术语

1） 有一个对象 obj，`obj.__proto__`叫做「obj 对象的原型」，简称**「obj 的原型」**。

2） `obj.__proto__`所指向的对象叫做「**obj 的原型对象**」，也就是 obj 的构造函数的 prototype 属性所指向的对象。

## 继承

对象的继承。A 对象通过继承 B 对象，就能直接拥有 B 对象的所有属性和方法。

## 基于原型的继承

`子类.prototype.__proto__ = 父类.prototype`

`子类.prototype` 是个对象，它的原型指向`父类.prototype`。

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
// Cat.prototype = Object.create(Animal.prototype)
// Cat.prototype.constructor = Cat
// 或者
Object.setPrototypeOf(Cat.prototype, Animal.prototype) // 不必设置 Cat.prototype.constructor = Cat
Cat.prototype.jump = function () {
    console.log('cat jump');
}

let cat = new Cat(1)
cat.sayName() // 'cat'
cat.sayAge() // 1
cat.jump() // 'cat jump'
```

## 基于 class 的继承

通过 extends 关键字实现继承。

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
        super('cat', age)
    }
    jump() {
        console.log('cat jump');
    }
}

let cat = new Cat(1)
cat.sayName() // 'cat'
cat.sayAge() // 1
cat.jump() // 'cat jump'
```

## 