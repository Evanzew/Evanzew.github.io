---
layout: post
title: Angular 8如何更改子组件样式
date: 2020-12-12 16:07:24.000000000 +09:00
---

**关键词** Angular 8  style

### 前言
本文介绍了在Angular 11中，更改子组件样式失效时的一种解决办法。

#### 发生场景
根据项目需求，需要更改一个子组件的样式，单纯的使用后代选择器无法实现。

#### 解决办法
> 使用 ::ng-deep 

具体代码如下
{% highlight ruby %}
.text-class ::ng-deep .card{
  min-height: 100px;
}
{% endhighlight %}

#### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通。


