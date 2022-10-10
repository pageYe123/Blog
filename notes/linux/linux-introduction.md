## Linux

Linux 是一种操作系统。

Linux 系统一般有4个主要部分：内核、shell、文件系统和应用程序。内核、shell和文件系统一起形成了基本的操作系统结构。Linux 操作系统的内核大部分是用 C 语言写的，但也有部分是用汇编语言写的，因为在对于硬件上，汇编有更好的性能和速度。

## Linux 和 Unix 的区别

Linux 是符合 POSIX 标准的类 Unix，但是不完全兼容。这是一个基于 Unix 操作系统原理的开源操作系统，是可以自由下载的系统。它也可以通过编辑、添加及扩充其源代码而定制该系统。这是它最大的好处之一，而不像今天的其它操作系统（Windows、Mac OS X 等）需要付费。

需要注意的是，Mac OS X 是 Unix，符合单一 Unix 规范（Single UNIX Specification 即 SUS 认证），然后加了一堆自己的库。Mac OS X 严格说是 BSD 系列，它的内核最初是基于 Mach 内核，以及部分   BSD 的附加内核层。

## GNU 和 Linux 的历史

以下改编自知乎回答：[GNU 是什么，和 Linux 是什么关系？](https://www.zhihu.com/question/319783573/answer/656033035)

> Unix 系统被发明之后，大家用的很爽。但是后来开始收费和商业闭源了。一个叫 RMS（Richard Matthew Stallman）的大叔觉得很不爽，于是发起 GNU 计划，模仿 Unix 的界面和使用方式，从头做一个开源的版本。然后他自己做了编辑器 Emacs 和编译器 GCC。
>
> GNU 是一个计划或者叫运动。在这个旗帜下起草了 GPL 等。
>
> 接下来大家纷纷在 GNU 计划下做了很多的工作和项目，基本实现了当初的计划。包括核心的 gcc 和 glibc。但是 GNU 系统缺少操作系统内核。原定的内核叫 HURD，一直完不成。同时 BSD（一种 UNIX 发行版）陷入版权纠纷，x86 平台开发暂停。然后一个叫 Linus 的同学为了在 PC 上运行 Unix，在 Minix 的启发下，开发了 Linux。注意，Linux 只是一个系统内核，系统启动之后使用的仍然是 gcc 和 bash 等软件。Linus 在发布 Linux 的时候选择了 GPL，因此符合 GNU 的宗旨。
>
> 最后，大家突然发现，这玩意不正好是 GNU 计划缺的吗！于是合在一起打包发布叫 GNU / Linux。然后大家念着念着省掉了前面部分，变成了 Linux 系统。实际上 Debian，RedHat 等 Linux 发行版中内核只占了很小一部分容量。



## 大小写敏感

Linux 系统是大小写敏感的，而 Windows 系统和 Mac 系统正好相反，大小写不敏感。

比如 Mac 系统认为`Foobar.md`和`foobar.md`是名称相同的文件，当一个文件被创建时，另一个文件将无法创建。

```shell
touch Foobar.md
# 相同目录下执行以下命令没有反应
touch foobar.md
```

如果硬要在图形化界面手动创建`foobar.md`，会提示：

> 已有相同名称的文件或文件夹。若替换，则会覆盖其当前内容。

## 文件系统

`/etc/hosts` 是配置 ip 地址和其对应主机名(域名)的文件。

```shell
127.0.0.1   localhost
255.255.255.255 broadcasthost
::1             localhost
```

如果把 `127.0.0.1 localhost` 去掉，系统还是会默认将`localhost`指向`127.0.0.1`，除非你把 localhost 指向其他 ip 地址，不过这是不推荐的做法。

修改`etc/hosts`文件需要系统管理员权限。