---
layout: post
title: Npm install报cb.apply is not a function错误
date: 2022-04-06 16:07:24.000000000 +09:00
tags: Node Npm
---

**关键词** Node Npm

### 前言
在项目npm install的时候，报出了cb.apply is not a function的错误，本文介绍了解决这个问题的办法

### 步骤
1. 访问 C:\Users(your username)\AppData\Roaming
2. 删除 `npm` 文件夹，如果有npm_cache的文件夹，也一并删除
3. 在命令行输入 `npm cache clear --force` 
4. 重新执行 `npm install`

### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通