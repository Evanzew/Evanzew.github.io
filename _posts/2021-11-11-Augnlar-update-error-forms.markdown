---
layout: post
title: Angular11升级到13报错：Angular Forms error Two incompatible decorators on class
date: 2021-11-11 16:07:24.000000000 +09:00
tags: Angular2+
---

**关键词** Angular 11 update

### 前言
本文介绍了在升级Angular版本的时候的一种报错和解决办法。

#### 发生场景
根据项目需求，需要把Angular版本从11升级到13。在`npm install`的时候，一切顺利，但是在`npm start`的时候，报错： 
>Angular Forms error: Two incompatible decorators on class。

#### 解决办法
在Google搜索了解决办法，发现遇到这种情况的人不少，但是都没有明确的解决办法，或者解决办法在本项目不适用。随后查阅了Angular的文档，发现通过以下方法可以解决问题。
在tsconfig.json中添加以下代码：
![image.png](https://upload-images.jianshu.io/upload_images/11992590-5f988580793f0d1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

{% highlight ruby %}
{
  "angularCompilerOptions": {
    "fullTemplateTypeCheck": true,
    "strictInjectionParameters": true
  },
}
{% endhighlight %}

#### 后言
希望本文会对你有所帮助。


