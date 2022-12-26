# JS 代码重构经验

## 一、重构是什么？

对软件内部结构的一种调整，目的是在不改变软件系统外部行为的前提下，提高代码可读性，降低维护成本。

| 对比       | Code Refactoring                                                                            | Code Optimization                                                                                                              |
| ---------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| 概念       | 代码重构：在内部结构上修改代码，且不影响软件外部行为。                                      | 代码（性能）优化：在性能方面修改代码，且不影响软件外部行为。                                                                   |
| 什么时候做 | ①代码不易理解<br />②代码不易维护。很难去修改，或者低效率的重复改动很多。                    | 出现性能问题时                                                                                                                 |
| 效益       | 代码易读性提高，易于维护。                                                                  | 优化算法，降低时间复杂度。程序运行得更快，性能更好                                                                             |
| 编程思想   | While code refactoring, *design principles* are more useful like *SOLID, KISS, DRY, YAGNI*. | Use advance/generic concepts like design patterns, Async Programming etc.<br />使用先进的/通用的概念，如设计模式、异步编程等。 |



## 二、重构角度

在写完一个代码文件后，可以从以下角度进行重构。

- 简化代码，删除「注释掉的代码」（[commented out code](https://codeql.github.com/codeql-query-help/cpp/cpp-commented-out-code/#commented-out-code)），尽量删除“无用”的代码。说它“无用”是需要主观判断的，比如这个工具函数当前已经不用了，并且未来也不太可能用，就要删掉。

- 逻辑优化，属性归谁管就挂载在哪个对象上，归属的逻辑要正确。比如`$hijack`上的`websiteScreen` 、`pauseVideo`等方法应该挂载在`$eventsHandler`上。

- 逻辑优化，减少没有必要的重复执行的部分，本质是解耦模块间的依赖。比如执行函数 a 过程中调用了 b 函数，b 函数里面执行一个 c 函数，但是接下来 a 过程又要调用 c 函数。给 A 和 C 解耦，可以让 c 函数只调用一次，提高代码性能。
![image.png](https://cdn.jsdelivr.net/gh/yeshiqing/imgHosting/blog-pictures/20221223170920.png)

- 统一变量、函数的命名规则。Follow the similar naming conventions throughout the project.

- 有些函数很臃肿，需要将大段的代码拆分成一个个小巧的功能函数。抽成函数并不是为了应对重复，而是为了拆分大段代码，使其便于阅读。

- 预测/预警重复，提前做「事不过三」的抽象，抽象出函数、类，以及使用「继承」。

- 意大利面条式代码作为属性，挂载在一个对象上，实际上是对象的思想（「模块化」思想）。与其说这个对象有这样的属性，我更愿意理解为这些属性归于这个对象来管理。
    另外，有很多全局变量，作为配置项，也可以挂载在一个叫 config 的对象上，都归 config 对象管。而且编辑器中还方便折叠代码，把一长串配置项折叠起来。

    

## 三、抽象思维

### 1）事不过三

- 同样的代码写三遍，就应该抽成一个函数
- 同样的属性写三遍，就应该做成**共用属性（抽出原型或类）**
- 同样的原型写三遍，就应该用**继承**；
    不同类有同样的方法，就应该让不同类**继承**自同一个父类

代价：
有时候会造成继承层级太深，无法一下看懂代码，可以通过写文档、画类图解决。

示例：

#### 1.1）用哈希表或数组替换 if 条件语句

```js
var video_status = 'loadedmetadata'

function foo(a) {
    if (a === 'loadedmetadata') {
        video_status = 'loadedmetadata'
    } else if (a === 'play') {
        video_status = 'play'
    } else if (a === 'playing') {
        video_status = 'pause'
    } else if (a === 'ended') {
        video_status = 'pause'
    }
}

function foo2(a) {
    const VIDEO_EVENTNAME = ['playing', 'play', 'waiting', 'pause', 'ended', 'loadedmetadata']
    if (VIDEO_EVENTNAME.includes(a)) {
        video_status = a
    }
}
```

#### 1.2）用哈希表替换 switch 语句

以随机选择东南西北为例，看看`switch`语句和`hashMap`实现的差异。

switch 语句

```javascript
let foo = Math.floor(Math.random()*4); // 随机产生一个 1 到 4 的整数
let output = 'Output: ';
switch (foo) {
  case 0:
    output += 'east';
  case 1:
    output += 'south';
  case 2:
    output += 'west';
  case 3:
    output += 'north';
  default:
    console.log('Please pick a number from 0 to 3!');
}
```

更换为 hashMap

```javascript
let foo = Math.floor(Math.random() * 4);
let output = 'Output: ';
// 对象的键名如果是数值，会被自动转为字符串
let hashMap = {
    0: 'east',
    1: 'south',
    2: 'west',
    3: 'north',
    default: 'Please pick a number from 0 to 3!'
}
// 虽然传入键名 foo 是数字，但会被自动转换为字符串
output += hashMap[foo || default]
```

总结：用 hashMap 代替 switch 语句

- 优点：少写了很多`case`和`output`
- 缺点：

  - `default:`需要用短路逻辑实现，~~代码不是很好读，对新手来说不是很好理解~~。

#### 1.3）将共用属性抽象到类的 prototype 属性上

抽象出构造函数（类）要比抽象成工厂函数好，因为类的实例可以使用`类.prototype`上的共有方法，节省内存；工厂函数生成的每个对象所绑定的方法都是一个新函数，浪费内存。
另外这二者在设计思想上有差异，面向对象程序设计的编程范式提供了四大特性、五大原则。

四大特性：

- 封装性 Encapsulation：对象属性是隐藏的，对象属性修改需要通过对象方法。
- 继承性 Inheritance：子类可以把父类的属性和方法都继承过来，无需重新定义。
- 多态性 Polymorphism：多态分为静态和动态，静态是指同一个对象可以有不同的表现形式，动态指一个父类型可以指向其子类型的实例，使子类型对同一方法作出不同的回应。
- 抽象性 Abstraction：抽象指把一类东西的共同属性和行为提取出来存在一个类里面，而不关注具体行为如何实现。

五大原则：

- 单一职责原则 SRP：一个类功能要单一，只实现一种功能。
- 开放封闭原则 OCP：一个类、方法或模块的扩展性要保持开放，可扩展但不影响源代码。
- 替换原则 LSP：父类出现过的地方，都可以用子类代替。
- 接口分离原则 ISP：一个类对另一个类应该用最小的接口来耦合。
- 依赖倒置原则 DIP：依赖抽象编程。把抽象类当成一种原型，所有具体类都按该原型拓展，下层模型依赖上层模型实现。

```js
// 事不过三原则，同样的属性写三遍，就应该做成共用属性（抽出原型或类）
// let obj1 = { name: Bob, age: 17 }
// let obj2 = { name: Tom, age: 20 }
// let obj3 = { name: Sam, age: 33 }


// 方法 1：抽象出构造函数（类）
function Person({ name, age }) {
    this.name = name
    this.age = age
}
Person.prototype.printName = function() {
    console.log(this.name)
}

let obj1 = new Person({ name: 'Bob', age: 17 })
let obj2 = new Person({ name: 'Tom', age: 20 })
let obj3 = new Person({ name: 'Sam', age: 33 })

// 方法 2：抽象成工厂函数
// function factory({ name, age }) {
//     let obj = {
//         name: name,
//         age: age
//     }
//     obj.printName = function () {
//         console.log(name)
//     }
//     return obj
// }
// let obj1 = factory({ name: 'Bob', age: 17 })
// let obj2 = factory({ name: 'Tom', age: 20 })
// let obj3 = factory({ name: 'Sam', age: 33 })
```



### 2）表驱动编程

「表」指的是数据结构，如哈希表、数组等。

表驱动法就是一种编程模式(scheme)——从表里面查找信息而不使用逻辑语句(if 和 case)。对简单的情况而言，使用逻辑语句更为容易和直白。但**随着逻辑链的越来越复杂，查表法会更具吸引力**。

```js
// 表驱动编程示例
function translate(number) {
  let numberList = {
  '1': '一',
  '2': '二',
  '3': '三'
  }
  return numberList[number];
}
```

表驱动编程可以减少重复代码，只将重要的信息放在表里，然后利用表来编程。当你看到大批类似但不重复的代码，看看哪些是不同的数据，把这些重要数据做成哈希表，代码就简单了。

示例：在`autobindEvents`函数中根据`events`哈希表自动绑定事件。

![示例](https://cdn.nlark.com/yuque/0/2022/png/29373291/1667298874991-a70373a4-1503-4b9a-80bc-afada89d65fb.png)

### 3) if...else 的简化

**3.1）嵌套条件通过取反早 return**
三层以上的嵌套条件，并且代码经常变动，或者极大可能会发生代码膨胀，就要提早 return。

![image-20221223173332987](https://cdn.jsdelivr.net/gh/yeshiqing/imgHosting@master/uPic/20221223173334.png)



## 四、将超大函数拆分成小函数，一次只做一件事

在 Steve McConnell 编写的《代码大全》（*Code Complete*）有如下说明：

> From time to time, a complex algorithm will lead to a longer routine, and in those circumstances, the routine should be allowed to grow organically up to 100-200 lines. (A line is a noncomment, nonblank line of source code.) Decades of evidence say that routines of such length are no more error prone than shorter routines. Let issues such as depth of nesting, number of variables, and other complexity-related considerations dictate the length of the routine rather than imposing a length restriction per se.
>
> If you want to write routines longer than about 200 lines, be careful. None of the studies that reported decreased cost, decreased error rates, or both with larger routines distinguished among sizes larger than 200 lines, and you’re bound to run into an upper limit of understandability as you pass 200 lines of code.

翻译：

有时，**一个复杂的算法会导致一个较长的程序，在这种情况下，应该允许该程序有机地增长到 100-200 行**。(几十年来的证据表明，这种长度的程序并不比短的程序更容易出错。让诸如嵌套的深度、变量的数量以及其他与复杂性有关的考虑因素来决定例程的长度，而不是对其本身施加长度限制。

如果你想写长于 200 行的函数，要小心。没有任何一项研究报告指出成本降低，错误率降低，或者两者都有，这些研究对大于 200 行的函数进行了区分，**当你超过 200 行代码的时候，你一定会遇到一个可理解性的上限。**



进一步阅读：

- https://stackoverflow.com/questions/475675/when-is-a-function-too-long

## 五、代码简写技巧

```js
// 1. 逻辑或赋值运算 x||=y，仅在 x 为 falsy 值时赋值
// 七个 falsy 值：false、NaN、null、undefined、''、0、document.all
let a=1, b=2;
a = a || b;
// 等价于
a ||= b // ES 2021 引入的语法，低版本 node.js 环境不支持


// 2. 可选链运算符
// 允许读取位于连接对象链深处的属性值，而不必明确验证链中的每个引用是否有效。
let obj = {
    first: {
        second: 'prop'
    }
}
let nestedProp = obj && obj.first && obj.first.second;
// 等价于
let nestedProp = obj?.first?.second;


// 3. 短路逻辑
// 且运算符，读作“and and”
A && B && C && D // 取第一个假值或 D
window && window.fn && window.fn() // 适合读对象属性时，进行保护
// 等价于 
window?.fn?.()

// 或运算符
A || B || C || D // 取第一个真值或 D
A = A || B // 或运算符为变量设置默认值，B 称作“保底值”
// 等价于
A ||= B

// 4. 三元运算符
a = f1(a) ? a : f2(a)
// 等价于
f1(a) || (a = f2(a))
```

## 六、易读性

三元运算符（*Conditional/ternary operator*）最好写成一行。如果不得不写成多行，要进行格式化：

```js
// 代码风格一，可以被 switch...case 语句代替
let result = (foo == bar)  ? result1 :
             (foo == baz)  ? result2 :
             (foo == qux)  ? result3 :
             (foo == quux) ? result4 : 
                             fail_result;

// 代码风格二。condition 非常长，有很多行。这样换行有助于分清 if 部分和 else 部分
let variable = (condition) 
    ? fn()
    : $variable;
```

