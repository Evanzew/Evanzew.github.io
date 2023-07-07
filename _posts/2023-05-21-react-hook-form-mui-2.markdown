---
layout: post
title: React-hook-form-mui （二）：表单数据处理
date: 2023-05-21 09:59:24.000000000 +09:00
tags:  React MUI react-hook-form-mui
---

**关键词** `React` `MUI`  `react-hook-form-mui`

### 前言
在上一篇文章中，我们介绍了`react-hook-form-mui`的基础用法。本文将着表单数据处理。
`react-hook-form-mui`提供了丰富的表单数据处理功能，可以通过`watch`属性来获取表单数据。
### Demo
下面是一个使用`watch`属性的例子：

{% highlight ruby %}
import React from 'react';
import { useForm } from 'react-hook-form';
import { Button } from '@mui/material';
import { FormContainer, TextFieldElement } from 'react-hook-form-mui';

export interface UserSettings {
  firstName: string;
  lastName: string;
}

const MyForm = () => {
  const formContext = useForm<UserSettings>({
    defaultValues: {
      firstName: '',
      lastName: ''
    }
  });

  // 使用 watch 属性获取表单数据
  const { watch } = formContext;
  const formData = watch();

  const onSubmit = (data) => {
    console.log(data);
  };


  const handleGetDataClick = () => {
    const { firstName, lastName }= formData;

    console.log(firstName); //输出 firstName
    console.log(lastName); //输出 lastName
    console.log(formData); //输出 { firstName: xx, lastName: xx }
  };

  return (
    <FormContainer
      formContext={formContext}
      onSuccess={(data) => {
        onSubmit(data);
      }}
    >
      <TextFieldElement name="firstName" label="First Name" />
      <TextFieldElement name="lastName" label="Last Name" />
      <Button onClick={handleGetDataClick}>Get Form Data</Button>
      <Button type="submit">Submit</Button>
    </FormContainer>
  );
};

export default MyForm;
{% endhighlight %}

从以上的demo不难看出，我们能够通过`watch`很轻易的去获取表单元素的值，而不需要通过`useState`的方式去获取，能够减少项目中的不必要的代码。

### 总结
以上是关于`React-hook-form-mui`的表单数据处理。希望本文会对你有所帮助。如果有什么问题，可在下方留言沟通。