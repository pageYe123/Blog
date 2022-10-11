# [Mac 目录结构](https://github.com/yeshiqing/Blog/issues/4)

![Mac 目录结构](https://raw.githubusercontent.com/yeshiqing/diagrams/main/simple/mac-directory-structure.svg)

![tree 命令查看 Mac 根目录](https://wx4.sinaimg.cn/mw690/6cdfff77gy1h47rkzle9kj207e08yaai.jpg)

<!-- tree -L 3 -d -I "Applications|Library|Users|System|Volumes|cores|opt|share|lib|libexec|fd|etc|Caskroom|Cellar|Frameworks|Homebrew|Watchdata|include|authserver|iRATBW.mlmodelc|standalone|tmp|tftpboot|private|home" -->

`/sbin`是 Super User Binaries（超级用户二进制文件）的缩写，这里存放的是系统管理员使用的系统管理程序。如 ifconfig 命令。

`/usr`发音是user，但它是 Unix System Resources（Unix 系统资源） 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。

- `/usr/local/bin`是给用户放置自己的可执行程序的地方，**用户写的脚本放在这里**。
- `/usr/bin`都是系统预装的可执行程序，会随着系统升级而改变。系统用户使用的应用程序。
- `/usr/sbin`是系统管理员用于存放供系统启动后使用的不重要的系统使用工具。

`/bin`是 Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令。比如 cp、mkdir、mv、rm、cat、echo、kill、ln、chmod、date、ls、ps 命令。

`/var`目录存放数据文件，如程序数据、日志等，我们习惯将那些经常被修改的目录放在这个目录下。

`/dev`存放设备文件和设备驱动。可以将`/dev/null`（空设备文件）看作"黑洞"，等价于一个只写文件，所有写入它的内容都会永远丢失。

- 字符设备（c），是一种以字符（也称为字节或位）传输数据的设备，例如鼠标，扬声器等。
- 块设备（b），它们是按数据块（例如 USB，硬盘等）传输数据的设备。

`/Library/Apple/usr/bin`macOS上的特殊目录，只放了一个名为 rvictl 的程序，是随着 XCode 一起被安装的。

参考资料：

- LearnKu Server 社区：[Linux 目录结构概览](https://learnku.com/server/wikis/36491)