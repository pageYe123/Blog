## var()函数

用函数传参的变量形式代替属性值，在 :root 上定义然后使用它

```css
:root {
  --main-bg-color: pink;
}

body {
  background-color: var(--main-bg-color, black);/* 当第一个值未定义，回退值 black 生效*/
}
```

参考：[MDN var()函数](https://developer.mozilla.org/zh-CN/docs/Web/CSS/var)

## font-family

`font-family: Georgia, serif;`

`font-family: "Gill Sans", sans-serif;`

属性 `font-family` 列举一个或多个由逗号隔开的字体族。每个字体族由 `<family-name>` 或 `<generic-name>` 值指定。浏览器会选择列表中第一个该计算机上有安装的字体，或者是通过 [`@font-face`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face) 指定的可以直接下载的字体。

参考：[MDN font-family](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family)