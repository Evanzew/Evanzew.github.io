---
layout: post
title: 如何在h5和小程序中适配iphoneX及更高版本全面屏底部的安全区
date: 2022-09-01 09:59:24.000000000 +09:00
tags: Css IOS
---

**关键词** Css IOS 

### 前言
在项目开发的过程中，需要IOS全面屏底部安全区适配

### 步骤
1. h5需要设置页面属性：
{% highlight ruby %}
 <meta name="viewport"
    content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no,viewport-fit=cover">
{% endhighlight %}

2.在body添加如下属性 
{% highlight ruby %}
 body {
    padding-bottom:constant(safe-area-inset-bottom); /*兼容 IOS<11.2*/
    padding-bottom: env(safe-area-inset-bottom); /*兼容 IOS>11.2*/
  }
{% endhighlight %}

### 后言
最近工作很忙，没有时间更新。希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通