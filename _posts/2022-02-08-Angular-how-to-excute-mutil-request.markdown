---
layout: post
title: 如何在Angular里执行多个内嵌请求
date: 2022-02-08 16:07:24.000000000 +09:00
tags: Angular2+
---

**关键词** Angular2+

### 前言
在项目过程中，在发送WS请求的时候，需要多个内嵌请求都结束后，再进行后续操作，除了promise以外，本文介绍了另外一种方法。

### 步骤
1.新建一个数组
{% highlight ruby %}
let requestList = [];
{% endhighlight %}
2.用mergeMap方法进行内嵌操作，并push到新建的数组中
{% highlight ruby %}
 requestList.push(
    this.sevice.createDemo(this.id, demo)
      .pipe(
        mergeMap(item =>
          this.sevice.createAnotherDemo(this.id, item)
        )))
  }
{% endhighlight %}
3.发起请求
{% highlight ruby %}
 forkJoin(...requestList).subscribe(
  () => {
    //do something
  },
  (err) => {
    // catch error
  }
);
{% endhighlight %}

### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通