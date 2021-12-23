---
layout: post
title: Angular12 升级到13 本地升级正常 云端部署报错
date: 2021-11-14 16:07:24.000000000 +09:00
tags: Angualr13
---

**关键词** Angular13 npm install

### 前言
本文介绍了在Angular11升级到13时，本地运行正常但是在云端部署时还会有版本号报错。

#### 问题原因
造成此报错的原因是，当在升级Angular13后，没有上传新的package-lock.json。

#### 解决办法
删除本地的package-locak.json和node_modules，然后执行`npm install`, 然后git上传最新的package-lock.json和package.json。

#### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通。