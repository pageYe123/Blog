【待梳理成API】......
如果讲解过多就单独成立 blog





Promise 是目前前端解决异步问题的统一方案。使用 Promise 对象之后可以使用一种链式调用的方式来组织代码，让代码更加的直观。也就是说，有了 Promise 对象，就可以将异步操作以同步的操作的流程表达出来，避免「回调地狱」。

Promise 有三种状态：
- pending
- fulfilled
- rejected

## 如何创建一个 new Promise

```js
let promise = new Promise((resolve, reject)=>{
    /* 将 promise 对象的状态变为 fulfilled，并执行 then(success) */
    resolve(data) // data 可以是任何类型
    /* 将 promise 对象的状态变为 rejected，并执行 then(fail) */
    reject(reason) // reason 可以是任何类型
})
```
实际代码：
```js
let promiseFactory = (arg) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => { // 模拟异步操作
            const res = Math.random()
            if (res >= 0.5) {
                resolve(`参数 ${arg} 成功`)
            } else {
                reject(`参数 ${arg} 失败`)
            }
        }, 1000)
    })
}

// 使用示例
let success = (response) => {
    console.log(response)
}
let fail = (response) => {
    console.log(response)
}
promiseFactory('foo').then(success, fail)
```

## Promise.prototype.then
它的作用是为 Promise 实例添加状态改变时的回调函数，一旦状态改变，就执行相应的回调。`.then`方法的第一个参数是 resolved 状态的回调函数，第二个参数是 rejected 状态的回调函数，它们都是可选的。

```js
let promise = fn() // 得到 promise 对象
promise.then(success1, fail1).then(success2, fail2).catch(fail3)
```

## Promise.all

`.all`方法可以接受一个「元素为 Promise 对象的 iterable 类型」（ES6 中 Array，Map，Set 都属于其中），并只返回一个`Promise`实例。

这个`Promise`实例的`resolve`回调执行方式：在所有输入的`Promise`的`resolve`回调都结束，或者输入的 iterable 里没有 `Promise` 的时候。「resolved 状态的回调函数」中传入的参数是一个数组。

这个`Promise`实例的`reject`回调执行方式：只要任何一个输入的`Promise`的 `reject`回调执行或者输入不合法的`Promise`就会立即抛出错误，并且`reject`的是第一个抛出的错误信息。

```js
let p1 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("第一个promise");
    }, 3000);
});

let p2 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("第二个promise");
    }, 1000);
});

Promise.all([p1, p2]).then(function (result) {
    console.log(result); // 3秒后输出["第一个promise", "第二个promise"]
});
```

## Promise.race

race 的意思为赛跑。promise.race 接收一个数组作为参数，但是与 promise.all 不同的是，race 只返回跑的快的值，也就是执行比较快的那个。一旦数组中的某个 promise 解决或拒绝，返回的 promise 就会解决或拒绝。

```js
let p1 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("第一个promise");
    }, 3000);
});

let p2 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("第二个promise");
    }, 1000);
});

Promise.race([p1, p2]).then(function (result) {
    console.log(result); // 1秒后输出 "第二个promise"
});
```

## 使用 Promise 解决回调地狱

下面的例子是三个小方块，小方块1向右移动 100px 之后停止运动，小方块2开始向右移动。等到小方块2向右移动 200px 之后停止运动，小方块3开始向右移动直至 300px 时停下。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Promise 解决回调地狱</title>
</head>

<body>
    <style>
        ul,
        ol {
            list-style-type: none;
        }

        .wrapper .item {
            width: 50px;
            height: 50px;
            border: 1px solid red;
        }
    </style>
    <ul class="wrapper">
        <li class="item b1"></li>
        <li class="item b2"></li>
        <li class="item b3"></li>
    </ul>
    <script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ="
        crossorigin="anonymous"></script>
    <script type="text/javascript">
        function moveTo(obj, left) {
            const STRIDE = 2
            const FREQUENCY = 10
            // 创建一个promise实例，并传入两个函数参数，来设置promise的状态
            return new Promise(function (resolve, reject) {
                let leftPos = parseInt(obj.css('margin-left'));
                if (leftPos !== left) {
                    let timer = setInterval(() => {
                        leftPos += STRIDE;
                        obj.css('margin-left', leftPos);

                        if (leftPos === left) {
                            clearInterval(timer)
                            resolve()   // 成功运动完成之后，返回一个状态resolved，then接收到后继续执行下一个回调
                        }
                    }, FREQUENCY)
                } else {
                    resolve()
                }
            })
        }
        // 采用promise链式操作的方式 通过传递状态的方式来调用回调函数
        moveTo($('.b1'), 100)
            .then(function () {                 // 当b1成功运动完成后，链式调用b2
                return moveTo($('.b2'), 200)    // then函数返回的是一个新的promise对象，继续then
            })
            .then(function () {                // 当b2运动完成并且成功之后 链式调用b3
                return moveTo($('.b3'), 300)
            })
    </script>
</body>

</html>
```

如果不用 Promise 看看回调地狱长什么样？

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>回调地狱</title>
</head>

<body>
    <style>
        ul,
        ol {
            list-style-type: none;
        }

        .wrapper .item {
            width: 50px;
            height: 50px;
            border: 1px solid red;
        }
    </style>
    <ul class="wrapper">
        <li class="item b1"></li>
        <li class="item b2"></li>
        <li class="item b3"></li>
        <li class="item b4"></li>
        <li class="item b5"></li>
        <li class="item b6"></li>
    </ul>
    <script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ="
        crossorigin="anonymous"></script>
    <script type="text/javascript">
        function moveTo(obj, left, cb) {
            const STRIDE = 2
            const FREQUENCY = 10
            let leftPos = parseInt(obj.css('margin-left'));
            if (leftPos !== left) {
                let timer = setInterval(() => {
                    leftPos += STRIDE;
                    obj.css('margin-left', leftPos);

                    if (leftPos === left) {
                        clearInterval(timer)
                        cb()
                    }
                }, FREQUENCY)
            }
        }

        // 回调地狱
        moveTo($('.b1'), 100, () => {
            moveTo($('.b2'), 200, () => {
                moveTo($('.b3'), 300, () => {
                    moveTo($('.b4'), 400, () => {
                        moveTo($('.b5'), 500, () => {
                            moveTo($('.b6'), 600, () => {

                            })
                        })
                    })
                })
            })
        })
    </script>
</body>

</html>
```

