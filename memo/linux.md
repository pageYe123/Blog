## Linux

Linux 操作系统一般有4个主要部分：内核、shell、文件系统和应用程序。内核、shell和文件系统一起形成了基本的操作系统结构。Linux 操作系统的内核大部分是用 C 语言写的。

Linux 系统是大小写敏感的，而 Windows 系统和 Mac 系统正好相反，大小写不敏感。

注意：任何系统的 **Shell 变量都是大小写敏感。**

## Linux 命令

命令行四要素：

- 可执行程序（Executable programs）
- 参数（arguments）
- 环境变量（Environment variable）
- 工作目录（Working directory）

### 一、参数

#### 双引号、单引号与反引号

*双引号解析变量，单引号不解析变量，反引号解析命令。*

```shell
# 替换变量
"$HOME/Library/Application Support/abnerworks.Typora"
# 不替换变量
echo '$HOME'
```

如果想传入单引号，可以

```shell
# 1. 套层双引号
"'I am a boy.'"
# 2. 使用反斜杠转义
\'I am a boy.\'
```

引号的嵌套。

双引号嵌套直接用转义字符`\"`就可以。有些参数只能用双引号包裹，否则报错，比如`"iTerm2"`。

```shell
osascript -e "tell application \"iTerm2\" to set miniaturized of every window to true"
```

单双引号交替嵌套是可以的。

```shell
alias e='echo 123;node -e "console.log(456)"'
```

单引号嵌套，不能简单地用转义字符`\'`实现，因为 bash 的 line 扫描算法*遇到下一个单引号就会和上一个直接配对*，没有贪婪扫描。要用`'\''`来替换内部的单引号，末尾的`'`改成`\'`和前面的进行配对。（不到万不得已不要使用，因为很丑不便阅读）

```shell
# 单引号中嵌套双引号
alias e='echo 123;node -e "console.log(456)"'
# 即使使用转义字符，单引号嵌套单引号报错
alias e='echo 123;node -e \'console.log(456)\''
# 正确的单引号嵌套方法
alias e='echo 123;node -e '\''console.log(456)'\'
```

#### 正则表达式

`*`匹配目录下所有文件：`~/Desktop/test/*`。

**举例**

将中的所有文件移到当前目录：

`mv ~/Desktop/test/* ./`

### 二、环境变量

#### 进程与环境变量：

每个进程在启动时从环境中捕获一些变量，并和这些环境变量绑定。

进程树的概念。每个进程都是由父进程创建，进程会从父进程中继承环境变量。

#### 【增】新增环境变量：

- 设置环境变量只对当前命令有效。命令行终端中，迅速传递一个环境变量 AAA 给`java Main.java`命令：
    `AAA=321 java Main.java`
    
- 设置环境变量只对当前窗口有效。命令行终端中，`export BBB=123`。重新打开的窗口不会读取到此变量。


#### 局部环境变量和全局环境变量：

上述设置的环境变量都可看做局部环境变量。

如果想设置全局环境变量，需要向 Shell 配置文件中，写入`export`语句。如果使用 Bash，修改`~/.bash_profile`文件；如果使用 Zsh，修改`~/.zshrc`文件。

- 比如在`~/.zshrc`中加入如下内容，设置全局环境变量`https_proxy`：

  ```shell
  export https_proxy=$http_proxy
  # 注意：不写 export 的环境变量，仅可用于当前 Shell，子 Shell 默认读取不到父 Shell 定义的变量。
  foo=222 # 子 Shell 读取不到该变量
  ```

- 再以`PATH`为例，如果想修改这个环境变量，就在 Shell 配置文件中写入

  ```shell
  export PATH=/opt/apache-maven-3.8.5/bin:$PATH
  ```
  表示在原有的 `$PATH` 基础上新增`/opt/apache-maven-3.8.5/bin`，每个`PATH` 之间以`:`分割。

- 注意 `PATH` 只是众多环境变量中的一个。

#### 【删】删除环境变量：

```shell
unset 变量名
```

#### 【查】读取环境变量的值：

`$AAA`读取变量 AAA 的值。

`echo $AAA`读取变量 AAA 的值，并将结果输出到终端。

`export`命令列出当前环境所有环境变量及其值。这会可以看到`HOME=/Users/jeffrey`。

`printenv`、`env` 和`export`类似，能打印出所有环境变量及其值。（？？和 export 的区别是什么？？）

#### 验证是否为环境变量

`printenv` 命令检验该变量是否为环境变量。`printenv HOME`有返回值，代表 HOME 是环境变量；如果没有输出返回值，意味着该变量不是环境变量。

#### 与代理相关的环境变量

终端中使用代理。系统代理在 shell session 中是不会生效的，在终端中使用代理，需要手动提供 `http_proxy`、`https_proxy` (或 `all_proxy`) 环境变量，Shell 中很多可执行程序会自动识别这三个环境变量，然后决定是否走代理。

注意大小写问题，比如`curl` 除了`http_proxy`仅支持小写外，其他两个既支持大写也支持小写(参见`man curl`)，统一写成小写合适。

#### Shell 内置环境变量

【学习资料】gnu.org 官方 bash 手册：[bash 内部变量](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html)

- HOME

`echo $HOME`显示家目录`/Users/jeffrey`

- PS1

主提示符（the primary prompt）。这个变量用于设置提示符左边的样式。

bash 默认是`PS1='\h:\W \u\$ '`

zsh 默认是`PS1='${ret_status} %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'`

发现的来源是`/etc/bashrc`代码：

```shell
# System-wide .bashrc file for interactive bash(1) shells.
if [ -z "$PS1" ]; then
   return
fi

PS1='\h:\W \u\$ '
# Make bash check its window size after a process completes
shopt -s checkwinsize

[ -r "/etc/bashrc_$TERM_PROGRAM" ] && . "/etc/bashrc_$TERM_PROGRAM"
```

#### 需要注意

- `alias setproxy='export ALL_PROXY=socks5://127.0.0.1:51837'` alias 只是命令别名，不是环境变量，无法通过`$setproxy`取到值。只能用`alias proxyon=setproxy`。

#### 其他

- StackExchange 的一个有趣问题：[Where is the $HOME environment variable set?](https://superuser.com/questions/271925/where-is-the-home-environment-variable-set)
    答案是：登录程序在你的 shell 上调用 exec 函数之前，会根据`/etc/passwd`中的值来安排它（把它包裹在 exec 的参数中）。如果`/etc/passwd`没有定义 $HOME，shell会帮助填进去，即使处于没有登录的状态。



### 三、工作目录

Linux中某些特殊目录的表示：
```
.  表示当前目录，也可用 ./表示
.. 表示上层目录，也可用../表示
-  表示上次工作目录
~  表示当前工作用户的家目录
```

比如：`cd -` 返回上一次访问的目录，等同于`cd $OLDPWD`。

### 四、可执行程序

- 去那里找？

    - Windows：PATH 环境变量、当前目录下文件扩展名为 exe、bat、com 的文件。
    - Mac：PATH 环境变量。（若要执行当前目录下的可执行文件，需加相对目录，如`./main`，单独输入`main`会报错“command not found”）

- 其他

    - 执行一个命令，就是运行一个可执行程序，就会创建一个进程。命令执行完毕，进程自动结束。

    - Mac 中的文件夹具有可执行权限

        - Mac 中`mkdir`命令创建的文件夹，默认是具有可执行权限的。如果`chmod -x <directory>`将一个文件夹的可执行权限删除，当前用户将无法进入该目录，也无法从图形界面打开文件夹。
    
        ```shell
        cd: permission denied: my-directory
        ```
    
        参考 stack overflow：[cd into directory without having permission](https://stackoverflow.com/questions/8221820/cd-into-directory-without-having-permission)

#### 【注明】以下所有程序适配于 Mac，在 Linux 中可能有所不同。

#### set 命令

`set` 命令显示包括环境变量与 Shell 变量在内的所有变量以及 Shell 函数的列表（比`export`、`env`命令多了函数）。

#### 管道命令

`bash 命令 | 管道命令`

- 管道命令能够接收标准输出，例如`grep`，`less`，`head`，`tail`等命令。

- 非管道命令不能接收标准输出，例如：`echo`，使用`xargs`命令实现管道功能：`echo 111 | xargs echo`

对于管道命令，Shell 会先调整命令执行顺序，先创建管道命令的进程，然后再执行前面的命令。

```shell
ps > log | grep xxx
# shell 先执行 grep xxx，之后执行 ps > log
# 所以才会多出来 grep xxx 这条命令。
# 这点 iTerm 和 terminal 有差异，terminal 的 log 里面没有grep xxx
```

参考资料：

[Shell 多命令执行符与管道符“|”](https://blog.csdn.net/weixin_40277264/article/details/118606674)

#### file 命令

查看文件类型，可以判断是否为二进制文件。

```shell
file HelloWorld.o
# 返回：HelloWorld.o: Mach-O 64-bit object x86_64
# Mach-O 文件就是一个二进制文件。
file HelloWorld.c
# 返回：HelloWorld.c: c program text, ASCII text
```



#### mkdir 命令

`-p`参数，两个作用。①如果没有父级目录自动创建父级目录。②如果目录已经存在则不进行创建也不会报错。

#### echo 命令

`echo $?`检验上个命令是否成功执行。返回值是0，代表成功；如果是非零，代表失败。
有时候一个命令（如`mkdir foo`）执行完，终端控制台没有任何输出。到底执行成功了没有？此时可以用`echo $?`检验。

#### 查看系统可用 shell

`cat /etc/shells`查看系统可使用的 shell。

#### 查看本机 ip 地址

`ifconfig en0`其中的 inet 就是本机 IP 地址

#### bc 命令

bc命令是一种支持任意精度的交互执行的计算器语言。非交互模式必须搭配管道符合 `echo` 使用。

```shell
echo "15+5" | bc
```

#### expr 命令

手工命令行计数器，仅限整数值。Linux 下支持`length`、`substr`参数，Mac 下不支持。

```shell
# expr 中的加减乘除运算必须加空格，否则语法报错
expr 10 + 10
# 使用乘号时，必须用反斜线屏蔽其特定含义。因为shell可能会误解显示星号的意义
expr 30 \* 3
```



#### wc 命令统计文件的字节数、字数、行数

`-l` 统计行数

示例:

- 统计git变更文件的数量：

```shell
git status -s | wc -l
```

- 统计当前文件夹目录下有多少个文件(不含子目录)

之所以减 1，是因为第一行显示的是 total 值。注意减号后面必须有空格，否则语法错误。

```shell
expr `ls -l | wc -l` - 1
expr $(ls -l | wc -l) - 1
# 或者
echo `ls -l | wc -l` - 1 | bc
echo $(ls -l | wc -l) - 1 | bc
# 或者
ls -l | grep "^[-|d]" | wc -l
```

- 统计当前目录下文件的个数（包括子目录）

```shell
ls -lR | grep "^[-|d]" | wc -l
```

​    

#### date命令得到当前时间

```shell
date +"%Y-%m-%d %H:%M:%S"
# 返回值格式示例：2019-07-06 16:32:22
```

#### awk 命令

通过 git log 统计代码行数

```shell
git log --pretty=tformat: --numstat | head -n 100 | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }'
# 输出结果：added lines: 38861, removed lines: 22916, total lines: 15945
# 原文链接：https://blog.csdn.net/cyf15238622067/article/details/82980782
```



#### grep命令

`-e`正则表达式。

`-o`只打印出匹配项。

```shell
# 可以连续使用多个，匹配多个模式
echo this is a text line | grep -e "is" -e "line" -o
# 输出
# is
# is
# line
```

`-E` 强制使`grep`命令变为`egrep`命令，支持扩展的正则表达式。可参考：[linux里grep和egrep的区别](https://blog.51cto.com/mcmvp/1180951)

```shell
echo this is a test line. | grep -o -E "[a-z]+\."
# 输出 line.
# `+` 匹配一个或多个先前的字符。`grep -e` 是不支持的。
```



##### grep搭配xargs：过滤出指定文件，并删除

```shell
ls -al | grep -oe ".gclient.*" | xargs rm -rf
```

`xargs`将过滤出的文件名作为参数传递给`rm`命令。

另一个经典用例，文件很多的时候

```shell
git status | grep -E "deleted by us:" | grep -o "src/.*" | xargs git rm
```



##### grep搭配xargs：过滤出含有指定文件内容文件，并对文件做出操作

```shell
# 测试文件：
echo "aaa" > file1
echo "bbb" > file2
echo "aaa" > file3

grep "aaa" file* -lZ | tr '\n' '\0'|xargs -0 rm
等价于 
grep "aaa" file* -l | xargs rm
# 执行后会删除file1和file3。
# grep输出用-Z选项来指定以0值字节作为终结符文件名（\0），-Z通常和-l结合使用，-l列出文件内容符合指定的范本样式的文件名称，没有-l则显示"文件名：文件内容"，有-l则只显示文件名。
# tr命令把换行符转换成特殊字符 \0。
# xargs 会把换行符、空格、制表符、EOF等符号做为分隔符，把输入的内容切分为一个数组，并把数组中每一个元素作为参数，放到后面的命令中执行。根据文档所述，xargs -0参数会把分隔符指定为 \0，从而避免了文件名中含有空格的影响。如果文件名带有空格，比如 hello world 就会被 xargs 截断为两个参数。xargs -0 读取输入并用0值字节终结符分隔文件名，然后删除匹配文件，。

find . -type f -print0 | xargs -0 rm
# find 命令在指定目录下查找文件，无法查找指定内容的文件。参数 -print0 选项，相当于tr '\n' '\0'|xargs -0，写法更简单。
```



#### nohup 命令：关掉终端，作业仍保持运行

参考资料：[linux后台执行命令：&和nohup](https://blog.csdn.net/liuyanfeier/article/details/62422742)

#### 将挂起进程恢复到前台

`ctrl+c` 是强制中断程序的执行。 `ctrl+z` 是将任务挂起，此时作业仍然在进程中并没有结束，它只是维持挂起的状态在后台运行。

`ctrl+z`是把进程挂起，所有进程都会这样，而不仅是vim。这时候可以通过`jobs`命令查看当前shell有多少个挂起的进程。使用`fg`命令使得作业在前台运行。

```shell
➜ jobs
[1]  + suspended  vim test.sh
[2]  + suspended  vim test.txt
```

前面会有个序号，如果想把哪个进程恢复到前台，用`fg %<序号>`即可，如`fg %1`把第一项作业恢复到前台。

#### 删除文件或文件夹命令

`rm -rf <文件夹路径>` 

`rm -rf *` 删除当前文件夹内所有内容

#### kill 命令

发送信号到进程。

强制终止进程：

```shell
kill -9 <进程id> # 根据 PID 杀掉某个进程
```



#### ps 命令

作用：报告系统的进程状态。

基础概念：UID 是用户 id，PID 是进程 id，PPID 是父进程 id。
-e：显示所有程序
-f：显示 UID,PPID,C 与 STIME 信息项。

查找指定进程 id 的进程信息：

```shell
ps -ef | grep -e <进程id>

# 显示首行
ps -ef | grep -e UID -e <进程id>
```

#### lsof 命令

lsof命令 用于查看进程打开的文件、打开文件的进程、进程占用的端口(TCP、UDP)，找回/恢复删除的文件。

`-p`根据进程id，查看指定进程所打开的文件：

```shell
lsof -p <进程id>
```

根据进程id，查看监听的端口号：

```shell
# 只列出 NODE 为 TCP 的进程
# -P 表示显示端口号，在最后的 NAME 列显示端口号。不写则显示http-alt或ddi-tcp-1
lsof -P -p <进程id> | grep LISTEN
```

`-i`根据端口号，查看监听端口的进程id：

- 方法一：

```shell
lsof -P -i:8080 | grep LISTEN
# 只针对 NODE 为 TCP 节点的进程有效
# 等价于
lsof -P -i tcp:8080 | grep LISTEN
lsof -P -iTCP:8080 | grep LISTEN
```

- 方法二：

```shell
# -n 禁止 IP 地址转变为host文件中对应的域名。127.0.0.1 会显示成 localhost
# -iTCP 只显示 NODE 为 TCP 的进程信息
# -sTCP:LISTEN 只显示监听状态为 LISTEN 的 TCP 的进程信息
lsof -nP -iTCP:8080 -sTCP:LISTEN
```

返回值：

```shell
COMMAND   PID    USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node    34773 jeffrey   23u  IPv4 0x75342ef2b2f3d915      0t0  TCP *:8080 (LISTEN)
```

其中 NAME `*:8080`表示监听 8080 端口，本身不占用IP地址？？

Mac「活动监视器」中的端口并不是 TCP 端口号，而是 Mac 内部使用的端口。

#### curl 命令

`curl -v http://www.google.com`查看执行日志

`curl -x http://127.0.0.1:7890 www.google.com` 为 curl 命令添加代理功能

`curl -X POST http://127.0.0.1:8888` 设置请求动词，搭配`-d`参数使用。

`curl -d '数据体'`用于发送 POST 请求的数据体。使用`-d`参数以后，HTTP 请求会自动加上标头`Content-Type : application/x-www-form-urlencoded`。并且会自动将请求转为 POST 方法，因此可以省略`-X POST`。

`curl -H 'key: value'`设置请求头

`curl www.google.com`向 URL 发送 get 请求，经常用于测试一台服务器是否可以访问一个网站

`curl -I http://www.google.com` 向服务器发出 HEAD 请求 (获取资源的元数据)，HEAD 方法请求一个与 GET 请求的响应相同的响应，但没有响应体，等同于`curl --head`

`curl -k https://www.google.com` `-k`参数跳过 SSL 检测，不会检查服务器的 SSL 证书是否正确。

参考资料：

[curl 的用法指南](https://www.ruanyifeng.com/blog/2019/09/curl-reference.html)

[curl 命令详解](http://aiezu.com/article/linux_curl_command.html)

#### sed 命令

配合正则表达式使用，是文本处理中非常重要的工具。它把当前处理的行存储在临时缓冲区中，接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。

- 替换文本中的字符串

```shell
# 命令格式
sed [options] 'sed 命令' file(s)

# sed 命令 s 替换指定字符
sed 's/原始字符/替换成的目标字符' filename
## 举例：
sed 's/51837/51833/' ~/.gitconfig
```

需求：批量修改`~/.gitconfig`中重复的部分`proxy = socks5://127.0.0.1:51837`。`~/.gitconfig`文件内无法设置变量，http.proxy和https.proxy每次都要改两遍，很麻烦。

**需要注意：文件内容并没有改变，除非你使用重定向存储输出，只可以追加，不能覆盖。**

```shell
sed 's/51837/51833/' ~/.gitconfig >> ~/.gitconfig
```

怎么才能直接覆盖呢？`-i`选项加`''`表示没有用于保存备份的 extension。Mac系统需要加空字符串，否则报错，Linux 系统则不需要。

```shell
sed -i '' 's/51837/51833/' ~/.gitconfig
```



#### tree 命令

列表选项：

`tree -L 3` 列出层级深度为3

`tree -d` 仅列出文件夹

`tree -I 'Applications|Library|Users'` 不列出匹配的文件

`tree -F` 格式化，目录后面添加'/'，文件中的空格用'_'代替等等，就像`ls -F` 所展现的那样。

#### mv命令

1）移动文件到指定目录：`mv <filepath> <dest-directory>`

**Mac 系统中：**

如果移动的是文件，而目标目录中有同名文件，默认会覆盖，即执行`-f`参数。

如果移动的是文件夹，如果有同名的空文件夹，则覆盖；如果目录中有同名的非空文件夹则报错：

> mv: rename foo to /Users/jeffrey/Desktop/foo: Directory not empty

如果移动的是文件，而目标目录中有同名文件夹，则报错：

> mv: rename foo to /Users/jeffrey/Desktop/foo: Is a directory

如果移动的是文件夹，而目标目录中有同名文件，则报错：

> mv: rename foo to /Users/jeffrey/Desktop/foo: Not a directory

**Linux 系统（Mac无此参数）：**

-t，--target-directory=DIRECTORY move all SOURCE arguments into DIRECTORY，即指定 mv 的目标目录，该选项适用于移动多个源文件到一个目录的情况，此时目标目录在前，源文件在后。

It is an error for the source operand to specify a directory if the target exists and is not a directory.

2）文件重命名：`mv <source-file> <dest-file>`

#### ln 命令：硬链接和软链接

```shell
# 注意前后顺序
ln f1 f2          # 创建f1的一个硬连接文件f2
ln -s f1 f3       # 创建f1的一个符号连接文件f3
ln -s f1 fooDirectory # 如果 fooDirecotry 是已存在的文件夹，该命令会在其中创建一个同名(名为f1)软连接文件

# 删除硬链接
unlink f2    # 等同于 rm -rf f2
```

**软连接实际上指向目标文件的路径名，软链接和目标文件有着不同的 inode 号**，说明软链接是一个**独立的文件**。可以理解为软链接存放的是目标文件的路径，包括目标文件的名称。

软链接文件的大小就是它所指向文件的名字的大小。比如`fun-sym -> fun`，文件大小就是字符串“fun”的长度3。

软链接弥补硬链接的两个不足：

- 硬链接不能跨物理设备
- <u>硬链接不能引用目录只能引用文件</u>

硬链接是通过 inode 引用另外一个文件，因此硬链接和目标文件具有相同的 inode 号，当我们创建一个硬链接时，硬链接数就会增加。**硬链接数表示有多少个文件的名字和该文件的inode产生映射关系。**

在 Linux 的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)

创建一个目录文件（文件夹），发现它的硬链接数为2。因为里面有`.`当前目录和`..`上级目录

<img src="https://wx4.sinaimg.cn/large/6cdfff77gy1h4bdbg7ghej20bj0cngnd.jpg" alt="image-20220704205352894" style="zoom: 50%;" />

<img src="https://wx4.sinaimg.cn/large/6cdfff77gy1h4bdbg47blj20cc0d2tbm.jpg" alt="image-20220704205413676" style="zoom:50%;" />

如何查看文件是否为硬链接？看两个文件是否有相同的 inode 号。

- 用`ls -i`命令查看文件的 inode 号，第一列数字就是。
- 用`find <path> -inum <inode号>` 在指定路径中搜寻有同样的 inode 号的文件。



软链接和硬链接的共同用途：

- 修改链接之后源文件也会自动同步。
    比如项目中共用一个`ysq-jquery.js`文件，目录结构要求两个地方都放。如果是复制粘贴的话，为保持二者同步，改一个文件，还得手动再改另一个文件。换成硬链接，两文件自动同步，改一个另一个自动改。

硬链接的优点、软链接的缺点：

- 允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”。

- 删除源文件，硬链接仍可以正常访问。但是软链接删除源文件，软链接就无法正常访问了。

- 硬链接的源文件移到其他目录，并不会影响硬链接的正常访问。而软链接文件包含有原文件的路径信息，所以当源文件从一个目录移到其他目录中，再访问链接文件，系统就找不到了。

硬链接的缺点、软链接的优点：

- 支持引用目录。硬链接不支持引用目录。

疑惑和思考：

从具体应用场景来看，什么时候使用硬链接，什么时候使用软连接？我举不出来例子。



### 五、命令行终端其他

- 反引号\`与`$()`相同
    作用：反引号括起来的字符串被 Shell 解释为命令。在执行时，Shell首先执行该命令，并以它的标准输出取代整个反引号包围的部分。效果同`$() `

    ```shell
    # 计算当前目录下有多少文件及文件夹，不含子目录
    expr `ls -l | wc -l` - 1
    ```

    ```shell
    function echo_local_variable() {
        local PORT=$1
        if [ -z "$1" ]; then
          PORT=51837
        fi
        # 这样写是打印出变量值
        echo $PORT
        # 这样写是打印出 PORT 对应的命令
        echo $(PORT)
    }
    ```
    
    
    
- 命令之间的`;`、`&&`和`||`

    `;`（分号）：顺序地独立执行各条命令，彼此之间不关心是否失败，所有命令都会执行。

    `&&`：顺序执行各条命令，只有当前一个执行成功时候，才执行后面的。

    `||`： 顺序执行各条命令， 只有当前面一个执行失败的时候，才执行后面的。

- Shell 历史展开 (histroy expand)

    - 命令。

        - `!-n`表示倒数第几条命令，如`!-1`表示上一条命令。

        - `!!`代表了上一条执行的命令，是`!-1`的快捷方式。用于扩展上一条命令。如果是基于上条命令扩充，`!!`就来得比 up 键方便（比如后面追加其他参数或者前面追加命令）。

            参考资料：[命令行中感叹号的应用](https://cloud.tencent.com/developer/news/387476)

    - 参数。`:` 用于分割命令部分与参数部分

        - `!!:0`表示上个命令的第0个参数，即可执行程序。
        - `!!:n`表示上个命令的第n个参数。
        - `!!:$`表示上一命令的最后一个参数，可进一步简写为`!$`
        - 参考资料：[获取shell命令行上一次命令的参数](https://blog.csdn.net/howeres/article/details/122907944)

- 让命令在后台运行。当在前台运行某个作业时，终端被该作业占据，可以在命令后面加上`&`实现后台运行。例如：`sh test.sh &`
    适合在后台运行的命令有 find、费时的排序。需要用户交互的命令不要放在后台执行。

    后台运行的作业同样会将结果输出到屏幕上，干扰你的工作，如果放在后台运行的作业会产生大量的输出，最好使用下面的方法把它的输出重定向到某个文件中：

```
command > log 2>&1 & 
```

command > log 是将 command 的输出重定向到 log 文件，即输出内容不打印到屏幕上，而是输出到 log 文件中。
2>&1 是将标准错误重定向到标准输出，这里的标准输出已经重定向到了 log 文件，即将标准错误也输出到 log 文件中。最后一个`&`，是让该命令在后台执行。
试想 2>1 代表什么，2与>结合代表错误重定向，而 1 则代表错误重定向到一个文件 1，而不代表标准输出；换成 2>&1，&与1结合就代表标准输出了，就变成错误重定向到标准输出。

当使用 & 将一个进程放置到后台运行的时候，Shell 会提示这个进程的进程 ID，我们可以使用进程 ID 来终止对应的进程。

- 重定向

stdout 标准输出，文件描述符1；stderr 标准错误，文件描述符2。

```shell
# 覆盖文件。
java Main > log # 将标准输出放到文件中，标准错误原样输出到终端。
java Main 2 > log # 仅将标准错误输出到终端。

# 追加文件
java Main >> log

# 多次重定向，标准输出和标准错误都输出到文件
java Main > log 2>&1

# 垃圾桶，标准错误将被吞掉，
java Main 2 > /dev/null
```





## Shell 脚本编程

### `$(command)`和`$variable`的区别

`$variable`获取变量值，`$(command)`把圆括号中的内容当做命令处理。

```shell
alias cdtmp="cd $HOME/dev-temp" # 这叫定义命令的别名，cdtmp 是一个命令。
cdtmp=111 # 这叫定义变量，cdtmp 是一个变量。

alias cdtemp=$(cdtmp) # 等同于 alias cdtemp=cdtmp，涉及参数时就体现出作用
alias cdtemp=$(cdtmp parameter1 parameter2) # 圆括号里面的内容当做命令来执行

$foo # 获取变量 foo 的值
echo $foo # 获取变量的值并打印出来
```

### `${variable}`变量替换

比`$variable`更加高级

```shell
WORDCHARS="*?_-.[]~=/&;!#$%^(){}<>"
WORDCHARS="${WORDCHARS/_\/$/}" # 删除掉字符串中的 _ / $ 三个字符
```



### source 命令

用于在脚本内部加载外部库，使得外部库定义的函数可以再脚本中使用。比如名为 `lib1.sh` 的外部库。

```shell
#!/usr/bin/env bash
function echo1(){
		echo 1111
}
```

在脚本中以下内容会输出`1111`。

```shell
source lib1.sh

# 输出 1111
echo1
```

如果是在`~/.zshrc`、`~/.bash_profile`等 Shell 的配置文件(这些文件本质也是 Bash 脚本)中，通过`source`命令引入外部库，那么外部库中的函数可直接在终端当命令使用，而不必在环境变量中添加命令。



### 输出的字符串带颜色

```shell
function upgrade_oh_my_zsh() {
  echo >&2 "${fg[yellow]}Note: \`$0\` is deprecated. Use \`omz update\` instead.$reset_color"
  omz update
}
```

### 脚本参数

- **传递到当前脚本的参数个数：**

```shell
$#
```

- **在本脚本中执行其他脚本**：

```shell
PYTHONDONTWRITEBYTECODE=1 exec vpython "$base_dir/gclient.py" "$@"
```

设置临时环境变量`PYTHONDONTWRITEBYTECODE`，执行(`exec`)可执行程序`vpython`(已预先加入环境变量)，传递了至少一个参数`"$base_dir/gclient.py"`，以及当前脚本接受到的参数。

- **脚本内获取参数：**
    `$1` 为执行脚本的第一个参数，`$2` 为执行脚本的第二个参数，以此类推。第10个参数用`${10}`的形式引用。

- **传递给函数或脚本的所有参数：**
    `$*` 和 `$@` 都表示传递给函数或脚本的所有参数。当 `$*` 和 `$@` 不被双引号`""`包围时，它们之间没有任何区别，都是将接收到的每个参数看做一份数据，彼此之间以空格来分隔。
    但是当它们被双引号`""`包含时，就会有区别了：
    `"$*"`会<u>将所有的参数从整体上看做一份数据</u>，而不是把每个参数都看做一份数据。传了一整个字符串。
    `"$@"`仍然将每个参数都看作一份数据，<u>每个参数彼此之间是独立的</u>。
    比如如果传了5个参数，对于`"$*"`来说，会变成一个字符串；而对于`"$@"`来说，分别传了5个参数。

### 获取当前脚本的文件路径

```shell
$0
echo $0 # 输出 /Users/jeffrey/V8-research/depot_tools/gclient
```

### 获取当前脚本所在文件夹

```shell
$(dirname "$0") 
# 或者
$(dirname $0) # 输出 /Users/jeffrey/V8-research/depot_tools
```

### 不想执行后续脚本

```shell
exit
```

不要用`return`，`return`只能用于函数或 sourced script。

### if条件判断中：双中括号与单中括号的区别

- 双方括号搭配`==`运算符使用。

    `=`运算符应该与`[`配合使用。`==`运算符应该与`[[`配合使用，以进行模式匹配。

```shell
if [ "string1" = "string1" ]; then
  echo 'two string are equal'
fi

# 或者
if [[ "string1" = "string1" ]]; then
  echo 'two string are equal'
fi
```
- 有变量时要么使用双中括号，要么使用双引号

    如果变量`$1`的值为空，那么就 if 语句就变成了`if [ = 1 ]`，这不是一个合法的条件，此时加上双中括号就能避免。

```shell
if [[ $1 == 1 ]]; then
  echo "\$1 is 1"
  
# 或者是使用双引号
if [ "$1" == 1 ]; then
  echo "\$1 is 1"
```


- 括号中可以定义一些正则表达式来匹配字符串

```shell
if [[ $USER == j* ]]; then
  echo 'hello jeffrey'
fi
```



### 文件测试操作符

#### -d 判断文件夹是否存在

  ```Shell
#!/usr/bin/env bash
temp="/Users/jeffrey/IdeaProjects/tmp"
if [ ! -d "$temp" ]; then
    mkdir -p "$temp"
fi
  ```

`! -d "$temp"`表示如果文件夹不存在或是文件而不是文件夹。

注意 `"temp"`和`]`之间要有空格，否则报错。

mkdir 命令`-p`参数，如果没有父目录创建父目录。

Shell 脚本文件路径不识别`~`，需用绝对路径`/Users/jeffrey/`

#### -z 判断字符串是否为 null（零长度）

一个字符串没定义是 null。linux 认为字符串如果长度为零也是 null。

```shell
#! /usr/bin/env bash
String=''   # Zero-length ("null") string variable.

if [ -z "$String" ]; then
  echo "\$String is null."
else
  echo "\$String is NOT null."
fi     # $String is null.
```

### shell 脚本学习资料：

[Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/index.html)
