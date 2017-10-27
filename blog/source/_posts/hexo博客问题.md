---
title: hexo博客问题
toc: true
date: 2017-10-10 14:28:48
tags: git
categories: 
---

hexo博客中遇见的问题及解决办法。

<!--more-->

## 解决fs.SyncWriteStream报错问题

最近，在执行hexo命令时，遇到了这样的报错：

```js
(node:11320) [DEP0061] DeprecationWarning: fs.SyncWriteStream is deprecated.
```

在网上查阅资料发现，原来是`node.js`从8.0开始弃用`fs.SyncWriteStream`这个方法。有人issue了这个问题（[fs.SyncWriteStream](https://github.com/hexojs/hexo/issues/2598)），原来是需要更新hexo-fs插件：

```js
npm install hexo-fs --save
```

但是这样我更新之后还是有同样的错误，那么就是用以下方法定位错误在哪里：

```js
$ hexo clean --debug
```

结果如图：

![hexo--debug](http://oxligjg1w.bkt.clouddn.com/error-debug.png)

可以看到我所在报错的位置是`hexo-deployer-git`，于是找到`hexo-deployer-git\node_modules\hexo-fs\lib\fs.js`的第718行，将`exports.SyncWriteStream = fs.SyncWriteStream;`注释掉，即可。

再次使用`$ hexo clean --debug`，发现问题解决了：

![](http://oxligjg1w.bkt.clouddn.com/hexo--debug.png)

## 解决Template render error问题

在执行命令时，又遇到了这样的问题：

```js
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path)
```

经过查询，我知道了，有两个地方会导致出现这样的问题：①`_config.yml`配置文件中，你写的格式、缩进等可能会导致出现问题；②在你的`page/post`可能是你的哪篇文章出现了问题。


帮助文档：[hexo--troubleshooting](https://hexo.io/docs/troubleshooting.html)

