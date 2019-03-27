# 从创建到部署的 一个vuedemo

> Vue Demo 

## Build Setup

# 前置准备 安装npm和nodejs 并安装vue脚手架
npm install --global vue-cli
# 查看vue版本
vue -V

# 创建项目 webpack 为模板名称， vuedemo 为项目名称 
vue init webpack vuedemo
# 切换到项目根目录
cd vuedemo
# 安装依赖
npm install

# 运行服务 默认地址 http://localhost:8080  可以在文件config/index.js中修改
npm run dev

# build for production with minification
npm run build

# 构建项目并生出项目构建报告
npm run build --report

# 本地部署环境 
- 假设项目访问地址为 http://vue.test.com/vuedemo/ 

- host配置/etc/hosts
>  127.0.0.1	vue.test.com

- nginx的配置 /usr/local/nginx/conf/nginx.conf 添加如下配置

>   server_name  vue.test.com
>
>   location /vuedemo {
>        try_files $uri $uri/ /vuedemo/index.html;
>    }

- 重启nginx 
> sudo nginx -s reload

- 修改构项目建配置 config/index.js build模块中的assetsPublicPath的值。设为 assetsPublicPath: '/vuedemo/'
- 重新构建 npm run build 然后将构建文件即 项目目录dist中的文件copy到 nginx的 /usr/local/nginx/html/vuedemo中

- 浏览器访问项目 http://vue.test.com/vuedemo/


# 运行测试单元 需要设置文件 test/unit/jest.conf.js文件中添加:  testURL: 'http://localhost',
npm run unit

# 运行e2e测试 默认端口8888
npm run e2e

# run all tests
npm test

```
