# 第一个 node.js 脚本
文件名：`unicode`，所属目录：`/usr/local/bin`（已加入环境变量，用户写的脚本一般都放在这里）。

使用：在命令行任意目录下，输入`unicode <单个字符>`，返回对应的 html、javascript、css 转义字符：

![image](https://user-images.githubusercontent.com/11813936/179329127-aa5e9bd6-c742-4d23-8451-413098b05ae9.png)

如`⌘`对应 unicode 编码：`U+2318`。2318是十六进制，对应的十进制是`8984`。

## 文件源代码

```Shell
#!/usr/bin/env node
var arguments = process.argv.splice(2);
if (arguments.length === 0) {
    console.log('one character is expected!');
    return;
}
if (arguments.length > 1) {
    console.error('command supports only one argument!');
    return;
}
var character = arguments[0];
if (character.length > 1) {
    console.error('there is too much characters, only one is supported!');
    return;
}
var decimal = character.charCodeAt(0); // 十进制
var unicode = decimal.toString(16); // 十六进制

console.log(`html 转义字符: &#${decimal};
javascript 转义字符: \'\\u${unicode}\'
css 转义字符: \'\\${unicode}\'`);
```

`process.argv` 前两个参数不使用。
`console.error()` VS `throw new Error()`console 返回的效果更清爽，但是需要加 return 来停止程序继续运行。throw 会把堆栈调用信息都抛出来。

## 转义字符原理讲解
### html 字符实体（字符转义）

对于 Unicode，HTML 采用十进制方式计算，并以`&#{unicode};`方式进行表示。例如对于“⌘”字符，unicode 编码是`\u2318`，对应十进制值为`8984`，HTML的表示方式如下：

`<span>&#8984;</span>`

### 补充：CSS、JS 与 Unicode 字符互转

对于 Unicode，CSS 采用十六进行方式进行计算，并以`\{unicode}`方式进行表示：

```css
span::before {
    content : '\2318';
}
```

对于 Unicode，Javascript 也采用十六进制的方式进行计算，但以`\u{unicode}`的方式表示：

`document.querySelector('span').innerHTML = '\u2318';`
