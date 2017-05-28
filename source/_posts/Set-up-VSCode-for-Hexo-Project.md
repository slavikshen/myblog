---
title: 为Hexo项目配置VSCode
date: 2017-05-28 16:53:05
tags:
---

## 目的

方便在VSCode环境下更新Hexo内容，减少终端窗口。

## NPM Package设置

增加简化脚本命令

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

## 配置VSCode Task

```json
{
	// See https://go.microsoft.com/fwlink/?LinkId=733558
	// for the documentation about the tasks.json format
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

## 运行

调出VSCode命令窗口 <kbd>CMD/CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd></kbd>，输入test。

在Output会出现测试服务器和内容变化监控的启动信息

```sh
> hexo-site@0.0.0 hot-server /Users/slavik/Documents/myblog
> hexo server & hexo generate --watch

INFO	Start processing

```