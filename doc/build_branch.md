# 根据环境构建不同的产物

> 不同的环境可能需要不同的配置
> 每一个环境的配置可能不同，例如 调用api，不同的环境api的访问地址不同。



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





