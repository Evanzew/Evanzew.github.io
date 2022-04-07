---
layout: post
title: Angular13 如何修改项目baseUrl
date: 2021-12-01 16:07:24.000000000 +09:00
tags: Angular2+
---

**关键词** Angular13

### 前言
因为项目需求，需要修改项目的baseUrl，本文详细介绍了具体步骤和代码。

### 步骤
在文件中主要需要添加以下代码
> index.html
{% highlight ruby %}
<head>
  ...
  <base href="/test">
</head>
{% endhighlight %}

> package.json
{% highlight ruby %}
"scripts" :{
  ...
  prod": "npm run icons && npm run wbv && ng build --configuration=production --base-href=/test/", 
}
{% endhighlight %}

- 注意，这里的base-href前后都需要“/”

### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通