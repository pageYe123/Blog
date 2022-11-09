```js
class Foo {
    #field = "A" // 私有属性定义
    #method() { } // 私有方法定义
    publicProperty = "foo's father"
    getField() {
        console.log(this.#field);
    }
    getMethodName() {
        console.log(this.#method.name); // 获取私有方法的名字
    }
}
```

