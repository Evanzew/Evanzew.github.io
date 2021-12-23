---
layout: post
title: Angular13 无法在浏览器debug
date: 2021-11-14 16:07:24.000000000 +09:00
tags: Angualr13
---

**关键词** Angular13 npm install

### 前言
本文介绍了在Angular 13中，无法在浏览器debug的一种解决办法。

#### 发生场景
根据项目需求，升级Angular13，结果本地环境无法在浏览器debug。

#### 问题原因
造成无法debug的原因是，当Angular13 npm start的时候，会自动选择production环境，从而无法在浏览器页面debug。

#### 解决办法
解决这个问题需要以下三个步骤：

1. 在angular.json的build的configuration中添加development对象
{% highlight ruby %}
 "configurations": {
            ...
            "development":{
              "sourceMap": true,
              "optimization": false
            },
}
{% endhighlight %}

2. 在angular.json的serve的configuration中添加development对象
{% highlight ruby %}
 "configurations": {
          ...
            "development": {
              "browserTarget": "<app-name>:build:development" //<app-name>需要修改为具体项目名称
            }
}
{% endhighlight %}

3. 修改package.json的scripts下的dev命令
{% highlight ruby %}
"dev": "...ng build --configuration=development && ng serve --configuration development "
{% endhighlight %}

#### 后言
希望本文会对你有所帮助。