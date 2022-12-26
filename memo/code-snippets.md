# 常用代码片段

## JavaScript

JS 控制滚动条
```js
// wrapper 的滚动条距离顶部 1000px，只能跟数字。
wrapper.scrollTop = 1000

// wrapper 的滚动条到底
// 方法一：
wrapper.lastElementChild.scrollIntoView()
// 方法二：
// `scrollHeight`是元素内容的高度
wrapper.scrollTop = wrapper.scrollHeight
```

`ele.matches(selector)`判断元素是否匹配某个选择器

```javascript
ele.matches('span.text')

ele.matches('li')
// 等价于
ele.tagName.toLowerCase() === 'li'
```

新声明的全局变量，防止覆盖原有的全局变量：

```js
let prop= 'log' // 测试用例

let isFn = function (x) { return (x instanceof Function) }
let raw = window[prop]
let raw_isFn = isFn(raw)
if (raw == null || raw_isFn) {
    window[prop] = function (...args) {
        raw && raw_isFn && raw()
        console[prop](...args)
    }
}
```



## CSS

css reset，一般作用在元素标签，伪类或伪元素上。

```css
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
*::after,
*::before {
    box-sizing: border-box;
}

ul,
ol {
    list-style-type: none;
}
img {
    max-width: 100%;
    max-height: 100%;
}
a {
    color: inherit;
    text-decoration: none;
}

:focus { /* 输入框聚焦时不产生边框*/
  outline: none; 
}
```

一旦用了浮动`float`，必须在浮动元素上加 `.clearfix`

```css
.clearfix::after {
  content: '';
  display: block;
  clear: both;
}
```



属性选择器

```css
/* 存在 class 属性，并且属性值以 level 开头的元素 */
div[class^=level]

/* 存在 class 属性，并且属性值以 level 结尾的元素 */
div[class$=level]
```

隐藏滚动条，但用鼠标滚轮仍然可以滚动

```css
/* -webkit- (Chrome, Safari, newer versions of Opera)*/
ele::-webkit-scrollbar {
    width: 0 !important;
}
/* -moz- (Firefox) */
ele { overflow: -moz-scrollbars-none; }
/* -ms- (Internet Explorer +10) */
ele { -ms-overflow-style: none; }
```

CSS 强调色（accent-color 属性）改变 HTML 原生表单元素配色

```css
/* https://web.dev/i18n/zh/accent-color */
/* input radio checkbox range 内置配色方案就变了 */
:root {
   accent-color: deeppink; 
}
```



## HTML

