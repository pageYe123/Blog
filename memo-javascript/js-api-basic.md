## JSON.stringify()

语法：

```js
JSON.stringify(value[, replacer [, space]])
```

参数：

- `value`
    将要序列化成一个 JSON 字符串的对象。
- `replacer`
    如果是一个函数，则被序列化的值的每个属性都会经过该函数的转换。
    如果是一个数组，则只有包含在这个数组中的属性名才会被序列化。数组中只有 string 和 number 类型以及他们的包装对象会被读取，Symbol 类型不会被读取。
    如果是其他值，则对象所有的属性都会被序列化。
- `space`
    指定缩进用的空白字符串，用于美化输出。
    如果是个数字，表示有多少个空格。最大为 10，超过 10 则等同于 10。
    如果是个字符串，字符串会被插入，最大取字符串中的前 10 个字符。经常用 JS 中的转义字符，比如 `"\t"` 表示一个 tab 键。
    如果是其他值，则没有空格。

返回值：序列化之后的 JSON 字符串。

注意：

`function(){}`、 `Symbol('')`、`undefined`都会被转为 `null`

示例：

```js
// 1.1 Using a function as replacer
// Filtering out property value whose type is string
function replacer(key, value) {
  // console.log(key) // 对象 foo 的属性名
  return (typeof value === "string" ? undefined : value)
}

const foo = {
  foundation: "Mozilla",
  model: "box",
  week: 45,
  transport: "car",
  month: 7,
};
JSON.stringify(foo, replacer); // '{"week":45,"month":7}'


// 1.2 Using an array as replacer
const foo = {
  foundation: "Mozilla",
  model: "box",
  week: 45,
  transport: "car",
  month: 7,
};

JSON.stringify(foo, ["week", "month"]);
// '{"week":45,"month":7}', only keep "week" and "month" properties


// 2. space 参数
// 一般采用 2 个空格或"\t"即 4 个空格
console.log(JSON.stringify({ a: 2 }, ["a"], "  ")); 
/**
{
  "a": 2
}
**/
```





## e.stopPropagation() 和 e.stopImmediatePropagation() 的区别

参数：无

返回值：无

含义：两者都阻止事件冒泡，后者还多了个功能：**取消「当前元素」余下的事件监听函数**。

一个元素可能同时绑定多个同类型事件的「事件监听函数」，当该类型的事件触发时，函数会按照被添加的顺序执行。如果调用 `event.stopImmediatePropagation()` 则余下的未执行的函数不会执行。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Document</title>
</head>

<body>
    <div id="div">
        <textarea id="textarea" cols="30" rows="10"></textarea>
    </div>
    <script>
        textarea.addEventListener('keydown', (e) => {
            if (e.keyCode === 13 && e.metaKey) {
                console.log('更新1')
            }
        })

        textarea.addEventListener('keydown', (e) => {
            if (e.keyCode === 13 && e.metaKey) {
                console.log('更新2')
                // 解开注释查看效果
                // e.stopPropagation();          // 仅仅阻止事件冒泡到祖先节点，即'div 更新'的事件不执行
                // e.stopImmediatePropagation(); // 不仅阻止事件冒泡到祖先节点，并且阻止该元素之后绑定同类事件执行，即'更新3'的事件也不执行。
            }
        })

        textarea.addEventListener('keydown', (e) => {
            if (e.keyCode === 13 && e.metaKey) {
                console.log('更新3')
            }
        })

        div.addEventListener('keydown', (e) => {
            if (e.keyCode === 13 && e.metaKey) {
                console.log('div 更新')
            }
        })
    </script>
</body>

</html>
```