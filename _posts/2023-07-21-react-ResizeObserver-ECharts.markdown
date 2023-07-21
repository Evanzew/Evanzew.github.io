---
layout: post
title: 使用React和ResizeObserver实现自适应ECharts图表
date: 2023-07-21 09:59:24.000000000 +09:00
tags:  React ResizeObserver ECharts
---


**关键词** `React`   `ECharts` `ResizeObserver`

### 摘要
在现代 Web 开发中，响应式布局和数据可视化是非常常见的需求。本文将介绍如何使用React、ResizeObserver和ECharts库来创建一个自适应的图表组件。

## 什么是ResizeObserver
ResizeObserver是JavaScript的一个API，用于监测元素的大小变化。它可以在元素大小发生变化时触发回调函数，使我们能够及时地作出相应的调整。

## 为什么使用ResizeObserver
在响应式布局中，我们经常需要根据容器的大小调整图表的尺寸。传统的方式是使用定时器或者事件监听器来检测容器大小的变化，但这些方法效率较低且不够精确。

ResizeObserver提供了一种更高效和准确的方式来监测元素的大小变化。它能够实时地感知元素的大小改变，并立即触发回调函数。

## 使用React创建图表组件

首先，我们将使用React来创建一个基本的图表组件。我们将使用ECharts作为数据可视化库来渲染图表。

{% highlight ruby %}
import React, { useEffect, useRef } from 'react';
import echarts from 'echarts';

const Chart = ({ data }) => {
  const chartRef = useRef(null);

  useEffect(() => {
    const chart = echarts.init(chartRef.current);

    const options = {
      // 配置图表选项
      // ...
    };

    chart.setOption(options);

    return () => {
      chart.dispose(); // 销毁图表
    };
  }, [data]);

  return <div ref={chartRef} style={{ width: '100%', height: '400px' }} />;
};

export default Chart;

{% endhighlight %}

在上面的代码中，我们创建了一个名为`Chart`的函数组件。组件接收一个名为data的属性，它用于更新图表的数据。

在组件的`useEffect`钩子函数中，我们初始化了`ECharts`实例，并通过`setOption`方法设置图表的选项。我们还在组件卸载时使用`dispose`方法销毁了图表实例，以释放资源。

组件的返回部分包含一个`<div>`元素，我们使用`ref`属性将其与`chartRef`关联起来。这个`<div>`元素将作为ECharts图表的容器，并且我们为其设置了宽度为100%和高度为400像素，你可以根据需要调整这些值。


## 使用ResizeObserver监听容器大小变化

现在，我们要使用ResizeObserver来监听图表容器的大小变化，并在大小发生变化时重新渲染图表。

为此，我们将使用一个名为`useResizeObserver`的自定义Hook，它使用ResizeObserver API来监听元素的大小变化。

{% highlight ruby %}

import { useEffect, useState } from 'react';

const useResizeObserver = (ref) => {
  const [dimensions, setDimensions] = useState(null);

  useEffect(() => {
    const observer = new ResizeObserver((entries) => {
      const { width, height } = entries[0].contentRect;
      setDimensions({ width, height });
    });

    observer.observe(ref.current);

    return () => {
      observer.unobserve(ref.current);
    };
  }, [ref]);

  return dimensions;
};

export default useResizeObserver;

{% endhighlight %}

在上面的代码中，我们定义了一个名为`useResizeObserver`的Hook。它接收一个`ref`作为参数，该`ref`引用了要监听的元素。每当元素的大小发生变化时，我们会更新`dimensions`状态，以便我们能够在组件中获取到最新的宽度和高度。

现在，我们可以在我们的`Chart`组件中使用`useResizeObserver`来监听容器的大小变化，并相应地重新渲染图表。

{% highlight ruby %}

import React, { useEffect, useRef } from 'react';
import echarts from 'echarts';
import useResizeObserver from './useResizeObserver';

const Chart = ({ data }) => {
  const chartRef = useRef(null);
  const dimensions = useResizeObserver(chartRef);

  useEffect(() => {
    const chart = echarts.init(chartRef.current);

    const options = {
      // 配置图表选项
      // ...
    };

    chart.setOption(options);

    return () => {
      chart.dispose();
    };
  }, [data, dimensions]);

  return <div ref={chartRef} style={{ width: '100%', height: '400px' }} />;
};

export default Chart;


{% endhighlight %}

在上述示例代码中，我们从`useResizeObserver`钩子中获取到最新的`dimensions`值，并将其添加到`useEffect`的依赖数组中。这意味着每当容器的大小发生变化时，我们都会重新执行副作用函数并重新渲染图表。

这样，当图表容器的大小发生变化时，图表将自动根据新的尺寸重新绘制，以便适应新的布局。

## 结论

通过使用React、ResizeObserver和ECharts，我们可以轻松地创建自适应的图表组件。借助ResizeObserver，我们可以有效地监听元素大小的变化，而不需要使用定时器或事件监听器。

希望本文对你理解如何使用React、ResizeObserver和ECharts来创建自适应的图表有所帮助。你可以在你的项目中尝试并根据自己的需求来定制图表组件。