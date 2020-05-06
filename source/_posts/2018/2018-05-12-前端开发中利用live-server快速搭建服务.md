---
title: 前端开发中利用live-server快速搭建服务
date: 2018-05-12
datestring: "20180512"
pageid: 1
categories: 
- 前端
tags:
- node
photos: images/poster/5433.jpg
---

在开发中，我们经常需要一个临时服务环境来进行开发或者验证某种想法。live-server就可以满足我们的这种需求，且使用起来非常方便。其相比http-server来说已经集成了hot socketing（live-reload）、opener等功能，功能更加完善。
<!-- more -->

## live-server 的安装

[live-server](https://www.npmjs.com/package/live-server)是npm中的一个模块，所以首先需要安装node.js的环境。如何安装这里不做介绍，可以参考[node.js的官方网站](https://nodejs.org/zh-cn/) 

安装好node.js后，npm管理工具也已经自动安装完成了。然后在终端中运行

```shell
npm install -g live-server
```

这里用了全局安装，虽然还有其他的安装方式，但本人推荐大家用全局的方式。

## live-server的快速使用

要立刻马上将某个文件夹作为服务的根目录，可以直接输入

```shell
#比如用户的项目更目录为public
live-server ./public --port=3000
```

这样就马上就会自动打开浏览器，展示出相关的页面。非常方便。 如果在已有的项目中，可以在package.json中的scripts里设置

```js
"scripts": {
  "server": "live-server ./public --port=3000"
}
```
这样设置好后只要输入 npm run server就可以达到同样的效果。

当然live-server的配置还有许多，可以详细参考 https://www.npmjs.com/package/live-server
