> 前言：记录软件的配置和软件常用操作。

## Alfred

- 文件搜索功能
    打开文件：`空格 + 文件名`
    打开文件所在文件夹：`find + 文件名`
    搜索包含指定内容的文件：`in + 文件内容字符串`
    在 Default Results 面板可设置“搜索范围 search scope”。
    参考：[Alfred系列教程---文件搜索](https://www.jianshu.com/p/2ce1dd633f4f)

## Mac改键软件

Karabiner-Elements

- 系统偏好设置→安全性与隐私→隐私→输入监听→右键karabiner_observer→手动添加karabiner_grabber，然后两者都打上勾。
- 把 Karabiner-EventViewer.app 加入「输入监听」。
- 允许 Karabiner-DriverKit-VirtualHIDDeviceClient  app，进入软件主页面会有提示。
- Karabiner-EventViewer 用于配置不生效时，调试按键。

配置文件地址：`~/.config/karabiner/karabiner.json`

## Github 官网

- 搜索功能，有限制。
    - 只有小于 384 KB 的文件可搜索。
    - 只有文件少于 500,000 个的存储库可搜索。
    - Only repositories that have had activity or have been returned in search results in the last year are searchable.
    - **解决办法：代码下载到本地，用本地IDE的搜索功能。**

- 如果想搜索 README.md 的内容，可以用指令`<搜索词> filename:readme path:/`，或者直接在项目首页`⌘+f`搜索关键词。
- 查看 Github 某个仓库大小。
    登录 Github 网页首页，点击右上角自己的头像，下拉菜单中选中`settings`。左侧点击`repositories`，这里就有每个repository对应的存储空间大小。

## Mac

- 想只专注当前工作，比如我就想写博客，请按`option+command+h`，隐藏其他所有。
- 如何把 Downloads 文件夹变成中文“下载”。
    `touch ~/Downloads/.localized`
- 配置应用中的右键菜单。
    可以通过“键盘→快捷键→服务”进行配置。比如取消勾选“添加到印象笔记”，右键菜单中就没有这个选项了。
- 可以自定义 App 快捷键
    设置 → 键盘 → 快捷键 → ＋ → 「菜单标题」要填写菜单命令的准确名称。
- 终端中使用代理。系统偏好设置中的代理设置在 shell session 中是不会生效的，在终端中使用代理、需要手动提供 `http_proxy`、`https_proxy` (或 `all_proxy`) 环境变量，Shell 中很多可执行程序会自动识别这三个环境变量，然后决定是否走代理。注意大小写问题，比如`curl` 不识别大写的`HTTP_PROXY`，只能用小写的`http_proxy`；但是`HTTPS_PROXY`支持小写(参见`man curl`)。
- Finder 中显示隐藏文件及文件夹，按快捷键`⌘+⇧+.`

## Mac Automator

自定义的 AppleScript 脚本存储在：`~/Library/Services`

Applescript 文件后缀名为`scpt`，可搭配「脚本编辑器」或 Automator 实现自动化。

## Mac iTerm2

- 显示之前的命令历史：`command+shift+h`

- 撤销编辑，按 `control + -`。`control+y`重复上次的粘贴项。按 command+z 没有效果，不知道在哪里配置。

- 文字的左边距调整（方便截图）：Appearance → Side margins

- 设置启动 shell：默认为 zsh。
    profiles → command → `bin/bash`默认在 zsh 里面调用 bash

![image-20220716114309530](https://wx4.sinaimg.cn/mw690/6cdfff77gy1h48mbz1dgwj20jw04habh.jpg)

- `man curl`中的搜索用`/`不要用`⌘+f`，前者才是全文搜索，后者搜索范围仅限已显示的部分。

    

## Mac Zsh

安装目录：`/Users/jeffrey/.oh-my-zsh`

## Mac Homebrew

Homebrew brew update 长时间没反应：https://juejin.cn/post/6931190862295203848

## Chrome 浏览器

关闭使用文本光标来浏览网页。设置→无障碍→使用文本光标浏览网页→关闭。快捷键 F7 误碰会开启。



地址栏访问：`chrome://inspect/#devices`

有一个可以调试 Node 的开发工具，我还不知道怎么用。但是一旦添加了监听，http-server 启动的服务就会一直发送请求。

<img src="https://wx4.sinaimg.cn/large/6cdfff77gy1h44axmj5zuj20ft0bvgof.jpg" alt="image-20220712180449650"  />

### Shortkeys 插件

屏蔽浏览器默认快捷键。`chrome://extensions/shortcuts`搜索`UTILITY: Do nothing (disable browser shortcut - experimental)`设置快捷键`⌘+s`，选择“在 Chrome 中”。

如果上述不成功，在[扩展程序选项](chrome-extension://logpjaacgmcbpdkdchjiaagddngobkck/options/options.html)中新增配置。如果还不行，则 Chrome 不让改这个键。

文档参考：[Can I use Shortkeys to disable a core browser shortcut?](https://github.com/crittermike/shortkeys/wiki/FAQs-and-Troubleshooting#Can_I_use_Shortkeys_to_disable_a_core_browser_shortcut)

## 软件字体统一设置

需求来源：写博客的时候能区分出中文的左右双引号“”`“”`

最终选择 Monaco 作为 Mac 下 Typora 的字体。能区分 0 和 o。

参考：[程序员选择字体的标准是?](https://segmentfault.com/q/1010000000193702#a-1020000000193822)

## 翻墙软件

- ClashX 修改代理端口。

ClashX > 配置 > 打开本地配置文件夹，找到“config.yaml”打开编辑（只有这份文件的端口设置会随ClashX启动生效，自定义的配置文件中的端口是无效的）。

- Chrome 插件 SwitchyOmega，每次换翻墙客户端时需要核对端口号是否一致。情景模式→代理模式→代理端口

- proxychains-ng
    `~/.proxychians.conf` 中的[ProxyList][不能同时使用多个 127.0.0.1 地址，只能使用一个](https://github.com/rofl0r/proxychains-ng/issues/30)。

    ```shell
    [ProxyList]
    # add proxy here ...
    # meanwile
    # defaults set to "tor"
    #socks4 	127.0.0.1 9050
    socks5 127.0.0.1 51837
    # http 127.0.0.1 58591
    ```

    

## VSCode

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

### 设置快捷键

操作：Code→Preferences→Keyboard Shortcuts

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



## 在线 jsbin

`cmd + shift + l` 格式化代码快捷键

## Typora

- 本地图片：`/Users/jeffrey/Library/Application Support/typora-user-images`
  
- 在`/Application`目录中，`Typora.app`如果改名为`Typora-foo.app`是打不开的，也就是说 Typora 会判断——如果应用名字不是`Typora.app`，就无法打开。
  
- 自定义 CSS 主题
    css 文件的命名有格式要求。不能是驼峰式，不能有下划线，需要全部小写，以连字符分割。
    如：只有 *github-custom.css* 这个名称是允许的。
    :no_good:github custom.css \ githubCustom.css \ github_custom.css 均无法正常加载。
    
    ```css
    /* custom css by ysq 20220711 */
    ol ol,
    ul ol {
        list-style-type: lower-roman;
    }
    
    ul ul ol,
    ul ol ol,
    ol ul ol,
    ol ol ol {
        list-style-type: lower-alpha;
    }
    ```
    
    

*感悟：还是要读官方文档。*发现解决问题的思路是[官方主题 Compact Night](https://theme.typora.io/theme/Compact-Night/) 最后 Installation 里写

> Put *compact-night* folder and *compact-night.css* file into the open folder 
