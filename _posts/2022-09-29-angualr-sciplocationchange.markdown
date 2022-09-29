---
layout: post
title: Angualar2+ 路由跳转，url不变
date: 2022-09-20 09:59:24.000000000 +09:00
tags:  Angualar2+ 
---

**关键词**  Angualar2+ 

### 前言
在项目开发的过程中，客户需要切换到无权限页面的时候，路由不变。当添加成功权限后，刷新页面即可访问。

### 解决步骤
当跳转路由时候，使用`{ skipLocationChange: true }`
{% highlight ruby %}
 this.router.navigate(['/not-allowed'], { skipLocationChange: true });
{% endhighlight %}

### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通