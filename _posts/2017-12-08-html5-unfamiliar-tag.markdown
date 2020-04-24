---
layout: post
title: HTML5 中不常用的标签
date: 2017-12-08 14:36:24.000000000 +09:00
---

### 前言

本文中记录了几个在平时的工作中不常用到的 HTML5 标签。

#### 一. 上标标签

`<sup>1</sup>`
<br>

#### 二. 下标标签

`<sub>1</sub>`
<br>

#### 三. 摘要与作用标签

> 作用：允许将页面的内容进行展开或者收缩。

{% highlight ruby %}

<details>
  <summary>
    标题
  </summary>
     Lorem ipsum dolor sit amet consectetur adipisicing elit.
 </details>
{% endhighlight %}

##### 效果

<details>
  <summary>
    标题
  </summary>
     Lorem ipsum dolor sit amet consectetur adipisicing elit.
 </details>
<br>

#### 四. 度量元素（电池电量

> 作用： 用于显示静态比例的信息。

`<meter value="0.59" min="0" max="1"></meter>`

##### 属性

- 1. min 最小值默认为 0。
- 2. max 最大值，默认为 1。
- 3. value 当前显示的度量值，默认为 0。

##### 效果

<meter value="0.59" min="0" max="1"></meter>
<br>

#### 五. 高亮文本显示（黄色底）

> 作用： 将页面中的某部分文本，以特殊的背景颜色显示出来。

`<mark> 高亮显示文本 </mark>`

###### 效果

<mark> 高亮显示文本 </mark>
