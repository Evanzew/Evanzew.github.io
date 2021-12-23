---
layout: post
title: CSS IE浏览器自带的清除按钮
date: 2019-09-23 16:07:24.000000000 +09:00
tags: CSS IE
---

最近在项目中遇到了一个浏览器自带样式的问题，在IE浏览器中，Input输入框会自带清除按钮，会与项目中写好的清除按钮发生冲突。

### 解决办法：
给Input添加以下样式：
{% highlight ruby %}
::-ms-clear {
  width : 0;
  height: 0;
}
{% endhighlight %}
