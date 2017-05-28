---
title: 搭建Hexo
date: 2017-05-28 15:00:26
tags:
---

## 安装Hexo

因为Mac已经有XCode自带的Git和Node.js开发环境，所以直接安装。

中间遇到了恶心的Log环境问题（DTraceProviderBindings）。不明白为啥这么多人喜欢用bunyan。参考[kikiroc](https://kikoroc.com/2016/05/04/resolve-hexo-DTraceProviderBindings-MODULE-NOT-FOUND.html)上的方法解决。

```sh
$ npm install -g -hexo-cli --op-optional
```

如果之后还是出现DTraceProviderBindings的错误，可以在项目删除这个模块。因为没有必要调试Hexo
```sh
$ npm uninstall dtrace-provider
```

## 配置项目

和[Hexo手册](https://hexo.io/docs/setup.html)上说的完全一样。十分顺利。

```sh
$ hexo init myblog
$ cd myblog
$ npm install
```

编辑_config.yml的时候遇到一个问题，时区设置不支持UTC+的表示方法。必须填写名称。还好有wiki上的[时区名称列表](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)。

如果时区设置错误，所有hexo命令都不会有正确结果。因为他们都是按照时间戳判断文章是不是应该加入内容更新。


## 启动本地测试环境

```sh
$ hexo generate
$ hexo server
```

打开[Hexo本地测试服务器](http://localhost:4000/)，一切顺利

## 发布到GitHub

需要插件hexo-deployer-git

参考[安装方法](https://gist.github.com/btfak/18938572f5df000ebe06fbd1872e4e39)

```sh
$ npm install hexo-deployer-git --save
```

向_config.yml增加git设置

```yml
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
	type: git
	repo: git@github.com:slavikshen/slavikshen.github.io.git
	branch: master
```

## 完成

```sh
hexo deploy
```








