---
title: Koa2基础
date: 2019-2-21 23:54:01
tags: Koa2基础
categories: Koa2
---
<!-- toc -->
[Koa官网](https://koa.bootcss.com)
[《Koa2进阶学习笔记》已完结](https://chenshenhai.github.io/koa2-note/)
### 使用koa-generator生成koa2项目
全局安装koa-generator: `npm install -g koa-generator`
使用koa-generator生成koa2项目: `koa2 -e koa2-learn` 
　　　　　　　　　　　　　　　　　__-e 添加ejs模板引擎支持(默认是jade)__
　　　　　　　　　　　　　　　　　koa2-learn 项目名<!-- more -->
``` js
$ koa2 -e koa2-learning

   create : koa2-learning
   create : koa2-learning/package.json
   create : koa2-learning/app.js
   create : koa2-learning/public/javascripts
   create : koa2-learning/routes
   create : koa2-learning/routes/index.js
   create : koa2-learning/routes/users.js
   create : koa2-learning/public/images
   create : koa2-learning/public/stylesheets
   create : koa2-learning/public/stylesheets/style.css
   create : koa2-learning/public
   create : koa2-learning/views
   create : koa2-learning/views/index.ejs
   create : koa2-learning/views/error.ejs
   create : koa2-learning/bin
   create : koa2-learning/bin/www

   install dependencies:
     $ cd koa2-learning && npm install

   run the app:
     $ DEBUG=koa2-learning:* npm start
```
PS: 如果出现 `npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.7: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})`  不用担心。出现原因：
`fsevents` 不在 `package.json`里，但是仍然安装了，是因为你的系统是Windows系统，fsevents是苹果系统的可选依赖,你的项目有可能是团队项目，别人在他的mac上安装了fsevents相关依赖库，所以到这边你也就安装到你的windows上边了。你可以检查你的package.json 文件中是不是有fsevents相关依赖，删除即好！
如果没有，其他的npm包也会有依赖fsevents的！！！
__这是warning错误，是因为mac下需要fsevents，这里是在windows环境，所以可以忽略这个警告，对你没什么影响的。__
运行: `DEBUG=koa2-learning:* npm start ` || `npm run dev`
效果： 出现 __node bin/www__ 访问 `http://localhost:3000/`
注意： npm start 、 npm test 、 npm run dev 、 npm run prd

### async 和 await 语法
#### 异步概念
> 是指一个进程在执行某个请求的时候，如果这个请求没有执行完毕，进程不会等待，而是继续执行下面的请求。
#### 理解async 和 await 

### Koa2 中间件
#### koa2 中间件的原理
![koa2](/assets/images/koa2.png)
![koa2](/assets/images/koa2中间件.png)
#### 自定义 koa2 中间件

### koa2 路由
#### 路由写法
#### 接口举例

### cookie 和 session
#### cookie 和 session 的定义
#### cookie 和 session 的作用


推荐： 
　　[从头实现一个koa框架](https://zhuanlan.zhihu.com/p/35040744)
　　[浅析koa的洋葱模型实现](https://segmentfault.com/a/1190000013981513)