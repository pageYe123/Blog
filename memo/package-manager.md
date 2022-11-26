## 包管理工具

### init 生成 package.json

```shell
# 所有问题选“是”，生成 package.json
npm init -y 
## 等价于
yarn init -y # 这个和 npm init -y 生成的文件内容有区别
```

### 查看模块远程仓库信息

```shell
npm info moduleName
```

不建议使用：~~`yarn info moduleName`~~，提供的版本信息非常冗长。

### 安装模块

### npm

```shell
# 全局安装
npm install -g moduleName # Mac 中 npm 全局安装的模块在 /usr/local/lib/node_modules/

# 非全局安装，自动生成 package-lock.json
npm install moduleName # 安装模块到项目目录下
npm install -save moduleName # -save 的意思是将模块安装到项目目录下，并在 package.json 的 dependencies 字段写入依赖。
npm install -save-dev moduleName # -save-dev 的意思是将模块安装到项目目录下，并在 package.json 的 devDependencies 字段写入依赖。

# 安装 package.json 里模块依赖
## 安装 package.json 中所有依赖文件，包括 dependencies、devDependencies
npm install # 覆盖 package-lock.json
## 只安装 package.json 中 dependencies（运行依赖）文件
npm install --dependencies
## 只安装 package.json 中 devDependencies 字段包含的模块
npm install --devDependencies
```

### yarn

```shell
# 全局安装
yarn global add moduleName
yarn global add @vue/cli@4.1.2 # 安装指定版本的模块

# 非全局安装，自动生成 yarn.lock
## 1）给当前项目的生产环境引入模块依赖。并写入 package.json 的 dependencies 字段。
yarn add moduleName 
yarn add jquery # 要求 moduleName 全小写，如 jquery
## 2）给当前项目的开发环境引入模块依赖。并写入 package.json 的 devDependencies 字段。
yarn add moduleName --dev

# 安装 package.json 中 dependencies 字段包含的模块
yarn  # 或者 yarn install
```



### package.json

版本号：

- `~指定版本`：**安装时不改变大版本号和次要版本号。**比如 "vue": "~2.6.11"，表示安装 2.6.x 的最新版本（不低于2.6.11），但是不安装 2.7.x。
- `^指定版本`：**安装时不改变大版本号。**比如 "vue": "^2.6.11"，表示安装 2.6.11 及以上的版本，但是不安装 3.0.0。

修改完`package.json`中的版本号，需要执行`yarn`或`npm install`命令重新更新相应版本号的模块。

### npm——Node.js 默认程序包管理器

```shell
# 查看 npm 全局安装的包
npm list -g
npm list -g --depth 1 ## 指定依赖深度

# npm 换源。安装 nrm（npm 镜像源管理工具）
npm install -g nrm --registry=https://registry.npm.taobao.org
nrm use taobao
```

### nvm——Node.js 版本管理工具

```shell
# 查看本机当前使用 nvm 安装的 node.js 版本列表
nvm list
# 查看远程可用版本号
nvm ls-remote
# 安装制定版本的 node.js
nvm install 10.24.1
# 卸载指定版本的 node.js
nvm uninstall 10.24.1
# 使用指定版本的 node.js
nvm use 10.24.1
# 使用系统自带的 node.js
nvm use system
```

### yarn——包管理工具

```shell
# 查看 yarn 全局安装的包
yarn global list
# 查看 yarn 当前镜像源
yarn config get registry
# 设置 yarn 镜像源为淘宝镜像
yarn config set registry https://registry.npm.taobao.org/
```

