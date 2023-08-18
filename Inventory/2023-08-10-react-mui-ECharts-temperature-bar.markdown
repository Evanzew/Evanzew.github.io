<!-- ---
layout: post
title: 使用React 18、Echarts和MUI实现温度计
date: 2023-08-10 09:59:24.000000000 +09:00
tags:  React Echarts MUI
---


**关键词** `React 18` `Echarts` `MUI`

### 摘要
在本文中，我们将结合使用`React 18`、`Echarts`和`MUI（Material-UI）`库，展示如何实现一个交互性的温度计。我们将使用`Echarts`绘制温度计的外观，并使用`MUI`创建一个漂亮的用户界面。
本文将详细介绍实现温度计所需的关键代码以及其他必要的步骤，本文会尽可能的提供详细的注释。

### 完成后的样式
![温度计](/assets/images/temperaturebar.jpg '温度计')

### 关键代码

{% highlight ruby %}
import React from 'react';
import * as echarts from 'echarts/core';
import { EChartOption \} from '../../EChartOption';
import CommonChart from '../../CommonChart';
import { Box \} from '@mui/material';

interface TemperatureBarProps {
  deviceData: any;
\}

const MAX_TEMPERATURE_SOCPE = 120; //温度上限
const MIN_TEMPERATURE_SOCPE = -40; // 温度下限

/**
 * 温度图表组件
 */
const TemperatureChart = () => {
  // 温度数值
  let TPvalue = MAX_TEMPERATURE_SOCPE;
  // 刻度数据
  let kd = [];
  // 渐变色配置
  let Gradient = [];

  // 创建刻度数据
  for (
    let i = 0, len = MAX_TEMPERATURE_SOCPE - MIN_TEMPERATURE_SOCPE;
    i <= len;
    i += 1
  ) {
    if (i % 20 === 10) {
      kd.push('');
    \} else if (i % 40 === 0) {
      kd.push('-48');
    \} else if (i % 8 === 0) {
      kd.push('-28');
    \} else {
      kd.push('');
    \}
  \}

  // 根据温度数值设置渐变色和文本内容
  if (TPvalue > 20) {
    Gradient.push(
      {
        offset: 0,
        color: '#93FE94'
      \},
      {
        offset: 0.5,
        color: '#E4D225'
      \},
      {
        offset: 1,
        color: '#E01F28'
      \}
    );
  \} else if (TPvalue > -20) {
    Gradient.push(
      {
        offset: 0,
        color: '#93FE94'
      \},
      {
        offset: 1,
        color: '#E4D225'
      \}
    );
  \} else {
    Gradient.push({
      offset: 1,
      color: '#93FE94'
    \});
  \}

  // 温度图表配置选项
  const option = {
    animation: false, // 禁止动画效果
    title: {
      text: '  ℃',
      top: '5px',
      left: 'center',
      textStyle: {
        color: '#fff',
        fontStyle: 'normal',
        fontWeight: 'normal',
        fontSize: '16px',
        padding: 5
      \}
    \},
    grid: {
      left: '45', // 图表距离容器左边的距离
      bottom: 20, // 图表距离容器底部的距离
      top: 30 // 图表距离容器顶部的距离
    \},
    yAxis: [
      {
        show: false, // 不显示y轴
        data: [], // y轴的数据
        min: 0, // y轴的最小值
        max: MAX_TEMPERATURE_SOCPE - MIN_TEMPERATURE_SOCPE + 10, // y轴的最大值
        axisLine: {
          show: false // 不显示y轴的轴线
        \}
      \},
      {
        show: false, // 不显示y轴
        min: 0, // y轴的最小值
        max: MAX_TEMPERATURE_SOCPE - MIN_TEMPERATURE_SOCPE // y轴的最大值
      \},
      {
        type: 'category', // 类型为分类
        position: 'left', // y轴的位置在左边
        offset: -80, // y轴与图表的偏移量
        axisLabel: {
          fontSize: 10, // y轴标签的字体大小
          color: 'white' // y轴标签的颜色为白色
        \},
        axisLine: {
          show: false // 不显示y轴的轴线
        \},
        axisTick: {
          show: false // 不显示y轴的刻度线
        \}
      \}
    ],
    xAxis: [
      {
        show: false, // 不显示x轴
        min: -50, // x轴的最小值
        max: 80, // x轴的最大值
        data: [] // x轴的数据
      \},
      {
        show: false, // 不显示x轴
        min: -48, // x轴的最小值
        max: 120 // x轴的最大值
      \}
    ],
    series: [
      {
        name: '条', // 数据项名称
        type: 'bar', // 图表类型为柱状图
        xAxisIndex: 0, // 与第一个x轴关联
        data: [MAX_TEMPERATURE_SOCPE - MIN_TEMPERATURE_SOCPE + 10], // 柱状图的数据
        barWidth: 18, // 柱状图的宽度,
        label: {
          show: true // 显示标签
        \},
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [
            {
              offset: 0.05,
              color: '#5087EC' // 渐变颜色的起始色
            \},
            {
              offset: 0.5,
              color: '#58DBA4' // 渐变颜色的起始色
            \},
            {
              offset: 0.6,
              color: '#FFF81D' // 渐变颜色的中间色
            \},
            {
              offset: 0.8,
              color: '#FA9917' // 渐变颜色的中间色
            \},
            {
              offset: 1,
              color: '#FF4D4F' // 渐变颜色的结束色
            \}
          ]),
          borderRadius: [8, 8, 2, 2] // 柱状图的圆角样式
        \},
        z: 2 // 数据系列层叠的顺序值
      \},
      {
        name: '圆', // 数据项名称
        type: 'scatter', // 图表类型为散点图
        hoverAnimation: false, // 禁止散点图的Hover动画效果
        data: [0], // 散点图的数据
        xAxisIndex: 0, // 与第一个x轴关联
        symbolSize: 18, // 散点图的符号大小
        itemStyle: {
          color: '#5087EC', // 散点图的颜色
          opacity: 1 // 散点图的透明度
        \},
        z: 2 // 数据系列层叠的顺序值
      \},
      {
        name: '刻度', // 数据项名称
        type: 'bar', // 图表类型为柱状图
        yAxisIndex: 0, // 与第一个y轴关联
        xAxisIndex: 1, // 与第二个x轴关联
        label: {
          show: true, // 显示标签
          position: 'left', // 标签的位置在左边
          distance: 10, // 标签与柱状图的距离
          color: 'white', // 标签的颜色为白色
          fontSize: 14, // 标签的字体大小
          formatter: function (params) {
            if (
              params.dataIndex >
              MAX_TEMPERATURE_SOCPE - MIN_TEMPERATURE_SOCPE
            ) {
              return '';
            \}
            if (params.dataIndex % 20 === 0) {
              return params.dataIndex + MIN_TEMPERATURE_SOCPE;
            \}
            return '';
          \} // 标签的格式化函数
        \},
        barGap: '-100%', // 柱状图之间的距离
        data: kd, // 柱状图的数据
        barWidth: 0.5, // 柱状图的宽度
        itemStyle: {
          color: 'white', // 柱状图的颜色为白色
          barBorderRadius: 120 // 柱状图的圆角样式
        \},
        z: 0 // 数据系列层叠的顺序值
      \}
    ]
  \} as EChartOption;

  // 返回渲染图表的组件
  return <CommonChart option={option\} width="100%" height="100%" />;
\};

export default function TemperatureBar() {
  const [maxTemperature, setMaxTemperature] = React.useState<number>(80); // 定义最大温度的状态值，默认为80
  const [minTemperature, seMinTemperature] = React.useState<number>(-20); // 定义最小温度的状态值，默认为-20

  return (
    <Box
      ref={parentRef\}
      sx={{
        display: 'flex',
        height: '100%',
        alignItems: 'center',
        justifyContent: 'center',
        position: 'relative',
        color: '#fff'
      \}\}
    >
      {isMinHieght ?
        <Box
          sx={{
            display: 'flex',
            flexDirection: 'column',
            textAlign: 'left'
          \}\}
        >
          <Box
            sx={{
              display: 'flex',
              alignItems: 'center',
              mb: 2
            \}\}
          >
            <Box
              sx={{
                borderLeft: '10px solid transparent',
                borderRight: '10px solid transparent',
                borderBottom: '20px solid #FF4D4F',
                width: 0,
                height: 0,
                display: 'inline-block'
              \}\}
            ></Box>
            <span
              style={{
                paddingLeft: '4px'
              \}\}
            >
             最高温度
              {parseFloat(String(maxTemperature)).toFixed(1)\}℃
            </span>
          </Box>
          <Box
            sx={{
              display: 'flex',
              alignItems: 'center'
            \}\}
          >
            <Box
              sx={{
                borderLeft: '10px solid transparent',
                borderRight: '10px solid transparent',
                borderTop: '20px solid #5087EC',
                width: 0,
                height: 0,
                display: 'inline-block'
              \}\}
            ></Box>
            <span
              style={{
                paddingLeft: '4px'
              \}\}
            >
             最低温度
              {parseFloat(String(minTemperature)).toFixed(1)\}℃
            </span>
          </Box>
        </Box> :
        <Box
          sx={{
            width: 'calc(100% - 80px)',
            maxWidth: '140px',
            height: '80%',
            background: '#363636',
            borderRadius: '8px',
            position: 'relative',
            boxShadow: '2px 2px 8px 0px rgba(0, 0, 0, 0.7)'
          \}\}
        >
          <Box
            sx={{
              position: 'absolute',
              top: '-25px',
              right: '-30px',
              display: 'flex',
              alignItems: 'center',
              fontSize: '12px'
            \}\}
          >
            <Box
              sx={{
                marginRight: '10px',
                display: 'flex',
                alignItems: 'center'
              \}\}
            >
              <Box
                sx={{
                  borderLeft: '8px solid transparent',
                  borderRight: '8px solid transparent',
                  borderBottom: '14px solid #FF4D4F',
                  width: 0,
                  height: 0,
                  display: 'inline-block'
                \}\}
              ></Box>
              <span
                style={{
                  paddingLeft: '4px'
                \}\}
              >
                最高
              </span>
            </Box>
            <Box
              sx={{
                display: 'flex',
                alignItems: 'center'
              \}\}
            >
              <Box
                sx={{
                  borderLeft: '8px solid transparent',
                  borderRight: '8px solid transparent',
                  borderTop: '14px solid #5087EC',
                  width: 0,
                  height: 0,
                  display: 'inline-block'
                \}\}
              ></Box>
              <span
                style={{
                  paddingLeft: '4px'
                \}\}
              >
                最小
              </span>
            </Box>
          </Box>
          <Box
            sx={{
              position: 'absolute',
              width: 'calc(50% + 20px)',
              margin: 0,
              left: '50%',
              top: `calc(42px + (100% - 62px) * (${MAX_TEMPERATURE_SOCPE\}  - ${maxTemperature\}) / ${
                MAX_TEMPERATURE_SOCPE - MIN_TEMPERATURE_SOCPE
              \})`,
              transition: 'top 0.3s ease'
            \}\}
          >
            <hr
              style={{
                position: 'relative',
                margin: 0,
                color: '#FF4D4F',
                border: 'none',
                borderTop:  '1px solid #FF4D4F' 
              \}\}
            />
            <Box
              sx={{
                position: 'absolute',
                left: 'calc(100% - 10px)',
                top: '-26px',
                borderLeft: '10px solid transparent',
                borderRight: '10px solid transparent',
                borderBottom:  '16px solid #FF4D4F' 
                width: 0,
                height: 0,
                display: 'flex',
                justifyContent: 'center',
                paddingBottom: '18px'
              \}\}
            >
              {parseFloat(String(maxTemperature)).toFixed(1)\}
            </Box>
          </Box>
          <Box
            sx={{
              position: 'absolute',
              margin: 0,
              width: 'calc(50% + 20px)',
              left: '50%',
              top: `calc(42px + (100% - 62px) * (${MAX_TEMPERATURE_SOCPE\}  - ${minTemperature\}) / ${
                MAX_TEMPERATURE_SOCPE - MIN_TEMPERATURE_SOCPE
              \})`,
              transition: 'top 0.3s ease'
            \}\}
          >
            <hr
              style={{
                position: 'relative',
                margin: 0,
                border: 'none',
                borderTop:  '1px solid #5087EC' 
              \}\}
            />
            <Box
              sx={{
                position: 'absolute',
                left: 'calc(100% - 10px)',
                top: '-8px',
                borderLeft: '10px solid transparent',
                borderRight: '10px solid transparent',
                borderTop:  '16px solid #5087EC'
                width: 0,
                height: 0,
                display: 'flex',
                justifyContent: 'center',
                paddingTop: '3px'
              \}\}
            >
              {parseFloat(String(minTemperature)).toFixed(1)\}
            </Box>
          </Box>
          <TemperatureChart />
        </Box>
      \}
    </Box>
  );
\}

{% endhighlight %}

### 后言
 在本文中，我们使用`React 18`、`Echarts`和`MUI`库展示了如何实现一个交互性的温度计。我们通过创建一个温度计组件，并使用`Echarts`库绘制温度计的外观。使用`MUI`库，我们创建了一个漂亮的用户界面来容纳温度计。如果不使用`MUI`，只需要把`MUI`相关标签改成`HTML`标签即可 -->