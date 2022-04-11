---
layout: post
title: React Ant Design Pro 之如何删除左上角的logo
date: 2022-04-11 09:59:24.000000000 +09:00
tags: React AntDesign 
---

**关键词** React AntDesign Pro

### 前言
在项目开发的过程中，客户没有设置自己的网站logo，想要将网站左上角的logo删除。

### 问题
查询项目代码，发现logo属性只能是字符串或者是`undefined`, 如果将项目代码中的logo属性设置为空字符串或者`undefined`, 网站会自动调用Antd的网站logo而不是删除左上角logo，本文介绍了解决办法。

### 步骤
1. 在app.tsx里添加`logo: false`代码, 如下所示：
{% highlight ruby %}
export const layout: RunTimeLayoutConfig = ({ initialState }) => {
  return {
    ...
    logo: false,
    ...
  };
};
{% endhighlight %}

### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通