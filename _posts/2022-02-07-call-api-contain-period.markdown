---
layout: post
title: 如何发起参数里面有句号的请求
date: 2022-02-07 16:07:24.000000000 +09:00
tags: API
---

**关键词** API

### 前言
在项目过程中，在发送WS请求的时候，需要传递一个带句号的参数，本文介绍了如何发起这样的请求。

### 步骤
在参数后加'/\'用来转义，详细代码如下

{% highlight ruby %}
 getCount(cwsLogin: string): Observable<ICount[]> {
    return this.apiService
      .get(`/count/${cwsLogin}\/`) // 注：\反斜线用来转义
      .pipe(
        map((res: any) => res as ICount[]),
      );
  }
{% endhighlight %}


### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通