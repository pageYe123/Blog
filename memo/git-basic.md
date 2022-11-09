# Git 基础操作备查
## git 配置

```shell
git config --global user.name '你的英文名'
git config --global user.email '你的邮箱'
git config --global core.editor 'code --wait' # 用 VSCode 代替 vim
git config --global core.autocrlf input # 在提交时把 CRLF 转换成 LF，签出时不转换
git config --global core.whitespace cr-at-eol # 消除 git diff 中的 ^M
git config --global core.quotepath false # 应对 git status 含中文的文件（夹）名显示为八进制的字符编码
git config --global init.defaultBranch master # 默认分支设置
git config --global pull.rebase false # git pull 时默认用 git merge 来合并代码，应对 git pull 警告
# 为 git 设置代理，注意端口号一定要设对。如果是用SSH协议，则不走http代理。
git config --global http.proxy 'socks5://127.0.0.1:51833'
git config --global https.proxy 'socks5://127.0.0.1:51833'
# 避免提交的文件太大(默认是1M)导致 push 失败，或下载的文件太大导致读取失败。
git config --global http.postBuffer 500M
# 将 .DS_Store 加入全局的 .gitignore 文件
echo .DS_Store >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

`--global`在全局配置，`--local`仅在本项目中配置，`--unset`取消配置  

除了通过命令行进行 git 配置，也可直接修改`~/.gitconfig`文件。  

注意：请确保已安装 VSCode，并且 `code` 命令已加入环境变量。必须加`--wait`参数，否则`git commit -v`会报错。

## 本地仓库的创建

使用 Git 来对现有的项目进行管理，在现有目录中初始化仓库。

```shell
git init
```

## 文件状态演变
### 加入暂存区与提交

- 提交工作目录中所有变更文件
```shell
git add .
git commit -m "提交信息"
```

等价于

```shell
# 只针对 tracked 文件
git commit -am "提交信息"
```

- 针对单个文件进行提交

```shell
git commit <file> -m '提交信息'
```

- 提交时显示所有 diff 信息（只针对 tracked 文件） 
提交信息在编辑器中输入。

```shell
git commit -v .
```

### 移除文件

#### 删除工作区文件，并且将这次删除放入暂存区

注意：要删除的文件是没有修改过的，就是说和当前版本库文件的内容相同。

```shell
git rm <file>
```

#### 将文件从本地仓库和暂存区移出到工作目录，变为 untracked 文件

```shell
git rm --cached <file>
git rm -r --cached <directory>
```

- 场景：小A在提交代码时不小心把一个用于本地测试的数据库文件 database 一并提交并推送到 Github，导致 Github 源码体积增加几百 M。现在他需要在仓库中删除 database 文件，但本地依然保留该文件（本地开发需要）。

```shell
# 暂存区删除database，本地保留database
git rm --cached database
# 提交变动
git commit -m "删除无关文件"

touch .gitignore
echo database >> .gitignore
git add .
git commit -m "添加.gitignore"
git push origin master
```

#### 删除未追踪文件(untracked files)

```shell
# 先看一下会删除哪些文件
git clean -nf
# 进行删除
git clean -f
```

删除未追踪文件及文件夹

```shell
# 先看一下会删除哪些文件及文件夹
git clean -nfd
# 进行删除
git clean -fd
```

### .gitignore忽略文件

```Ignore List
# 忽略.a为后缀的文件
*.a

# 忽略所有__为前缀的文件
__*

# 尽管上面忽略了.a为后缀的文件，但不忽略lib.a
!lib.a

# 只忽略当前目录下的TODO文件，但不忽略子目录下的TODO文件
/TODO

# 忽略build/目录下的所有文件
build/

# 忽略doc/notes.txt，但不忽略 doc/server/a.txt
doc/*.txt

# 忽略doc/目录下的所有的以.txt结尾的文件
doc/**/*.txt

```

有时会发现 .gitignore 规则不生效。原因是 .gitignore 只能忽略那些原来没有被 track 的文件，如果某些文件已经被纳入了版本管理中，则修改 .gitignore 是无效的。

解决方法：先把文件改成 untracked 状态，然后再提交。

```shell
git rm -r --cached .
```



## 临时储藏代码

### stash（藏匿）当前修改
`git stash`  
实际应用中推荐给每个 stash 加一个 message，用于记录版本信息

```Shell
git stash save 'test-git-stash'
# 等价于
git stash -m 'test-git-stash'
```



### 重新加载缓存的 stash
`git stash pop`

### 查看现有 stash
`git stash list`

### 移除 stash
`git stash drop stash@{0}`

## git diff 命令比较不同

比较文件在暂存区和工作区的差异。

```shell
git diff <文件名>
```

从`commit-1`到`commit-2`文件发生了那些变化。

```shell
git diff <commit-1> <commit-2> <文件名>
```



## 撤销操作 

### git reset：重置为特定 commit

**三个概念：(working directory)工作目录(add)→(Index)暂存区(commit)→本地版本库**

- 针对工作目录、暂存区、本地版本库的整体重置

`git reset <mode> <commit-id>` 重置为特定 commit

`git reset <mode> HEAD` 重置为 HEAD 提交

三个 mode：

```shell
# 更改本地版本库，文件还原至暂存区，不会对工作目录进行任何更改。
git reset --soft HEAD~1

# 默认选项，会清空暂存区
git reset
# 等价于
git reset --mixed 
# 等价于
git reset --

# 连工作目录里的文件都还原，但不能删除 untracked 的文件。
git reset --hard
```

- 场景：撤销提交，回退为之前的版本，保留工作目录中的文件

`git reset` 默认为 HEAD，回退到当前版本，等同于`git reset HEAD`

`git reset HEAD^` 回退所有内容到上一个版本，等价于`git reset HEAD~1`

`git reset HEAD^^^`回退到此版本之前的第3个版本，等价于`git reset HEAD~3`。


- 场景：单个文件、目录移出暂存区

```
git reset <file>
```

- 场景：取消本次提交，变更退回至暂存区
```
git reset --soft HEAD~1
```

注明：`git reset --soft HEAD` 无效。

- 场景：从本地版本库还原工作目录中的单个文件
    前提：本地版本库 HEAD commit 中必须有这个文件。

```
git checkout HEAD -- <file>
等价于
git restore <file>
```
注明：`git reset --hard <file>` 无效。


### git revert：撤销特定commit

`git revert HEAD`撤销最近的1次 commit。撤销后会向前新增提交，保留所有提交历史，相对安全。

`git revert <commit-id>` 撤销特定提交。

如果和所撤销 commit 的上一次 commit 有共同修改的文件，会产生冲突，解决冲突即可。

git revert 提交之后，push 到远程仓库，可以看到回退到之前哪个版本，保留了中间的提交历史。而 git reset 回退之后，push 到远程仓库，回滚的中间部分的提交历史全部没有。

### git checkout：临时切换到特定 commit

`git checkout <commit-id>`临时切换到特定 commit

`git checkout v1.4`查看 v1.4 标签（TAG）处的仓库状态。

这使得仓库的HEAD处于分离状态（detached HEAD）。意味着所做的任何更改都不会更新标签的内容。它们将创建一个新的分离提交。这个新分离的提交将不是任何分支的一部分，并且只能由提交SHA哈希直接访问。 因此，如果你需要进行更改，比如你要修复旧版本中的错误，那么通常需要创建一个新分支：`git checkout -b <new-branch-name>`之后再合并到主分支上

想离开分离头指针状态回到正常态，只需要`git checkout <branch-name>`即可


## 远程仓库操作

`git remote -v`查看远程仓库信息

### 添加、移除远程仓库

```shell
git remote add <远程仓库名> <远程仓库URL>
git remote add origin <远程仓库URL>
git remote add gitlab <远程仓库URL>
git remote remove gitlab
```

### 远程仓库换源

```shell
# 必须先创建远程仓库 origin 
git remote set-url origin <远程仓库URL>
```

### 拉取远程仓库数据

`git fetch`拉取远程仓库的数据，包括所有分支及各个分支上的代码修改情况。

`git checkout -b <分支名> <基分支> `将远程仓库的某分支作为基分支检出到本地仓库，并命名该分支。

#### 关于git pull vs git fetch

git pull <=> git fetch+git merge

git pull先拉取再合并

### 推送到远程仓库

`git push -u origin master` "-u" 表示 "--set-upstream"，建立本地分支与远程分支的关联(upstream reference) 以后 `git push`、`git pull`简写即可。也可以写进alias别名里。

`git push origin <分支名> --force`强制推送到远程仓库，会覆盖远程仓库的提交历史。

### 删除远程仓库分支

```
git push origin --delete <远程分支名>
```

## 分支

```shell
# 创建分支
git branch <分支名>

# 切换分支，如果提示本地有加入暂存区，想舍掉可用--force参数
git checkout <分支名>

# 创建分支并切换到新分支
git checkout -b <分支名>

# 删除本地分支
git branch -d <本地分支名>

# 删除远程分支
git push origin --delete <远程分支名>

# 改变分支名称
## 改变当前分支名称
git branch -m <新分支名> 
## 改变指定分支名称
git branch -m <指定分支名> <新分支名>
### 如果新分支名已经存在，则拒绝改变
## 强制改变当前分支名称（会覆盖掉冲突的分支名）
git branch -M <新分支名>

# 查看本地和远程仓库的所有分支 
git branch -a

# 分之合并
## 合并到哪个分支就切换到哪个分支。比如合并到主分支，就checkout到主分支
git checkout master
git merge <otherBranch>
## 之后解决冲突
```

## 提交历史
### 查看提交历史

`git show <commit-id>`查看指定 commit 的所有修改

`git log` 查看提交历史

`git log <分支名>`查看特定分支的提交历史

`git reflog` 查看所有提交历史，比git log要全面

### 修改或者合并提交历史
- 修改最后一次提交
```shell
git commit -m 'initial commit'
git add forgotten_file #不仅可修改提交消息，还可以增删改文件
git commit --amend
```

- 修改多个提交

`git rebase -i HEAD~3`交互式变基工具，打开文件后阅读命令提示，squash命令是合并的意思。遇到问题可以通过`git rebase --abort`终止变基。


## 变基 rebase
作用：美化提交历史（对比图一和图三）
### 变基举例

![图一](https://cdn.nlark.com/yuque/0/2022/png/29373291/1656836077196-b5b2a8f3-15eb-4435-83ad-22afb333bafd.png)

![图二](https://cdn.nlark.com/yuque/0/2022/png/29373291/1656835949938-38c5f0ba-487b-46e2-a04a-c7dd0fc58627.png)

![图三](https://cdn.nlark.com/yuque/0/2022/png/29373291/1656836205952-733574e8-5482-42f6-a6c1-fb3cd3396aec.png)

```shell
git checkout experiment
git rebase master #如果有冲突解决冲突

git checkout master
git merge experiment # 将 experiment 分支合并到 master 分支
```

### 场景：删除指定commit
git rebase -i <commit-id> 通过美化提交历史，删除本地仓库指定提交。**commit-id 为要删除的 commit 的上一个 commit 号。**

```shell
git log #获取 commit 信息 
git rebase -i <commit-id> 
# -i表示--interactive，交互式变基，
# 用户自己决定相对于祖先的历次提交，怎么处置，是删除还是与下一个合并。
# 特别注意：commit-id 为要删除的commit的上一个commit号
# 编辑文件，将要删除的 commit 之前的单词改为 drop 表示删除。
# 还有其他命令如 s, squash 表示与前面的提交融合
# 保存文件退出
git log #查看确认是否删除
```

参考资料：[Git 删除本地仓库指定commit的方法](https://www.jianshu.com/p/2fd2467c27bb)

## 打标签 git tag

```shell
# 创建一个标签
git tag <your_tag_name> 
# 创建一个带有注释的标签
git tag -a <your_tag_name> -m 'your_tag_description'

# 删除远程仓库的标签
git push --delete origin <tag-name>
# 删除本地的某个标签
git tag -d <your_tag_name>

# 查看所有本地标签
git tag --list
# 查看标签信息
git show <your_tag_name>
# 查看所有的远程标签及 commit ID
git ls-remote --tags origin

# 推送一个标签到远程
git push origin <your_tag_name>
# 推送多个本地标签到远程
git push origin --tags
```



## 其他

### Github `New repository` 最佳方案

新建远程仓库时，写完必填项`Repository name`，其他一概不填，内容完全由本地仓库推送上去。这样保证远程仓库没有 commit 信息，和本地仓库有 commit 信息的仓库进行合并时会容易一些。

### 命令行中打开远程仓库首页

  `yarn global add git-open`  
  用`git open`打开 Github 远程仓库首页

### 大体积仓库的下载

先克隆一个远程仓库到本地，其中只有一个`.git`文件夹，不检出任何项目文件，再更新代码。

```shell
git clone --no-checkout <repoURL>
git pull
```

### git 别名

我们可以在`~/.zshrc`中定义，zsh初始设置了一些 git 别名，放在`~/.oh-my-zsh/plugins/git/git.plugin.zsh`（[文章：oh-my-zsh中 git 别名设置](https://segmentfault.com/a/1190000007059404)）

### 查看 git config 信息

查看仓库和全局的 git 配置信息，及存放位置：
```Shell
git config --list --show-origin
```

### 查看某个 Github 仓库的大小

目前通过访问`https://api.github.com/repos/{owner}/{repo}"`，查看`size`字段获取，单位为 KB。 `owner`表示所有者，`repo`代表仓库名。

访问`api.github.com`可以查看 Github 提供的其他 API。

### 统计本地仓库记录的 commit 次数

```Shell
git log | grep -e 'commit [a-zA-Z0-9]*' | wc -l
```

### 为已存在的项目添加开源许可证

对于已创建的代码仓库，在 GitHub 仓库主页面中依次点击 “Add file → Create new file”。
在跳转到的页面内的文件输入框中填入 “LICENSE”，在文件输入框的右侧将会出现 “Choose a license template” 按钮，点击该按钮。

![添加开源许可证](https://wx4.sinaimg.cn/large/6cdfff77gy1h6yz3cpemmj20pj07o414.jpg)

### 应对文件太大或网络太慢超时导致 RPC failed

RPC (Remote Procedure Call)，远程过程调用，简单讲就是一个节点请求另一个节点提供的服务。

```shell
# 增大 postBuffer 避免缓存溢出，如果不够，就继续增大
git config --global http.postBuffer 800M

# 增加最低速时间
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999
```

### 为什么命令行中，不翻墙，git clone 就那么慢呢？明明访问 github 都是可以的。

20221009注：现统一走命令行代理。

因为 github.global.ssl.fastly.net 域名被限制了。 只要找到这个域名对应的 ip 地址，然后在 `/etc/hosts` 文件中加上 ip 到域名的映射，刷新DNS缓存便可。

```shell
199.232.69.194 github.global.ssl.fastly.net
140.82.113.4 github.com
```

参考：[git clone下载慢的问题](https://www.jianshu.com/p/b662a8b91890) 。修改完之后快很多。

## 解决报错

> fatal: refusing to merge unrelated histories

场景：github 上新建了仓库包含一次提交，本地 git init 之后也有提交，那么本地仓库和远程仓库 git pull 时，git 无法确定谁作为首次提交，于是 git 认为这两者的分支是分离的（divergent）、历史是没有关联的（unrelated）。 

解决方案：

```shell
git pull origin master --allow-unrelated-histories
```



> kex_exchange_identification: Connection closed by remote host

```
➜  githubBlog git:(master) git pull
kex_exchange_identification: Connection closed by remote host
Connection closed by 20.205.243.166 port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

解决方案：

没有查到明确的解决方案，过了一会再试就好了，归因于网络原因。



