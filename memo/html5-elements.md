## 六种元素

There are six different kinds of elements: 

- [void elements](https://html.spec.whatwg.org/#void-elements) 空元素
- [the `<template>` element](https://html.spec.whatwg.org/#the-template-element-2)
- [raw text elements](https://html.spec.whatwg.org/#raw-text-elements)
- [escapable raw text elements](https://html.spec.whatwg.org/#escapable-raw-text-elements)
- [foreign elements](https://html.spec.whatwg.org/#foreign-elements)
- [normal elements](https://html.spec.whatwg.org/#normal-elements).

参考：https://html.spec.whatwg.org/#elements-2

## label 标签

关联一个 label 和 input 标签，label 将作为 input 标签的文字说明。

for 属性指向 input 的 id。效果：点击 label 标签，自动聚焦到 input。

```html
<!-- input 写在 label 里面，就可以不写 for -->
<div>
    <label>用户名<input type="text" name="name"></label>
</div>
<!--等价于-->
<div>
  <label for="nameInput">用户名<input id="nameInput" type="text" name="name"></label>
</div>
```

## 空元素（Void Elements）可以不用闭合

不用写闭合标签，标签末尾也不用写斜杠`/`

空元素可以包括：`area, base, br, col, command, embed, hr, img, input, keygen, link, meta, param, source, track, wbr`

比如

```html
<input type="text">
a
```



## 【疑惑】为什么要用 HTML5 新标签？

“符合 W3C 制定的规范”并不能让我满意，纯用 div 标签也能写出页面结构。那么接下来有必要梳理一下 HTML5 新标签的好处。

- 语义化标签和 ARIA 对屏幕阅读器友好。所谓屏幕阅读器，是将文字、图形或电脑接口的其他部分转换成语音及盲文的软件，对于视障者或阅读障碍者有帮助。Mac 中内置了一款叫 VoiceOver（旁白）的屏幕阅读器。它可以帮助用户用耳朵代替眼睛来使用设备。打开旁白后，原本的操作逻辑将会改变，用户可以抛开鼠标，根据语音提示仅通过键盘上的热键来操作电脑。[YouTube 上有关于这款应用的简短教学视频](https://www.youtube.com/watch?v=_bJp6QWp3Tg)。作者是一位“患有 RP 视网膜色素病变的患者”。这个疾病会造成视觉从周围持续恶化，最终导致失明。作者希望在视觉变差前在 YouTube 上分享这款工具的使用。如果想对这样的群体有更多了解，可以点击：[视障用户评价鱼鱼读屏](https://github.com/megabitsenmzq/Information-Pages-Of-My-Apps/blob/master/FishScreenReader/UserReview%40%E4%B8%89%E6%A8%AA%E4%B8%80%E7%AB%965964.md)

- video、audio 标签对音视频的支持，canvas 可用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。

- 带有语义的代码让你轻易地将内容与样式分开，代码更简洁明了。

    ````html
    <header>
      <nav></nav>
    </header>
    
    <!-- 比下面的要简洁 -->
    
    <div id="header">
      <div id="nav"></div>
    </div>
    ````

- 语义化标签对 SEO（Search engine optimization，搜索引擎优化） 友好。

    > 当我们使用搜索引擎时，很恼火地搜到和搜索词完全不相干的内容。仔细一看才发现自己的搜索词实际上仅仅出现在一个网站的广告词。如果我们可以通过语义化标签，将网页的重点（比如文章的内容）放在一个 article；不太重点的广告词、文章推荐列表放在 aside 里，就可以成功分化不同内容的权重。实际上搜索引擎可以给某些语义化标签更高一点的权重，从而提高搜索精准度。

- 语义化标签方便机器识别。

    - 比如 wordpress 有一个生成文章目录的插件。它的原理是遍历文章代码，提取内容中的标题标签，然后生成目录。
    - 比如爬虫插件，把别人家网站的博客内容抓取下来，然后发布到自己的博客上。原理是在后台输出要输入要抓取网页的网址，然后过滤网页内容中 div 的 class 属性，把文章的导航链接、内容页分离出来。比如`<div class="content"></div>`，通过正则匹配把需要的信息过滤出来。但是不是所有的网站都是用“class=content”的方式，它也可能用“class=post”表示文章内容。而 HTML5 的语义标签则为我们提供了一个标准化的定义。如果大家很严格地按照 HTML5 语义标签的使用规则来设计自己的网页，将有利于爬虫程序“看懂”我们的网页。

参考资料：

[马上使用 HTML5 的十大理由](http://www.alloyteam.com/2012/07/immediately-using-html5-ten-reasons/)

[HTML5，你为什么要语义化标签？](https://juejin.cn/post/6844903793289609230)

## 标签页左侧的小图标

准备一个图标，名字为 `favicon.png`，放在项目的根目录下。

 必须放在`<head>` 标签中，放在`<body>`标签中无效。

```html
<link rel="shortcut icon" type="image/x-icon" href="/favicon.png">
```

可以通过 emmet 的 snippet `link:favicon` 来触发。



## br 换行标签

必须有起始标签，不能有闭合标签。在 HTML 中只有`<br>`这种写法， `<br/>` 是 XHTML 中的写法。

## section 章节标签

section 是一个有主题的内容分组，通常里面含有 heading（标题）标签（h1~h6）。

section 元素适用场景：标题标签在文件大纲（outline）中被明确列出时，需要包裹一层 section。

```html
<article>
    <section>
        <h2>Apples</h2>
        <p>Tasty, delicious fruit!</p>
    </section>
    <p>The apple is the pomaceous fruit of the apple tree.</p>
    <section>
        <h3>Red Delicious（蛇果）</h3>
        <p>These bright red apples are the most common found in many
            supermarkets.</p>
    </section>
    <section>
        <h3>Granny Smith（澳洲青苹）</h3>
        <p>These juicy, green apples make a great filling for apple pies.</p>
    </section>
</article>
```
大纲其实就是所有标题以树结构形式呈现出的目录（table of contents）。

```html 
<div class="TOC">
    <ol>
        <li>Apples</li>
        <ol>
            <li>Red Delicious（蛇果）</li>
            <li>Granny Smith（澳洲青苹）</li>
        </ol>
    </ol>
</div>
```

<details>
<summary>大纲（目录）效果图</summary>
<img alt="大纲（目录）效果图" src="https://wx4.sinaimg.cn/large/6cdfff77gy1h4c3d9soj6j20g40b775p.jpg"/>
</details>
需要注意：

- section 可以嵌套 section 使用。如果标题作为小节嵌套（比如 h3 标签下面还有 h4 标签），h4 也作为目录的一部分，section 标签可以包裹 h4 主题的内容。

- section 并不是通用容器标签。如果用于样式布局或方便 Javascript 操作，建议使用 div 标签。

## details 详细信息展示标签

HTML `<details>`元素可创建一个挂件，仅在被切换成展开状态时，它才会显示内含的信息。`<summary>` 元素可为该部件提供概要。

```html
<details>
  	<summary>click me</summary>
    <p>信息1</p>
    <p>信息2</p>
    <p>信息3</p>
</details>
```

## u 标签

The Unarticulated Annotation element

u 元素代表一段未阐明、非本义的文本，例如中文专名号（标明人名、地名、朝代名、国名等），或将文本标记为拼写错误。

```html
<!--中文专名号-->
<p><u>屈原</u>放逐，乃赋离骚。<u>左丘</u>失明，厥有国语。<p>
<!-- 示例中，u 元素用于标记拼写错误 -->
<p>The <u>see</u> is full of fish.</p>
```



以下情况，请用其他元素：

- `em`：denote stress emphasis

- `strong`：indicate that text has strong importance

- `b`：使得文本引起注意

- `mark`：标记关键词或词组

- `cite`：标记书名

- `ruby`：标记有明确注释的文本

    ```html
    <ruby>
      汉 <rp>(</rp><rt>Han</rt><rp>)</rp>
      字 <rp>(</rp><rt>zi</rt><rp>)</rp>
    </ruby>
    ```

- `i`：标记技术术语、分类学名称、音译、思想、西文中的船名

## em vs. i 标签

样式完全一样，都是`font-style: italic;`

`em`用于语气上的强调。

`i`用于技术术语、分类学名称、音译、思想、西文中的船名。

此外，需要特殊情绪或声音时用`i`更合适。

> Sometimes, text is intended to stand out from the rest of the paragraph, as if it was in a different mood or voice. For this, the `i` element is more appropriate.

https://developer.mozilla.org/en-US/docs/Web/HTML/Element/em#usage_notes

## strong vs. b 标签

## em vs. strong 标签

`em` 可以强调单词、整个句子，可以嵌套使用。[whatwg.org](https://html.spec.whatwg.org/multipage/text-level-semantics.html#the-em-element) 有很好的示例。

`strong` 标签

- Importance：可以用于 heading，caption，paragraph 来表明这部分的**重要性**明显高于其他琐碎的部分，需要优先注意。
- Seriousness：标记警告或引起注意
- Urgency：标记用户需要立刻看到的内容

https://stackoverflow.com/questions/1936864/what-is-the-difference-between-strong-and-em-tags
