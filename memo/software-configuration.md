> 前言：记录软件的配置和软件常用操作。

## Alfred

- 文件搜索功能
    打开文件：`空格 + 文件名`
    打开文件所在文件夹：`find + 文件名`
    搜索包含指定内容的文件：`in + 文件内容字符串`
    在 Default Results 面板可设置“搜索范围 search scope”。
    参考：[Alfred系列教程---文件搜索](https://www.jianshu.com/p/2ce1dd633f4f)
- 隐藏菜单栏的 icon
    Alfred Preferences → Appearance → Options（左下角） → Hide menu bar icon



## Github 官网

- 开源项目的 contributors
  
    > The contributors graph sums weekly commit numbers onto each Sunday, so your time period must include a Sunday.
    > 贡献者图表每周日汇总本周的提交数量，所以你的时间段必须包括一个星期天。
    
- 搜索功能，有限制。
  
    - 只有小于 384 KB 的文件可搜索。
    - 只有文件少于 500,000 个的存储库可搜索。
    - Only repositories that have had activity or have been returned in search results in the last year are searchable.
    - **解决办法：代码下载到本地，用本地IDE的搜索功能。**
    
- 如果想搜索 README.md 的内容，可以用指令`<搜索词> filename:readme path:/`，或者直接在项目首页`⌘+f`搜索关键词。
- 查看 Github 某个仓库大小。
    登录 Github 网页首页，点击右上角自己的头像，下拉菜单中选中`settings`。左侧点击`repositories`，这里就有每个repository对应的存储空间大小。

## macOS
- macOS `/Applications/<应用>/Contents/info.plist` 文件中，[`CFBundleIdentifier`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleidentifier?language=objc) 字段，就是对应 Karabiner 的 `bundle_identifiers`。
```xml
<!-- 对应 Karabiner-EventViewer 的 Bundle identifier -->
<key>CFBundleIdentifier</key>  
<string>com.clipy-app.Clipy</string>
```
- 从用户可感知的运行方式分类，macOS 的应用程序分为「窗口应用」和「菜单栏应用」。「菜单栏应用」是得益于 macOS 上菜单栏的构思，产生了一类独特的应用，它们没有单独的窗口，所有的交互都在单击菜单栏图标后展开的弹窗中完成。
  macOS系统的软件开发中有这样的一个布尔值：`LSUIElement`（Launch Service）. 它来决定这个应用是否是代理应用（agent app 仅在后台运行，不会出现在 Dock 中）。
  
- 想只专注当前工作，比如我就想写博客，请按`option+command+h`，隐藏其他所有。

- 如何把 Downloads 文件夹变成中文“下载”。
    `touch ~/Downloads/.localized`
    
- 配置应用中的右键菜单。
    可以通过“键盘→快捷键→服务”进行配置。比如取消勾选“添加到印象笔记”，右键菜单中就没有这个选项了。
    
- 可以自定义 App 快捷键
    设置 → 键盘 → 快捷键 → ＋ → 「菜单标题」要填写菜单命令的准确名称。
    
- 终端中使用代理。系统偏好设置中的代理设置在 shell session 中是不会生效的，在终端中使用代理、需要手动提供 `http_proxy`、`https_proxy` (或 `all_proxy`) 环境变量，Shell 中很多可执行程序会自动识别这三个环境变量，然后决定是否走代理。注意大小写问题，比如`curl` 不识别大写的`HTTP_PROXY`，只能用小写的`http_proxy`；但是`HTTPS_PROXY`支持小写(参见`man curl`)。

- Finder 中显示隐藏文件及文件夹，按快捷键`⌘+⇧+.`

- macOS 按键符号

 |                                                                                    按键符号                                                                                     |                     按键名                     |                       说明                       |
 |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------:|:------------------------------------------------:|
 |    <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/aadb53ae1fdbce0eaf46b6954280199a.png" alt="Top 符号" style="zoom:33%;" />    |                    Home ↖︎                     |              对应 `fn + left_arrow`              |
 |    <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/3a3ff6163a8e9957380b2775de10ed5f.png" alt="End 符号" style="zoom:33%;" />    |                     End ↘︎                     |             对应 `fn + right_arrow`              |
 |  <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/e22d54eccd7055d5a249d5c434238338.png" alt="Page Up 符号" style="zoom:33%;" />  |                   Page Up ⇞                    |               对应 `fn + up_arrow`               |
 | <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/5d5898f17582711e444b666ab9f14086.png" alt="Page Down 符号" style="zoom:33%;" /> |                  Page Down ⇟                   |              对应 `fn + down_arrow`              |
 |  <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/e9bcb32d2329460fa9262bb80287aeb4.png" alt="Option 符号" style="zoom:33%;" />   |                    Option ⌥                    |                        -                         |
 |  <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/da323a6c28ae08e4e11659963d334729.png" alt="Command 符号" style="zoom:33%;" />  |                   Command ⌘                    |                        -                         |
 |  <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/7b00c26c8add24c1d9728d21fbf8869a.png" alt="Control 符号" style="zoom:33%;" />  |                   Control ⌃                    |                        -                         |
 |   <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/0f795759124f9320650e97ddd0ff4a84.png" alt="Shift 符号" style="zoom:33%;" />   |                    Shift ⇧                     |                        -                         |
 |                          <img src="https://upload-images.jianshu.io/upload_images/1231311-254366a2778edc44.png" alt="caps_lock" style="zoom: 12%;" />                           |                  Caps Lock ⇪                   |                        -                         |
 |  <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/5218dc0ac3937a55c4c67ed1317fd499.png" alt="Escape 符号" style="zoom:33%;" />   |                     Esc ⎋                      |                        -                         |
 | <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/f04a43807d23b13bd4a5a2eeeaa20a7e.png" alt="右制表符符号" style="zoom: 33%;" />  |                     Tab ⇥                      |        右制表符。左制表符为 `Shift +Tab`         |
 |  <img src="https://help.apple.com/assets/61E8950FFC2FB340377F9335/61E89511FC2FB340377F933E/zh_CN/5886bf5fc6b62c1825cb5336b78a8cca.png" alt="Delete 符号" style="zoom:33%;" />   | Delete ⌫<br /> （等同于 Windows 的 Backspace） | 向左删除键（回退键）。向右删除键为 `fn + delete` |
 |                <img src="https://support.apple.com/library/content/dam/edam/applecare/images/en_US/mac/mac-fn-key-globe-icon.png" alt="img" style="zoom:33%;" />                |                     Fn 🌐                      |              Fn key 也叫 Globe key               |

## macOS 预置文件夹

- `~/Library/Application Support` 包含了应用相关的数据以及支持文件，比如第三方的插件，帮助应用，模板以及应用使用到但是并不需要用来支持运行的额外资源文件。
- `~/Library/Preferences/*.plist` 属性列表文件（property list files）通常用于存储用户的设置。只能用 Xcode 打开，VSCode 打开乱码。
  macOS 独有的 `defaults` 命令可以修改这些文件中的属性值。
```shell
# 1. 读取整数值
defaults read com.clipy-app.Clipy "kCPYPrefShowStatusItemKey"
# 2. 覆盖属性值，默认是字符串类型
defaults write com.clipy-app.Clipy "kCPYPrefShowStatusItemKey" 2
# 2.1. 设置为整数类型
defaults write com.clipy-app.Clipy "kCPYPrefShowStatusItemKey" -int "2"
# 3. 删除属性值
defaults delete com.clipy-app.Clipy "kCPYPrefShowStatusItemKey"
# 4. 查看系统重所有 domain（对应 Karabiner 中的 "bundle_identifiers"）
defaults domain
```

- `~/Library/Containers` 主要是 macOS 的原生应用在里面生成用户本地存储，用于用户配置、数据保存等。

## macOS Automator

自定义的 AppleScript 脚本存储在：`~/Library/Services`

Applescript 文件后缀名为`scpt`，可搭配「脚本编辑器」或 Automator 实现自动化。

## macOS iTerm2

- 显示之前的命令历史：`command+shift+h`

- 撤销编辑，按 `control + -`。`control+y`重复上次的粘贴项。按 command+z 没有效果，不知道在哪里配置。

- 文字的左边距调整（方便截图）：Appearance → Side margins

- 设置启动 shell：默认为 zsh。
    profiles → command → `bin/bash`默认在 zsh 里面调用 bash

![image-20220716114309530](https://wx4.sinaimg.cn/mw690/6cdfff77gy1h48mbz1dgwj20jw04habh.jpg)

- `man curl`中的搜索用`/`不要用`⌘+f`，前者才是全文搜索，后者搜索范围仅限已显示的部分。

- 取消勾选 iTerm2 → secure keyboard entry，否则 iTerm2 打开的新应用都被 iTerm2 盖住。

    

## macOS Zsh

安装目录：`/Users/jeffrey/.oh-my-zsh`

## macOS Homebrew

Homebrew brew update 长时间没反应：https://juejin.cn/post/6931190862295203848

## macOS Karabiner-Elements

见语雀文章：《[Karabiner-Elements 改键配置](https://www.yuque.com/jeffrey-ysq/popkql/lu8f1zfal7df9a4p/edit)》

## macOS Clipy
 [Clipy](https://github.com/Clipy/Clipy/blob/40445fd4f453edf5dd39cf18b9e4b5ffbf48deaa/Clipy/Sources/Preferences/Panels/Base.lproj/CPYGeneralPreferenceViewController.xib#L168) 剪贴板增强工具，菜单栏图标消失。
```shell
defaults delete com.clipy-app.Clipy kCPYPrefShowStatusItemKey
# kCPYPrefShowStatusItemKey 是 Clipy 源代码中定义的属性名
# 修改了 ~/Library/Preferences/com.clipy-app.Clipy.plist
```
`hyper_capslock + F12` 唤醒主菜单。目前菜单栏图标处于隐藏状态。

## Chrome 浏览器

- 插件存放路径：`/Users/jeffrey/Library/Application Support/Google/Chrome/Default/Extensions`
- 关闭使用文本光标来浏览网页。设置→无障碍→使用文本光标浏览网页→关闭。快捷键 F7 误碰会开启。

- 地址栏访问：`chrome://inspect/#devices`

有一个可以调试 Node 的开发工具，我还不知道怎么用。但是一旦添加了监听，http-server 启动的服务就会一直发送请求。

<img src="https://wx4.sinaimg.cn/large/6cdfff77gy1h44axmj5zuj20ft0bvgof.jpg" alt="image-20220712180449650"  />

### Chrome Shortkeys 插件

屏蔽浏览器默认快捷键。`chrome://extensions/shortcuts`搜索`UTILITY: Do nothing (disable browser shortcut - experimental)`设置快捷键`⌘+s`，选择“在 Chrome 中”。

如果上述不成功，在[扩展程序选项](chrome-extension://logpjaacgmcbpdkdchjiaagddngobkck/options/options.html)中新增配置。如果还不行，则 Chrome 不让改这个键。

文档参考：[Can I use Shortkeys to disable a core browser shortcut?](https://github.com/crittermike/shortkeys/wiki/FAQs-and-Troubleshooting#Can_I_use_Shortkeys_to_disable_a_core_browser_shortcut)

## 软件字体统一设置

需求来源：写博客的时候能区分出中文的左右双引号“”`“”`

最终选择 Monaco 作为 macOS 下 Typora 的字体。能区分 0 和 o。

参考：[程序员选择字体的标准是?](https://segmentfault.com/q/1010000000193702#a-1020000000193822)

## 翻墙软件

- 在 ClashX 代理规则列表中新增 domain 规则：
```yaml
- DOMAIN-SUFFIX,google.cn,Proxy
```
解决 sm.ms [文件](https://recaptcha.google.cn/recaptcha/api.js?render=6LdArYchAAAAADs9BJEP0Ud-MpC54mNN90Bp0BvK)加载不出来的问题
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


## 语雀

- 查看「本月新建文稿数」、「本月上传流量使用情况」
    点击头像 → 账户设置 → 语雀会员

## Stack Overflow

- 查看 my watched tags：
    https://stackoverflow.com/questions/tagged?tab=Unanswered&tagMode=Watched
- 


## JS Bin —— 在线 IDE

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
