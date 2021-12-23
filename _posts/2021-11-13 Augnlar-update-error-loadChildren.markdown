---
layout: post
title: Angular 11升级到13报错：loadChildren error 
date: 2021-11-13 16:07:24.000000000 +09:00
---

**关键词** Angular 11 update loadChildren

### 前言
本文介绍了在升级Angular版本的时候的一种报错和解决办法。

#### 发生场景
根据项目需求，需要把Angular版本从10升级到13。在`npm install`的时候，一切顺利，但是在`npm start`的时候，报错： 
>Type 'string' is not assignable to type 'LoadChildrenCallback

#### 解决办法
用回调函数的方法来loadChildren
> 报错代码
{% highlight ruby %}
 path: 'test',
 loadChildren: './modules/test/test#TestModule',
{% endhighlight %}

> 正确代码
{% highlight ruby %}
 path: 'test',
 loadChildren: () => import('./modules/test/test').then((m) => m.TestModule)
{% endhighlight %}

#### 后言
希望本文会对你有所帮助。

