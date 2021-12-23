---
layout: post
title: JS 如何判断是否为数组类型
date: 2020-05-14 16:07:24.000000000 +09:00
tags: JS
---

**关键词**：JavaScript Array prototype

### 前言
以下是我总结的四种判断方法，与大家一起分享：

#### 1. 检查class：
{% highlight ruby %}

Object.prototype.toString.call(arr) = "[object Array]"

{% endhighlight %}

#### 2. Array的原型链
{% highlight ruby %}
Array.prototype.isPrototypeOf(arr) = true
{% endhighlight %}

#### 3. 判断构造函数
{% highlight ruby %}
arr instanceof Array = true
{% endhighlight %}

#### 4. Array自带API
{% highlight ruby %}
Array.isArray(arr) = true
{% endhighlight %}

