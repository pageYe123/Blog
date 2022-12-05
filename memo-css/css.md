## Flexbox

**Flex container:**

| 属性名          | 含义                                       | 属性值                                                       |
| --------------- | ------------------------------------------ | ------------------------------------------------------------ |
| flex-direction  | 确定主轴方向<br />（交叉轴方向与主轴垂直） | row \| row-reverse \| column \| column-reverse               |
| flex-wrap       | 是否折行                                   | wrap \| nowrap                                               |
| flex-flow       | （缩写）                                   | `<flex-direction>` | `<flex-wrap>`                           |
| justify-content | 主轴对齐方向，控制所有 item                | flex-start \| flex-end \| center \| space-between \| space-around \| space-evenly |
| align-items     | 交叉轴对齐方向，控制所有 item              | flex-start \| flex-end \| center \| stretch \| baseline      |

**Flex item：**

| 属性名      | 含义                                                         | 属性值                                      |
| ----------- | ------------------------------------------------------------ | ------------------------------------------- |
| order       | 排序                                                         | 数字，默认为 0                              |
| flex-grow   | 有多余空间怎么分                                             | 数字，默认为 0 表示不增长                   |
| flex-shrink | 空间不够时 item 怎么缩小                                     | 数字。默认为 1 大家一起缩小；0 表示防止变瘦 |
| align-self  | 对 container 提供的 align-items 进行定制，<br />使得某个 item 在交叉轴上使用自己的对齐方式。 | 同 align-items                              |

**使用经验**

flex item 从底部开始向上伸展，又想显示滚动条。**慎用`flex-end` 会毁坏滚动条。**

```css
.container{
    overflow-y: auto;
    display: flex;
	flex-direction:column nowrap;
	/* justify-content:flex-end DO NOT USE: breaks scrolling */
}

.container::before{ /* 保证内容从底部开始 */
    content:'';
    display: block;
    margin-top: auto;
}
```





## 水平、垂直居中

如果能确定只有一行，用`line-height`等于`height`的方式。

```css
.box{
    height: 100px;
}
.h-center{
    text-align: center;
}
.v-center{
    text-align: center;
    line-height: 100px;
}
```

如果可能会出现两行甚至多行的情况，用 flex 更合适：

```css
.box{
    height: 100px;
}
.flex{
    display: flex;
    justify-content : center;
    align-items : center;
}
```



## `font-size`与文字产生的 width

英文字产生的 width 与字体有关，不同字体产生不同 width，但中文字产生的 width 与字体无关：

- 中文字产生的宽度与仅由 font-size 决定。对于大于 12px 的 font-size，一个汉字产生的 width 等于自身 font-size 值。所以有**`1em` 等于一个中文汉字宽度**。

- Chrome 支持的字体大小有最小值。中文字最小值为 12px，英文字最小值为 14px。主要是因为 chrome 认为字太小会增加识别难度。

## CSS 单位`em`和`rem`

**em**

相对于自身的以 `px` 为单位的`font-size`大小。

- ~~元素的 `font-size` 是**相对于父元素**`font-size`~~
- ~~元素的其他属性（`width/height/padding/margin`），使用`em`是**相对于自身**的`font-size`的`px`值~~

相对关系：

`width/height/padding/margin` → 自身`font-size` → 父元素的`font-size`

HTML里面，`em`大小**等于一个中文汉字的宽度**。

**rem**

- rem 值相对于根元素，根元素是谁？`<html>`元素。通常做法是给 html 元素设置一个字体大小，然后其他元素的长度单位就为 rem。

**应用场景**

em/rem 用于做响应式页面，不过我更倾向于 rem，因为 em 不同元素的参照物不一样（每个元素有不同的父元素），所以在计算的时候不方便，相比之下 rem 就只有一个参照物（html元素），这样计算起来更清晰。

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