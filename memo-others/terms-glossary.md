## Call Stack

```js
function bar() {
    debugger // Paused in debugger
    console.log(this); // window
}
function foo() {
    bar() // 等价于 window.bar() 所以 bar 中的 this 是 window
}
foo()
```

调用栈信息：

![image-20221120095753690](https://wx4.sinaimg.cn/large/6cdfff77gy1h8bczky4p2j208m03vq3c.jpg)

- 函数 foo 叫做 bar 的「调用者」（caller），**即 foo 中调用了 bar**。
- 函数 bar 叫做 foo 的「被调用者」（callee），**即 bar 在 foo 中被调用**。
- 【注意】bar 的 this 指向 window 并不指向 foo。

