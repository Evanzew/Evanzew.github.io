---
layout: post
title: Angular11 MSAL B2C登录实例(二) 
date: 2021-03-16 16:07:24.000000000 +09:00
tags: Auglar11
---

**关键词** Angular 11 B2C MSAL

### 前言
上文介绍了在app.module.ts里的配置，本文着重讲解下在app-routing.module.ts和index.html里的设置。

### 步骤
在文件中主要需要添加以下代码
> app-routing.module.ts

{% highlight ruby %}

const initialNavigation = (!BrowserUtils.isInIframe() && !BrowserUtils.isInPopup()) || window.location.href.indexOf("logout") > 0;

.@NgModule({
  imports: [RouterModule.forRoot(routes, {
    useHash: true,
    // Don't perform initial navigation in iframes or popups, except for logout
    initialNavigation: initialNavigation ? 'enabled' : 'disabled' // Remove this line to use Angular Universal
  })],
  exports: [RouterModule]
})
{% endhighlight %}
> index.html

{% highlight ruby %}
<body>
...
  <app-redirect></app-redirect>
</body>
{% endhighlight %}

### 后言
下一篇文章会讲解在在组件中的使用


