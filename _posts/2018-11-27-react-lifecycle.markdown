---
layout: post
title: React Lifecycle(生命周期)
date: 2018-11-27 11:27:24.000000000 +09:00
tags: React
---

按照我的理解，生命周期主要分为一下几个**步骤**：

> - componentWillMount()

在组件第一次渲染之前调用，这个是在 render 方法调用前可修改 state 的最后一次机会。这个方法基本不会用到。

> - render()

渲染虚拟 dom。

> - componentDidMount()

在组件第一次渲染后立即调用。一般会在这个方法里执行 ajax，setinterval，settimeout 等方法。因为 ajax 是异步请求，如果在 componentWillMount()里执行的话，可能会在元素渲染出来之前执行，那么就会造成 ajax 请求失败。

> - componentWillUnmount()

注销在 componentDidMount()里注册的方法和函数，释放内存。

> - compoentWillReceiveProps()

在组件接受到新的 props 时调用，每当组件更新 rprops 时就会被调用，初次渲染不会调用。在这里可以改变 state。

> - shouldComponentUpdate()

顾名思义，就是组件是否需要更新，是的话返回 true，否则返回 false，初次渲染不会调用。

> - componentWillUpdate()

组件接收到新的 props 或者 state 但是还未调用时执行，初次渲染不会调用。这里不能改变 state，或者是加判断改变 state。否则会一直 update，进入死循环。

> - render()

渲染虚拟 dom。

> - componnetDidUpdate()

在组件完成更新时立即调用，初次渲染不会调用。
