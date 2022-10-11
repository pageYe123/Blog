## 执行环境和作用域链

### 调用栈（call stack）

调用栈之所以叫 call stack 其实是很形象的。它就像个堆栈（堆栈就当做一个桶理解），往里面装啊装，然后取的时候，后进的在上面，所以后进先出。来看下面的代码。

```js
function a() {
  b();
}
function b() {
  console.log('hi');
}
a();
```



![image-20220828152714753](https://wx4.sinaimg.cn/large/6cdfff77gy1h5migegavsj20zj0afwo2.jpg)

Chrome DevTools 中通过 debugger 可以在 Sources 面板中同时看到 scope（作用域链）和 call stack（函数调用栈）。scope 中由上往下，就是查找变量时作用域链的搜索方向，最外层是全局作用域 Global。

![image-20220828153737279](https://wx4.sinaimg.cn/large/6cdfff77gy1h5mir6v4qwj20d109kjtf.jpg)

上图相当于 call stack 图的第三步——执行 b 函数。

调用栈的每一个栈帧（stack frame）中保存一个「执行环境」。

### 执行环境（execution context）

在 javascript 中，**执行环境**可以抽象的理解为一个 object，它由以下几个属性构成：

```
executionContext：{
    variable object：variables, functions, arguments,
    thisValue: context object,
    scope chain: variable object + all parents scopes（最顶级为 Global）
}
```

可以看出，每个执行环境的**「作用域链」**由**当前环境的变量对象**及**父级环境的作用域构成**。

参考资料：https://medium.com/itsems-frontend/javascript-execution-context-and-call-stack-e36e7f77152e

### 作用域链（scope chain）

作用域链，在解释器进入到一个执行环境时初始化完成，并将其分配给当前执行环境。

举个例子：

```js
var me = 'emma1'
a()

function a(){
  var me = 'emma2'
  b()
}

function b(){
  console.log(me)
}
```

打印结果是`emma1`

> 因为在 func b 运行的时候，JS 引擎替 func b 建立了他的函数执行环境。而在建立执行环境的时候，也替他建立了 this 和「外部环境的引用（Reference to Outer environment）。对 func b 来说，他自己没有 me 这个变量，所以他往外部参考环境找（根据书写的位置，查找上面一层的代码）。而他的外部环境是全域，不是 func a，所以 func b 取得的是全域的 emma1。

如上面所说，func b 查找变量时沿着一个作用域向外部的作用域，再向外部的作用域.....这些作用域顺序就形成了所谓的「作用域链」[^1]。

为什么 func b 的外部环境是全域环境而不是 func a ？因为 Javascript 在决定「外部环境的引用」的时候是以「词法环境 (lexical environment)」为准则的。

### 词法环境 (lexical environment)

> 「词法环境」是一种规范类型。它基于 ECMAScript 代码的词法嵌套结构，定义变量标识符与函数之间的关联。一个「词法环境」由一个环境记录和一个可能为空引用的外部词法环境引用组成。

在一个「执行环境」建立的时候，也会建立这个环境的「词法环境」。所以白话一点说，词法环境就是在记录环境信息：包括执行环境中变量标识符和函数们之间的关联，还有外部参考环境。<u>词法环境是在代码定义的时候决定的，跟代码在哪里调用没有关系。</u>

闭包就是由函数以及声明该函数的「词法环境」组合而成的。

### 词法作用域（lexical scope）

又称「静态作用域」（static scope）。<u>当我们对程序进行编译时，作用域就已经依据我们程序的内容决定好了。</u>

和「词法作用域」相对的是「动态作用域」（dynamic scope）。<u>JS 与大多数的语言多采用「静态作用域」</u>，「动态作用域」的语言则要直到程序执行到该位置才能决定作用域。

词法作用域有一个特性：函数内部可以访问外部变量，但外部无法访问函数内部变量。

变量的作用域就是根据「词法作用域」进行查找的。

## 总结

调用栈（call stack）的每一个栈帧（stack frame）中保存一个「执行环境」。换句话说，每个栈帧都有各自的作用域链（scope chain），每个栈帧中的函数可访问的变量对象是不同的。

## 阅读资料

[Medium：JS 作用域链](https://medium.com/itsems-frontend/javascript-scope-and-scope-chain-ca17a1068c96)

[掘金：JS 词法作用域](https://juejin.cn/post/6974746307973873672)

[VO、AO、执行环境和作用域链](https://www.cnblogs.com/lulin1/p/9712311.html)

[了解词法环境吗？它和闭包有什么联系？](https://segmentfault.com/a/1190000039854311)

## 脚注

[^1]: JS 内部维护了一个数组（Chrome 中函数有一个内部属性`[[Scopes]]`）来存放这条作用域链。