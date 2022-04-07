---
layout: post
title: Angular 2+ 如何向不关联组件传入数据
date: 2022-01-01 16:07:24.000000000 +09:00
tags: Angular 2+
---

**关键词** Angular 2+

### 前言
众所周知，angular 2+向子组件传递数据用@Input(), 子组件向父组件传递数据用@Output()。现在因为项目需求，需要在angular 2+的不关联组建中传递数据，本文详细介绍了具体步骤和代码。

### 步骤
1.在service文件添加如下代码
{% highlight ruby %}
private idSubject: BehaviorSubject<number> = new BehaviorSubject<number>(null);
public id = this.idSubject.asObservable();

setCurrentId(id: number = null) {
  this.idSubject.next(id);
}
{% endhighlight %}
2.在需要传值的地方call setCurrentId
{% highlight ruby %}

this.service.setCurrentId(xx) // xx代表要传入的id

{% endhighlight %}
3.在需要调用id的地方添加如下代码
{% highlight ruby %}

private ngUnsubscribe = new Subject<void>();
...
 ngOnInit(): void {
    this.service.id
      .pipe(takeUntil(this.ngUnsubscribe))
      .subscribe((id) => {
        if(id) {
         ....
        }
      });
  }
  
{% endhighlight %}


### 后言
希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通