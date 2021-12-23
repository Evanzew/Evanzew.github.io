---
layout: post
title: CSS z-index失效问题
date: 2018-02-11 14:36:24.000000000 +09:00
tags: CSS
---

### 前言

z-index在CSS里一直是比较神奇的一种属性，在项目过程中，总是会出现时灵时不灵的状况，以下是我总结的几类失效原因，希望与大家一起分享。

### 原因

- 父标签 position属性为relative。
	
- 问题标签无position属性（不包括static）。
	
- 问题标签含有浮动(float)属性。