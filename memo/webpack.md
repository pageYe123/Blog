# Webpack 使用经验

## Webpack 的作用

- 将一个或多个 JS 文件打包成对应的文件
- 将一个或多个 CSS 文件打包成对应的文件
- 压缩代码
- 将高版本的 JS 转译成低版本的 JS

## 在项目开发环境中使用 Webpack

```shell
yarn add webpack@4 webpack-cli@3 --dev
# 等价于
npm install webpack@4 webpack-cli@3 --save-dev
```

### 使用`webpack`命令

```shell
node_modules/.bin/webpack
# 或者
npx webpack
# 或者在 package.json 中的 "scripts" 字段中可以直接使用 webpack 命令
# "scripts" 中加入 "build": "rm -rf dist;webpack" 之后运行命令 npm run build 或者 yarn build
```

Windows 上需要将`;`换成`&&`：`"build": "rm -rf dist && webpack"`



使用「自定义配置文件」。在 package.json 中修改：

```shell
"scripts": {
    "build": "rm -rf dist ; NODE_ENV=production webpack --config webpack.config.prod.js"
}
```

## loader

style-loader：将 JS 字符串转为`<style>`节点，插入到`<head>`节点中。

css-loader：将 CSS 转化为 JS 字符串（CommonJS 模块）。

[sass-loader](https://webpack.docschina.org/loaders/sass-loader/)：Sass 编译为 CSS

## plugins

html-webpack-plugin：自动生成 HTML。

webpack-dev-server： 

- 文件内容变化就自动转译代码，并自动刷新页面
- 提供 server 方便开发预览

mini-css-extract-plugin：将多个 CSS 代码提取成单独的一个文件。



## webpack.config.js

```js
const path = require('path');

module.exports = {
  mode: 'development', // 'production' 生产版本
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js',
  },
};
```

`output.filename`中的占位符：

- `[name]` - chunk name (e.g. `[name].js` -> `app.js`). If a chunk has no name, then its id will be used.

- `[contenthash]` - 根据文件内容产生一个 md4-hash 值（e.g. `[contenthash].js` -> `4ea6ff1de66c537eb9b2.js`）。**文件名带上哈希值，可用于更新 HTTP 缓存。**因为缓存是跟着文件名走的，改了文件名就是一个硬盘上没有的新文件，只能发送 HTTP 请求来获取文件。
    背景知识：HTTP 响应头中 Cache-Control 字段用于设置缓存，决定文件是发 HTTP 请求还是从硬盘缓存读取。



## 解决报错

### node18 执行 webpack 报错

![node18执行webpack报错](https://wx4.sinaimg.cn/large/6cdfff77gy1h7xlg7u1kvj20uw0g6tj4.jpg)

原因是 node18 删掉了 webpack 用到的 hash 函数，解决办法有：

- 降级 node 到16（卸载重装）(推荐)
- 按照官方 issue 推荐，先执行`export NODE_OPTIONS=--openssl-legacy-provider`, 再执行 webpack 命令，见[原文](https://github.com/webpack/webpack/issues/14532#issuecomment-947012063)。

### Could not load content for webpack://...

uncheck the `Enable JavaScript source maps` in Chrome Dev Tool.

![Enable JavaScript source maps](https://i.stack.imgur.com/QpFQ7.png)