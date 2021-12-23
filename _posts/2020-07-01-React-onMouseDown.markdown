---
layout: post
title: React的onMouseDown，onBlur和onClick执行顺序问题
date: 2020-06-04 16:07:24.000000000 +09:00
---

**关键词** React MouseDown onBlur onClick

### 前言
本篇文章提供了一种解决按钮点击事件冲突的解决办法，希望能与大家分享沟通。

#### 冲突描述
当输入框有`onblur`事件的时候，在`onblur`的同时，点击提交按钮，会同时触发按钮的`onclick`事件和输入框的`onblur`事件。为了避免这种同时触发的情况，可以给button按钮添加以下代码：
```
onmousedown={e => {
  e.preventDefault();
}}
```
原理是事件触发顺序的不同 `onmousedown` => `onblur`=> `onclick`。

##### 错误场景：
`onblur`事件会对表单的输入框进行验证，若验证不通过，会在输入框下方显示错误信息。这时，在表单底部的提交按钮会产生向下的位移。

随后改正输入框信息，使之能通过验证，这时点击表单提交按钮，会同时触发`onblur`和`onclick`事件。因为验证通过，输入框下方的错误信息消失，会使表单提交按钮向上位移，这时如果点击提交按钮之前的位置，会无法触发点击事件。

具体情形如下图所示：

1. 初始状态
![image.png](https://upload-images.jianshu.io/upload_images/11992590-e69b2d077c772970.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.  输入错误信息并触发输入框的`onblur`事件，表单提交按钮发生向下位移，位移高度为错误信息文字高度。
![image.png](https://upload-images.jianshu.io/upload_images/11992590-1818c3e6113b5c91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 随后输入正确信息，点击提交按钮，先触发了`onblur`事件，错误信息消失，提交按钮向上位移，若鼠标点击在按钮位移之前的位置，会无法触发按钮的`onclick`事件。
![image.png](https://upload-images.jianshu.io/upload_images/11992590-172ccf0781d2bf53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 解决方案：
给button按钮添加以下代码：
{% highlight ruby %}
  onmousedown={e => {
    e.preventDefault();
  }}
{% endhighlight %}
原理是事件触发顺序的不同 `onmousedown` => `onblur`=> `onclick`。

>例外：但是如果按钮是一个封装的组件，如`react-route`的`Link`组件，这时候使用`onmousedown`并不会解决问题，原因是`Link`组件本身并没有`onclick`事件。

