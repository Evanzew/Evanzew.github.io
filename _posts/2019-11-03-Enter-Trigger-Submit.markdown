---
layout: post
title: React 单一Input回车触发表单提交事件 踩坑
date: 2019-11-03 16:07:24.000000000 +09:00
tags: React
---

当表单内只有一个`input`输入框时，即使表单没有`submit`按钮，在输入框内按回车就会触发表单的提交事件。
>会触发submt

```
<form id="form1" method="POST">
    <p>Does submit:</p>
    <input type="text"/>
</form>
```

>不会触发submit

```
<form id="form2" method="POST">
    <p>Does <strong>not</strong> submit:</p>
    <input type="text"/>
    <input type="text"/>
</form>
```

##### 解决办法：

1. 若表单只有一个输入框，可以不包含在`form`元素里。
2. 再添加一个输入框，并加上`display: none`的属性。
{% highlight ruby %}
<form id="form2" method="POST">
    <p>Does <strong>not</strong> submit:</p>
    <input type="text"/>
    <input type="text" style={{ display: 'none' }} />
</form>
{% endhighlight %}