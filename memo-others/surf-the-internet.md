## 入门篇
以下将「翻墙」简称为「FQ」，本文教你如何使用 Trojan 来 FQ。

1. 购买代理服务器（付款前在群里询问一下是否可用再买，防止买了不能用）
    1. 进入 [SS 网站 https://portal.shadowsocks.nz/aff.php](https://portal.shadowsocks.nz/aff.php?aff=473) 购买服务器
   
        0. 如果此网站你打不开，那么你可以试试[另一个网站](https://nthu.cc/#/register?code=gs5WasSD)，但是本教程不适合[另一个网站](https://nthu.cc/#/register?code=gs5WasSD)，好在该网站自己提供了教程和客户端，你需要自行查看（看完之后你再回来看本教程的第二篇 [如何用 Chrome 插件 FQ](https://github.com/sun-shadow/Surf_the_Internet/blob/master/%E6%8F%92%E4%BB%B6%E7%AF%87.md)，你唯一需要做的改动就是看完第二篇之后，把插件配置中的 agent 里的 1080 端口，改成 33211 端口，不会的话再找我。）
        1. 请购买Lite年付版，不用担心流量不够，因为随时可以购买流量补充包。
        2. 有同学反应无法注册，其实不用注册，你购买的时候会提示你注册的。
        3. 购买的时候会让你填各种信息，你只要保证邮箱是你的即可，其他信息可以瞎填。
    2. 如果你已经在其他地方购买了代理服务器，则可跳过此步骤
    总之你需要有一个服务器地址才行。买了服务器地址，才能配置客户端

2. 下载客户端
    1. 还是刚才的网站，通过顶部通知的第一个链接进入[Trojan 服务客户端设置教程索引
页面](https://portal.shadowsocks.nz/knowledgebase/151/)
    2. 该网站有完整的教程：https://portal.shadowsocks.nz/knowledgebase 。你可以找自己喜欢的客户端下载并按教程自己使用。下面我只介绍 Trojan-Qt5 的用法。
    3. 依次进行下面的操作，你试了两边之后还是没有成功，可以找我要视频教程。
        1. 下载客户端：点击它给出的[链接](https://dl.trojan-cdn.com/trojan/)，找到 Trojan-Qt5-Windows.[版本号].zip 下载，解压到 trojan-xxx 目录，然后把 trojan-xxx 目录放到 D:\Software\ 目录（可换成其他目录）里面，比如我的最终目录是 D:\Software\Trojan-Qt5-Windows-0.0.9\
        2. 双击 trojan-qt5.exe 打开软件，系统托盘里会出现一个小木马图标。
        2. 右键点击屏幕右下角托盘里的小木马图标，然后找到「服务器订阅」 =>「服务器订阅设置」，在里面添加你购买的服务器订阅地址。
            1. 你如果不知道订阅地址在哪，可以在购买服务器的网站找到「产品服务」，然后点击你购买的产品详情，再在里面找到「服务订阅」按钮。
        3. 右键点击屏幕右下角托盘里的小木马图标，然后找到「服务器订阅」 =>「更新trojan服务器订阅」以及「更新trojan服务器订阅(不通过代理)」按钮，分别点击之
        4. 等待1分钟后，右键点击屏幕右下角托盘里的小木马图标，然后找到「服务器」，就应该可以看见一大片 IP 列表了
        5. 双击小木马，点击菜单栏里的「设置 -> 常规设置 -> 入站设置」，将 socks5 端口号改为 1080（如果已经是 1080 就不用再改）
        5. 双击小木马，看到 IP 列表，随便选择一个 IP，右键，连接。
            1. 如果状态变成 Connected 就说明连接成功了。
            2. 如果连接失败，可能是 1080 端口被占用，解决方法见[这里](https://github.com/sun-shadow/Surf_the_Internet/blob/master/%E7%AB%AF%E5%8F%A3%E5%8D%A0%E7%94%A8%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95.md)。但也可能是 IP 被墙，那么就换个 IP 连接试试。
        6. 然后右键点击屏幕右下角托盘里的小木马图标，然后点击「打开 Trojan」
        7. 右键点击屏幕右下角托盘里的小木马图标，点击「系统代理模式」=>「PAC模式」或者「全局模式」
    4. 如果你用 trojan 发现还是不能翻墙，可以看看 SS 网站提供的其他翻墙客户端，比如 Clash 或者 Clashy，都可以试试。

3. 测试
    1. 尝试用 Chrome 访问 facebook.com，看看是否能连接成功吧！
    2. 如果失败了，换一个 ip 重新试试，有些 ip 能用，有些 ip 不能用。
    2. 如果又失败了，就试着把上面的步骤全部重新做一遍
    3. 如果还是失败了，找老师帮你远程解决一下吧~
4. 自动连接
    1. 双击小木马，找到刚刚连接成功的 IP，右键，编辑。
    2. 勾选「程序启动时自动连接」，搞定。以后软件一启动就会连接这个 IP 了。
    
### 总结

这种全局 FQ 的方式有一个缺点，那就是访问国外的网站变快，但是国内的网站缺巨慢无比，怎么解决了？

1. 使用 PAC 模式，但 PAC 模式也有缺点，比如自定义翻墙白名单不方便。

请确保你的小飞机全局翻墙可以用了，再阅读本教程！

如果你看不到本教程中的图片，说明图片被墙了。你需要开起全局翻墙（或者将 raw.githubusercontent.com 加入翻墙白名单）才能观看图片。

---

## 插件篇

Chrome 插件配合 Shadowsocks 使用，效果奇佳。

1. 打开 SS 代理（比如 Trojan），开启全局模式
2. 安装插件 [Chrome 插件下载地址](https://chrome.google.com/webstore/detail/padekgcemlokbadohgkifijomclgjgif)
3. 下载备份文件：[OmegaOptions-2020.zip](https://raw.githubusercontent.com/sun-shadow/Surf_the_Internet/master/OmegaOptions.bak.zip) (29.7 KB) 并解压，里面有一个 OmegaOptions.bak 文件
4. 在 Chrome 的插件栏右键点击图标，然后选择「选项」

![image](https://user-images.githubusercontent.com/59866634/72330854-2d680a00-36f2-11ea-96ae-5317075310ed.png)


5. 在弹出的配置页面里点击「导入/导出」，点击「从备份文件恢复」，选中刚刚下载的OmegaOptions.bak，最后点击左边的「应用选项」即可。一定要点击「应用选项」！

![image](https://user-images.githubusercontent.com/59866634/72330908-483a7e80-36f2-11ea-976c-73e21f8447b7.png)


6. 回到浏览器页面，选中插件的「自动切换（auto switch）」模式。

![image](https://user-images.githubusercontent.com/59866634/72330958-5d171200-36f2-11ea-9836-1192b0cef630.png)


7. 设置好自动切换后，就可以禁用 SS 的全局代理了（改为直连模式，这样国内的网站就变快了），在浏览器访问 https://twitter.com/ 看看是不是能用了，同时 https://www.baidu.com/ 是不是很快了。


![image](https://user-images.githubusercontent.com/59866634/72331022-6f914b80-36f2-11ea-8171-273f8aadc9ec.png)

8. 如果遇到一些网站被墙，通过浏览器插件的「添加条件」按钮把网站域名添加进去就可以自动 FQ 了。这个插件还有一个牛逼的地方，假设你下载某个外国的文件速度很慢，只需要点击「添加条件」按钮，添加对应的域名，然后重新下载，速度就飞起来了。

![image](https://user-images.githubusercontent.com/59866634/72331056-8041c180-36f2-11ea-852b-78d11ddb8cf8.png)

## Mac 命令行篇

### 方法一：为终端设置代理相关环境变量

`http_proxy`、`https_proxy` 和 `all_proxy` 三个环境变量用于设置命令终端的代理。  
在`~/.zshrc`中输入如下内容 (注意端口号要与代理客户端提供服务的端口号保持一致)。

```shell
alias setproxy="export all_proxy=socks5://127.0.0.1:51837"
alias unsetproxy="unset all_proxy;unset http_proxy;unset https_proxy"
alias proxyon=setproxy
alias proxyoff=unsetproxy
```

### 方法二：proxychains-ng
1. 安装 homebrew 

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```
2. 安装 proxychains-ng

```
brew install proxychains-ng
```

3. 配置proxychains-ng
  - 安装 proxychains-ng
  - 下载配置文件（如果下面的命令执行失败，那你就自己下载[proxychains.conf](https://raw.githubusercontent.com/FrankFang/dot-files/master/proxychains.conf)，然后将其移动到 ~/.proxychains.conf）
  - 注意修改配置文件中的端口号，和本地代理客户端提供 socks 服务的端口号保持一致。
    ```
    curl -L https://raw.githubusercontent.com/FrankFang/dot-files/master/proxychains.conf > ~/.proxychains.conf
    ```
  - 添加 bash alias，运行 
    ```
    touch ~/.bashrc; echo 'alias pc="proxychains4 -f ~/.proxychains.conf"' >> ~/.bashrc
    ```
  - `source ~/.bashrc`
  - pc git clone xxx 或者  pc brew install xxx --verbose，那么这个命令行就是翻墙的。
  
    注意：`pc git clone <仓库地址>` 仓库地址可以用 https 协议，但无法使用 SSH 的方式。
