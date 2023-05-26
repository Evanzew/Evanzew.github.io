---
layout: post
title: React-hook-form-mui （一）：基本使用
date: 2023-05-20 09:59:24.000000000 +09:00
tags:  React MUI react-hook-form-mui
---

**关键词** `React` `MUI`  `react-hook-form-mui`

### 前言
在项目开发中，我们选择了`React`+`MUI`作为技术栈。在使用`MUI`构建`form`表单时，我们发现并没有与`antd`类似的表单验证功能，于是我们选择了MUI推荐使用的`react-hook-form-mui`库去进行验证。但是发现网上关于这个库的使用方法和demo比较少且比较简单，并没有复杂的表单验证的demo。
因此本文及以下几篇文章，会从简到难讲解如何使用。希望通过这几篇文章的介绍，能够帮助你入门`react-hook-form-mui`

### 优势介绍
`react-hook-form-mui`可以帮助开发人员更轻松地构建表单，它结合了`React Hook Form`和`Material-UI`组件库。它提供了一些预定义的表单组件，如`TextFieldElement`、`CheckboxElement`、等，可以直接使用。此外，它还提供了一些自定义的表单组件，如`PasswordElement`、`DatePickerElemen`t等，可以根据需要进行定制。
使用`react-hook-form-mui`，开发人员可以更快速地构建表单，并且可以轻松地进行表单验证和数据处理。

### 简单Demo
下面是一个以`React` `MUI`  `react-hook-form-mui`简单用例

{% highlight ruby %}
import React from 'react';
import { useForm } from 'react-hook-form';
import { Button } from '@mui/material';
import { FormContainer, TextFieldElement } from 'react-hook-form-mui';

// 定义表单数据类型
export interface UserSettings{
  firstName: string;
  lastName: string;
}

const MyForm = () => {
  // 使用 useForm 声明一个 formContext
  const formContext = useForm<UserSettings>({
    // 初始化表单数据
    defaultValues: {
      firstName: '',
      lastName: ''
    }
  });

  // 表单提交时的回调函数
  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    // 使用 FormContainer 包裹表单组件
    <FormContainer
        formContext={formContext}
        // 表单提交成功时的回调函数
        onSuccess={(data) => {
          onSubmit (data);
        }}
      >
        {/* 使用 TextFieldElement 渲染表单组件 */}
        <TextFieldElement name="firstName" label="First Name" />
        <TextFieldElement name="lastName" label="Last Name" />
        <Button type="submit">Submit</Button>
    </FormContainer>
  );
};

export default MyForm;
{% endhighlight %}

首先，我们通过`useForm`来声明一个`formContext`, 在`formContext`可以声明`defaultValues`来初始化`form`表单的值。

其次, 在`formContainer`中，声明`onSuccess`方法，当点击`type=‘submit’`，按钮时，会回调用其中的方法。`onSuccess`方法中的`data`参数，会返回和`defaultValues`中一样的数据类型。


#### 总结
以上是关于`React-hook-form-mu`的最基础的用法。希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通。