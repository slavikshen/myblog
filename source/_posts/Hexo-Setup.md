---
title: 搭建Hexo博客系统
date: 2017-05-28 15:00:26
tags: [Hexo,NPM,Node.js]
categories: system
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

按照[Hexo手册](https://hexo.io/docs/setup.html)上说的配置。十分顺利。

```sh
$ hexo init myblog
$ cd myblog
$ npm install
```

编辑_config.yml的时候遇到一个问题，时区设置不支持UTC+的表示方法。必须填写名称。还好有wiki上的[时区名称列表](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)。

如果时区设置错误，所有hexo命令都不会有正确结果。因为他们都是按照时间戳判断文章是不是应该加入内容更新。

启动本地测试环境

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

测试提交

```sh
hexo deploy
```

## 设置皮肤

先寻找资源
[官方网站](https://hexo.io/themes/)
[Github](https://github.com/hexojs/hexo/wiki/Themes)

复制或者git clone内容到themes下。每个主题可能会有特殊的模块依赖关系。

最后选了Artchen的[Typescript](https://github.com/artchen/hexo-theme-typescript)。实现很干净，方便修改。

因为会需要修改内容，所以先Fork，然后同步了自己分支下的内容安装。

安装依赖的模块，并把Typescript加为submodule，方便以后同步。

```sh
npm i --save hexo-generator-tag hexo-renderer-ejs hexo-renderer-less hexo-renderer-marked hexo-pagination
git submodule add https://github.com/slavikshen/hexo-theme-typescript.git themes/typescript
```

## 配置VSCode环境

为了方便更新Hexo内容，减少终端窗口，决定采用VSCode的Task功能。

首先在package.json里增加脚本功能。

```json
"scripts": {
	"deploy": "hexo clean && hexo deploy;",
	"hot-server": "hexo server & hexo generate --watch"
}
```

deploy方便提交内容。

```sh
$ npm run deploy 
```

hot-server简化启动内容变化监视和本地测试服务器。

```sh
$ npm run hot-server
```

然后创建.vscode\task.json

```json
{
	"version": "0.1.0",
	"command": "npm",
	"isShellCommand": true,
	"showOutput": "always",
	"suppressTaskName": true,
	"tasks": [
		{
			// 部署
			"taskName": "deploy",
			"args": ["run", "deploy"]
		},
		{
			// 测试
			"taskName": "test",
			"args": ["run", "hot-server"]
		}
	]
}
```

调出VSCode命令窗口 <kbd>CMD/CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd>，输入test。

在Output会出现测试服务器和内容变化监控的启动信息。

```sh
> hexo-site@0.0.0 hot-server /Users/slavik/Documents/myblog
> hexo server & hexo generate --watch

INFO	Start processing

```







