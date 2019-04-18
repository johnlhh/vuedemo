# 为什么要构建不同分支

> 现在公司一般都有开发环境，测试环境和产线环境，有些有预发环境等等。不同的环境可能需要不同的配置,例如调用api，不同的环境api的访问地址不同。
> 那么构建出来的产品也应该不同，我们可以根据不同的构建命令来产生不同分支的构建产物，这样我们在发布项目的时候就非常清楚发那个分支的产物了。

# 那如何构建不同分支
> 一开始我们可能非常迷茫，怎么做呢？这里需要找到一个突破口，就是package.json文件，我们知道构建的命令是
> npm run build, 这个真正执行的是脚本  "build": "node build/build.js",
> 当执行完这个命令后，整个项目发生了什么变化。我们发现新增了一个dist目录，目录下面有index.html和static目录
> 这样我们就找到关键的文件build/build.js，我们可以找到build.js里面那些内容和产物相关，找到相关语句进行修改尝试构建看修改效果
> 经过尝试之后你就慢慢知道了怎么改成自己想要的样子。当然这个是在你有一些编程经验的前提下会更好理解代码的用意。
> 如果你想要知道为什么这么做，那你需要了解[webpack的知识点](https://www.webpackjs.com/guides/)
> 下面我们来修改成另一个构建的样子。

## 安装环境工具依赖

npm install cross-env --save-dev

## 修改构建配置 package.json

```
    把
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "build": "node build/build.js",

    替换成
    "dev": "cross-env NODE_ENV=dev RUN_FLAG=local webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "build:dev": "cross-env NODE_ENV=dev node build/build.js",
    "build:test": "cross-env NODE_ENV=test node build/build.js",
    "build:prepare": "cross-env NODE_ENV=prepare node build/build.js",
    "build:product": "cross-env NODE_ENV=product node build/build.js",
    "build:all": "npm run build:dev && npm run build:test && npm run build:prepare && npm run build:product"
```
> 定义环境变量来构建项目


## 修改构建配置 build/webpack.base.conf.js

```
把这段代码
output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  }
替换成
output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.RUN_FLAG === 'local'
      ? config.dev.assetsPublicPath
      : config.build.assetsPublicPath
  }

```
> 根据环境变量 设置全局环境参数


## 修改构建配置 build/webpack.prod.conf.js

```
把这段代码
const env = process.env.NODE_ENV === 'testing'
  ? require('../config/test.env')
  : require('../config/prod.env')

替换成
const env = require('../config/' + process.env.NODE_ENV + '.env')

把这段代码
filename: process.env.NODE_ENV === 'testing'
        ? 'index.html'
        : config.build.index,
替换成
filename: config.build.index,

```
> 根据环境变量 设置全局环境参数

## 修改构建配置 build/build.js

```
把这段代码
 process.env.NODE_ENV = 'production' 删掉

 rm(path.join(config.build.assetsRoot,config.build.assetsSubDirectory), err => {
 替换成
 rm(path.join(config.build.assetsRoot), err => {

```
> 根据环境变量 删除不同的环境产物，然后重新构建

## 修改构建配置 config/index.js

```
把这段代码
 index: path.resolve(__dirname, '../dist/index.html'),
 assetsRoot: path.join(__dirname, "..", "dist"),

 替换成
 index: path.resolve(__dirname, '../dist/' + process.env.NODE_ENV + '/index.html'),
 assetsRoot: path.join(__dirname, "..", "dist", process.env.NODE_ENV),

```
> 根据环境变量 生成不同的环境产物


## 修改目录config下环境配置的文件名称或文件内容

dev.env.js
```
'use strict'
module.exports = {
  BASE_API: '"https://api.dev.com"'
}

```

test.env.js
```
'use strict'
module.exports = {
  BASE_API: '"https://api.test.com"'
}

```


prepare.env.js
```
'use strict'
module.exports = {
  BASE_API: '"https://api.prepare.com"'
}

```
把 prod.env.js改成product.env.js内容如下
```
'use strict'
module.exports = {
  BASE_API: '"https://api.prepare.com"'
}

```

> 在vue文件中 可以通过 process.env.BASE_API 访问环境参数配置

# 执行不同环境的构建,执行下面命令

npm run build:dev   

npm run build:test

npm run build:prepare

npm run build:product

npm run build:alll

> 执行命令后会生产不同环境的产物 在目录dist下

# 小结
> 通过更改构建配置我们能够知道构建vue项目的整个流程，以及一些插件的用途。如果你读过webpack的指南，相信对你理解整个流程非常有帮助。





