---
layout: post
title: React 手机端没有:active伪类的解决办法
date: 2020-07-29 16:07:24.000000000 +09:00
tags: React
---

**关键词** React MouseDown onBlur onClick

### 问题描述

手机端是没有`:active`伪类，如果要像PC端一样显示`:active`的CSS效果的话，给元素添加`onTouchEnd()=>{}`即可。


