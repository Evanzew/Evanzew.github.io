---
layout: post
title: React的onMouseDown，onBlur和onClick执行顺序问题 踩坑
date: 2019-08-28 16:07:24.000000000 +09:00
---

最近在项目中遇到了一个因为onBlur和onClick执行顺序不同而造成的bug，希望同大家分享一下。

### 原理

当输入框有onBlur事件的时候，在触发onBlur事件的同时，点击按钮，会同时触发按钮的onClick事件和输入框的onBlur事件，而onBlur事件的顺序大于onClick事件，此时可能会造成一些bug。

### 具体案例

#### 1. 初始状态

![First](/assets/images/React-Onblur/First.png)


#### 2. 第一步
输入错误信息并触发输入框的onBlur事件，表单提交按钮发生向下位移，位移高度为错误信息文字高度。

![Second](/assets/images/React-Onblur/Second.png)


#### 3. 第二步

随后输入正确信息，点击提交按钮，先触发了onBlur事件，错误信息消失，提交按钮向上位移，若鼠标点击在按钮位移之前的位置，会无法触发按钮的onClick事件。

![Third](/assets/images/React-Onblur/Third.png)


### 解决方案

给按钮添加onmousedown事件:
```
onMouseDown={e => {
    e.preventDefault();
}}
```
>使用原理： 事件触发顺序 

` onMouseDown > onBlur > onClick `




#### 例外

> 但是如果按钮是一个封装的组件，如react-route的Link组件，这时候使用onMouseDown并不会解决问题。建议将Link换成button或者a标签。