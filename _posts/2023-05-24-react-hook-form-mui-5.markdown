---
layout: post
title: React-hook-form-mui（五）：包含内嵌表单元素的表单
date: 2023-05-24 09:59:24.000000000 +09:00
tags:  React MUI react-hook-form-mui
---

**关键词** `React` `MUI`  `react-hook-form-mui`

### 前言
在上一篇文章中，我们介绍了`react-hook-form-mui`如何与与后端数据联调。在实际项目中，从后端获取的数据可能是复杂的数据对象，本文将介绍，如何通过`react-hook-form-mui`实现一个包含内嵌表单元素的表单

### Demo
以下代码实现了一个包含内嵌表单元素的表单的完整代码：

{% highlight ruby %}
import React from 'react';
import { useForm } from 'react-hook-form';
import { Button, MenuItem } from '@mui/material';
import { FormContainer, TextFieldElement } from 'react-hook-form-mui';

//内嵌表单元素
const InnerForm = ({ index }: any) => {
  return (
    <>
      <TextFieldElement name={`items[${index}].name`} label="Name" />
      <TextFieldElement
        name={`items[${index}].quantity`}
        label="Quantity"
        type="number"
      />
    </>
  );
};

const MyForm = () => {
  const formContext = useForm({
    defaultValues: {
      firstName: '',
      lastName: '',
      email: '',
      gender: '',
      age: '',
      items: [{ name: '', quantity: '' }]
    }
  });
  const { watch } = formContext;

  const onSubmit = (data) => {
    console.log(data);
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
      <TextFieldElement name="email" label="Email" />
      <TextFieldElement select name="gender" label="Gender">
        <MenuItem value="male">Male</MenuItem>
        <MenuItem value="female">Female</MenuItem>
      </TextFieldElement>
      <TextFieldElement name="age" label="Age" type="number" />
      {watch('items')?.map((_, index) =>
        <InnerForm key={index} index={index} />
      )}
      //像数组中插入表新的元素
      <Button
        type="button"
        onClick={() => watch('items').push({ name: '', quantity: '' })}
      >
        Add Item
      </Button>
      <Button type="submit">Submit</Button>
    </FormContainer>
  );
};

export default MyForm;

{% endhighlight %}

###解析
{% highlight ruby %}
//内嵌表单元素
const InnerForm = ({ index }: any) => {
  return (
    <>
      <TextFieldElement name={`items[${index}].name`} label="Name" />
      <TextFieldElement
        name={`items[${index}].quantity`}
        label="Quantity"
        type="number"
      />
    </>
  );
};
{% endhighlight %}
以上代码是实现内嵌表单元素的关键代码，了解以上代码，我们需要了解`react-hook-form-mui`的核心理念。它是通过获取表单元素的name，生成数据结构数。因此，对于内嵌的组件而言，我们需要通过index来给name赋值。这样就可以获取到内嵌表单元素的表单值。

### 总结
以上是关于`React-hook-form-mui`的内嵌表单元素的讲解。希望本文会对你有所帮助。如果有什么问题，可在下方留言沟通。