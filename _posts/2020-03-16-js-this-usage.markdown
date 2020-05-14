---
layout: post
title: JS this的用法
date: 2020-03-16 16:07:24.000000000 +09:00
---

**关键词**：JavaScript  this

### 前言
本文总结了几类JavaScript中this的用法，同大家一起分享。

#### 1. object.function()
function中的this指向的是object。

#### 2. New Function() 构造函数
Function()中的this只想的是正在创建的新对象。

#### 3. function() 和匿名函数
this指向的是window。

#### 4. xxx.prototype.function()
function 中的this只想的是将来调用的点前的对象。

