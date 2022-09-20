---
layout: post
title: js处理IOS虚拟键盘弹出后输入框被遮住
date: 2022-09-20 09:59:24.000000000 +09:00
tags:  JS IOS 
---

**关键词** JS IOS

### 前言
在项目开发的过程中，在IOS手机端系统下，当对输入框（input/textarea）进行focus操作时，键盘弹起遮住输入框。

### 问题描述
1. 从页面底部focus输入框失败
2. 从页面中间focus输入框失败

### 原因
造成上述问题的：键盘弹起事件和输入框滚动到浏览器窗口的可见区域的事件有冲突。

### 解决步骤
根据不同的情况有不同的解决办法：
1. 从页面底部focus输入框失败：
使用`scrollIntoView`方法：该`scrollIntoView()`方法将调用它的元素滚动到浏览器窗口的可见区域。
{% highlight ruby %}
setTimeout(() => {
    document.activeElement.scrollIntoView()
}, 200)
{% endhighlight %}

- 注意：这里需要使用settimeout去异步请求

2. 从页面中间focus输入框失败：
{% highlight ruby %}

 let screenHeight = window.innerHeight;
 let keyboardHeight = 0
 let time = 0;
 // 获取键盘高度
 let interval  = setInterval(() => {
    if (screenHeight !== window.innerHeight) {                
          keyboardHeight = screenHeight-window.innerHeight;
          clearInterval(interval)
       }
  }, 50);

// 将输入框复原到原位
let timer = setInterval(() => {
  time++;
  if(time <=5) {
     if(keyboardHeight) {
        document.documentElement.scrollTop = keyboardHeight + 110
      }
   } else {
     clearInterval(timer)
   }
 }, 50);
{% endhighlight %}

### 后言
最近工作很忙，没有时间更新。希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通