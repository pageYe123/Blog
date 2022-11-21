## 一、命令行工具

- 根据服务器端代码，启动后端服务
    node-dev 监听服务器端代码变动，自动更新服务器。

```shell
yarn global add node-dev
# .....
node-dev server.js
```

## 二、WebStorm

- 添加默认的文件模版
    settings → File and Codes Templates → Vue Single File Component → 编辑

- Vue 文件中 `<script>`、`<style>`中的代码保持初始缩进，即左侧留出空行。
    Editor → Code Style → Vue template → Indent children of top-level tag

### 2.1 快捷键

| command / description                                        | opration                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| surround with...<br />将代码用...包裹，在 HTML 中经常用      | 选中代码 → `cmd + option + t` → 选择 Emmet |
| Go to File                                                   | cmd + p                                    |
| Switcher                                                     | control + tab                              |
| Join Lines 合并行                                            | cmd + shift + j                            |
| Move Statement up/down 上/下移一行<br />对 HTML 标签而言，成对的`<div>`、`</div>`即使不在同一行，也会同时一起移动。 | option + up/down                           |

## 三、VSCode

### 设置快捷键

操作：Code→Preferences→Keyboard Shortcuts

| command / description            | hotkeys                         |
| -------------------------------- | ------------------------------- |
| Transform to Uppercase           | option + cmd + u                |
| Go Back \| navigate              | option + -                      |
| Go Forward \| navigate           | option + shift + -              |
| Go to File                       | cmd + p                         |
| Show All Commands                | cmd + shift + p                 |
| Change Language Mode             | control + p                     |
| Expand Selector 选中光标所在单词 | Hyper + s                       |
| 给单词加单（双）引号             | Hyper + s 之后按 ' 或 shift + ' |

### 编辑器

- 自动换行设置
    命令：toggle word wrap。当行内容过长时，可自动换行。

- 每一行外嵌套列表标签

选中5行文字→命令：Emmet wrap→输入缩写包围个别行→`ul>li*`

```
第一张专辑《Jay》就获得金曲奖最佳专辑，同时也入围当年最佳制作人、最佳作曲人和最佳新人奖三项大奖。同年，获得新加坡金曲奖最佳新人奖。
第二张专辑《范特西》获得包括最佳专辑、最佳编曲等在内的五个金曲奖奖项。
第四张专辑《叶惠美》获得最佳专辑、最佳音乐视频导演奖
```

- JavaScript 代码自动加分号。Settings 搜索。JavaScript › Format: Semicolons。

### 插件

- 配置中文。插件搜索`Chinese`，重启编辑器，呈现中文
- 提供中文分词，支持中文词汇之间光标移动（word navigation）。插件搜索`CJK Word Handler`。

### settings 配置：

位置：左上角 Code→Preference→Settings

- 代码格式化

    - HTML`<pre>`标签中的内容不进行格式化。
        搜索：Unformatted，进行设置。

        ```json
        "comment":"settings.json",
        "html.format.contentUnformatted": "pre,code,textarea",
        "html.format.unformatted": "wbr",
        ```

- 单词拼写检查：
    `C Spell: Enabled`

- 打开 settings.json 文件
    `command+shift+p`输入`Open Settings(JSON)`

- Files: auto Save
    选择 onFocusChange 

- Editor: Format On Save
    打钩

- Editor > Suggest: Show Words
    取消打钩，这样设置 snippets 会排在第一个，按下 tab 键后直接就打出来。否则词汇提示排在第一个。

- Unicode Highlight: Ambiguous Characters
    取消打钩。否则提示用英文半角符号代替中文全角符号。

- 方应杭推荐 vscode 使用 [Fira Code 字体](https://github.com/tonsky/FiraCode)

    - 配合 Font Ligatures 的配置，`===`等符号才有特殊效果。
    - 当前 font-family：`'Fira Code', Menlo, Monaco, 'Courier New', monospace`

- Workbench: Startup Editor 可以设置关闭 welcomePage

- 自定义单词分隔符：搜索 wordseparator。

### snippets

命令搜：Configure User Snippets

目前使用搭配：

```json
"editor.tabCompletion": "onlySnippets" 
"editor.quickSuggestions": {
  "other": "on"
}
```

#### 前置条件

想使用 snippets 必须设置 

```json

"editor.tabCompletion": "on" // 插入最佳匹配项（quickSuggestions→other→inline 时直接忽略其他匹配项）
// 或者
"editor.tabCompletion": "onlySnippets" 
"editor.quickSuggestions": {
        "other": "inline",  // 直接在编辑器中展示扩写后的内容（ghost text 模式），优先显示插件的代码片段
  			// "other":"on",     // 会跳出一个窗口展示扩写后的内容（suggest widget 模式），优先显示用户自定义的代码片段
  			// "other":"off",		 // 没有任何代码补全提示。按 tab 键会自动补全自定义和内置的 snippets
        "comments": "off",
        "strings": "off"
}
```

`onlySnippets`在 editor.quickSuggestions.other 是 off 时效果最好。因为开启 quicksuggestion  后必须等待提示稳定后，按tab才有效。当高速输入时，提示会经常变，比如输入`for`的前两个字母时，会先蹦出来`focus`的提示，输入第三个字母时才稳定回`for`的提示。

但是如果这样做，就没有任何代码补全提示。

#### 内置的 JavaScript snippet

现在可以禁用 VSCode 中的内置扩展。Extensions选项卡，搜索`@builtin JavaScript`，找到 JavaScript Language Basics 插件。这个插件中已经内置了很多 snippets，如`foreach`、`fori`、`forin`等等。这些内置的片段无法修改。

如果内置片段与用户定义片段，前缀冲突怎么办？看`"editor.tabCompletion"`的值，

```json
"editor.tabCompletion":"on" // 以 user snippets 为最佳匹配填入。
// 但如果用户片段的前缀是关键字，比如 for 就无法通过 tab 键触发，这是 "on" 选项的一个弊端。
"editor.tabCompletion":"onlySnippets" // 将两个冲突代码显示，让用户来选。
```

### Emmet

官方文档：

- [Emmet 有哪些 html 缩写](https://github.com/emmetio/emmet/blob/master/snippets/html.json)

- [Emmet 有哪些 css 缩写](https://github.com/emmetio/emmet/blob/master/snippets/css.json)

常用缩写记录：

```json
{
 "link:favicon": '<link rel="shortcut icon" type="image/x-icon" href="favicon.png">',
 "pos:a": "position:absolute",
  "dib": "display:inline-block"
}
```

#### Emmet: 使用内联形式补全代码

按 tab 键之前，使用内联补全扩展代码。

1）settings 中 Emmet: Use Inline Completions 打钩

2）Editor: Quick Suggestions 中 other 选择 inline 或 off

#### 修改 Emmet 默认代码片段

- meta:vp 完整内容

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

在`/Users/jeffrey/Library/Application Support/Code/User`中新建文件夹 Emmet_custom。在 Settings 中Emmet: Extensions Path，添加 `/Users/jeffrey/Library/Application Support/Code/User/Emmet_custom`。

在文件夹中新建 `snippets.json`，注意该文件必须命名为`snippets.json`，这是官方文档的要求。并在 snippets.json 中新增如下代码：

```json
{
    "html": {
        "snippets": {
            "meta:vp": "meta[name=viewport content='width=${1:device-width}, initial-scale=${2:1.0}, minimum-scale=${3:1.0}, maximum-scale=${4:1.0}, user-scalable=${5:no}']"
        }
    }
}
```

参考：

[VSCode 官方文档：Using custom Emmet snippets](https://code.visualstudio.com/docs/editor/emmet#_using-custom-emmet-snippets)

[中文版：VSCode中Emmet修改默认html或css模板snippets](https://blog.csdn.net/weixin_42655717/article/details/112533401)



### 底部状态栏

鼠标右键可以配置在状态栏中显示的插件。我把 Code Spell Checker 关了。

### 其他

- 更新失败 Could not create temporary directory: Permission denied。参考：[mac vscode 更新失败 解决办法](https://segmentfault.com/a/1190000012881106)

    ```shell
    // 1. 这一步是需要输入密码的，更改文件夹的拥有权限
    sudo chown $USER ~/Library/Caches/com.microsoft.VSCode.ShipIt/
    
    // 2. xattr 命令删除文件夹下所有文件的 quarantine 属性
    xattr -dr com.apple.quarantine /Applications/Visual\ Studio\ Code.app
    ```

    xattr 命令，参考：[展示和修改扩展属性](https://blog.csdn.net/lovechris00/article/details/113060237)

## 四、Chrome DevTools

- Sources panel debugger 状态下只能修改原始代码，formatted 以后的代码没法改。

- Chrome DevTools 提供 `getEventListeners()`API，查看元素节点或 window 绑定的事件。另一种常规查看绑定事件在 Elements → Event Listeners，但是这个不方便看 document 和 window 所绑定的事件，除非勾选 `Ancestors`。

    

### 4.1 解决的问题

- `Could not load content for webpack://...`
    uncheck the `Enable JavaScript source maps` in Chrome Dev Tool.
    ![Enable JavaScript source maps](https://i.stack.imgur.com/QpFQ7.png)