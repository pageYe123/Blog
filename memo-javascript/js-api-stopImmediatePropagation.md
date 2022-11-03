## stopPropagation 和 stopImmediatePropagation 的区别

两者都阻止事件冒泡，后者还多了个功能：取消「当前元素」余下的事件监听函数。

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
                // e.stopPropagation();          // 仅仅阻止事件冒泡的祖先节点
                // e.stopImmediatePropagation(); // 不仅阻止事件冒泡到祖先节点，并且阻止该元素之后绑定同类事件执行，即'更新3'的事件不执行。
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