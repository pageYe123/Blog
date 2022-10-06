## .bash_profile 和 .bashrc 的区别

.bash_profile 和 .bashrc 都是系统环境初始化时启动的初始化脚本，为当前用户设置专属的环境信息和启动程序。二者的区别就是**在 login shell 中加载`~/.bash_profile`，而在 non-login shell 中加载`~/.bashrc`**

### login shell 与 non-login shell 的区别

用户每次使用 Shell，都会开启一个与 Shell 的 Session（对话）。Session 有两种类型：登录 Session 和非登录 Session，也可以叫做 login shell 和 non-login shell。

Linux 环境，输入用户名和密码，进入到终端（terminal），就是一个 login shell 环境。比如通过`ssh`远程进入到主机，此时执行`.bash_profile`。

Linux 环境，图形界面双击打开终端，没有输入用户名和密码，这时进入的就是 non-login shell环境，此时执行`.bashrc`。  
另外，手动新建的 shell 是 non-login shell，这时不会进行环境初始化。比如，在命令行执行`bash`命令，启动一个新的bash实例，就会新建一个 non-login shell，`.bashrc`也会执行。如果给`bash`命令加上`--login`参数，会开启 login shell，强制执行登录 Session 会执行的脚本，比如`~/.bash_profile`。  

有一种判断 login shell VS non-login shell 的非常快速的方法，使用命令 `echo $0`
- `-bash` 中 `-` 表示当前是一个 login shell
- `/bin/bash` 表示是 non-login shell

### 不同的终端可能有不同的行为

- macOS 的 terminal 就是个特例，它做了一些非标准的事情：每当新建一个窗口，总是进入 login shell 环境，这意味着`~/.bash_profile`会被执行。即使对 terminal 进行配置将 bash 作为 shell 也是如此（将配置中`shell的打开方式`，由`默认登录shell`改为`命令（完整的路径）`，并输入`/bin/bash`）。

- iterm2 的表现就很正常，当设置 shell 为 `/bin/bash` 时，默认进入 non-login shell 环境，这意味着`~/.bashrc`会被执行，`~/.bash_profile`不会被执行。如果想进入 login shell 环境，设置命令为`/bin/bash --login`

<details>
<summary>图示：iterm2 配置</summary>
<img alt="iterm2 配置" src="https://wx4.sinaimg.cn/large/6cdfff77gy1h48vbjq2bhj20pi0h8gpl.jpg"/>
</details>

### 同时执行这两个文件

可以在`.bash_profile`中添加如下代码，这也是 [GNU bash manual](https://www.gnu.org/softwa//bash/manual/bash.html#Invoked-as-an-interactive-non_002dlogin-shell) 中推荐的。

```shell
if [ -f $HOME/.bashrc ]; then
    source $HOME/.bashrc
fi
```

shell 脚本中`-f`用以验证：提供的路径存在，且为一个文件（file）。这段脚本是为了应对 Mac 默认没有`~/.bashrc`文件的问题。

### 其他资料
<details>
<summary>图解 bash 配置文件的加载顺序</summary>
<img alt="bash 的加载顺序" src="https://wx4.sinaimg.cn/large/6cdfff77gy1h48umb1dzpj217a0qjdm7.jpg">
<p>标注：在 Mac 中 ~/.bashrc 的父节点为 /etc/bashrc，在 Linux 中才是 /etc/bash.bashrc</p>
<p>该图准确性待考证。</p>
<p>~/.bash_profile 有4条线：远程和非远程各两种(login & non-interactive)、（login & interactive）</p>
<p>~/.bashrc 有2条线：非远程各两种(non-login & non-interactive)、（non-login & interactive）</p>
</details>

### 参考资料：

- Stack Exchange: [What is the difference between .bash_profile and .bashrc?](https://apple.stackexchange.com/questions/51036/what-is-the-difference-between-bash-profile-and-bashrc)
- 网道 Bash 脚本教程：[Bash 启动环境](https://wangdoc.com/bash/startup.html)
