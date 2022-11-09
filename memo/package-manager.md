## 包管理工具

### init 生成 package.json

```shell
# 所有问题选“是”，生成 package.json
npm init -y 
## 等价于
yarn init -y # 这个和 npm init -y 生成的文件内容有区别
```

### 查看模块信息

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

npm install -save moduleName # -save 的意思是将模块安装到项目目录下，并在package.json的dependencies字段写入依赖。
 
npm install -save-dev moduleName # -save-dev 的意思是将模块安装到项目目录下，并在 package.json 的 devDependencies 字段写入依赖。
```

### yarn

```shell
# 全局安装
yarn add global moduleName

# 非全局安装，自动生成 yarn.lock
## 1）给当前项目的生产环境引入模块依赖。并写入 package.json 的 dependencies 字段。
yarn add moduleName 
## 要求 moduleName 全小写，如 jquery
yarn add jquery
## 2）给当前项目的开发环境引入模块依赖。并写入 package.json 的 devDependencies 字段。
yarn add moduleName --dev
```

### 



### package.json

版本号：

- `~指定版本`：**安装时不改变大版本号和次要版本号。**比如 "vue": "~2.6.11"，表示安装 2.6.x 的最新版本（不低于2.6.11），但是不安装 2.7.x。
- `^指定版本`：**安装时不改变大版本号。**比如 "vue": "^2.6.11"，表示安装 2.6.11 及以上的版本，但是不安装 3.0.0。

修改完`package.json`中的版本号，需要执行`yarn`或`npm install`命令重新更新相应版本号的模块。
