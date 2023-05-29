---
layout: post
title: React-hook-form-mui（三）：表单验证
date: 2023-05-22 09:59:24.000000000 +09:00
tags:  React MUI react-hook-form-mui
---

**关键词** `React` `MUI`  `react-hook-form-mui`

### 前言
在上一篇文章中，我们介绍了`react-hook-form-mui`的基础用法。本文将着重讲解表单验证功能。
`react-hook-form-mui`提供了丰富的表单验证功能，可以通过`validation`属性来设置表单验证规则。本文将详细介绍`validation`的三种实现方法，以及如何与提交按钮联动。

### Demo
以下是一个表单验证的 demo，我们将通过三种方法来实现表单验证：

{% highlight ruby %}
import React from "react";
import { useForm } from "react-hook-form";
import { Button } from "@mui/material";
import { FormContainer, TextFieldElement } from "react-hook-form-mui";

const URL_REGEXP =
  \/^[A-Za-z][A-Za-z\d.+-]*:\/*(?:\w+(?::\w+)?@)?[^\s/]+(?::\d+)?(?:\/[\w#!:.,?+=&%@\-/]*)?$\/;

export interface UserSettings {
  firstName: string;
  lastName: string;
  url: string;
}

/**
 * @descpition: lastName长度验证
 * @param _value 当前表单元素的值
 */
const validateLastNameLength = (_value: string) => {
  return _value.length < 2 ? "Url is invalid!" as any : Promise.resolve();
};

const MyForm = () => {
  const formContext = useForm<UserSettings>({
    defaultValues: {
      firstName: "",
      lastName: "",
      url: ""
    },
    mode: "all" // 验证模式切换为all
  });

  const onSubmit = (data: UserSettings) => {
    console.log(data);
  };

  return (
    <FormContainer
      formContext={formContext}
      onSuccess={(data) => {
        onSubmit(data);
      }}
    >
      {/* 使用 validation 属性设置表单验证规则 */}
      <TextFieldElement
        name="firstName"
        label="First Name"
        validation={{
          required: "First Name is required!"
        }}
      />

      <TextFieldElement
        name="lastName"
        label="First Name"
        validation={{
          validate: validateLastNameLength
        }}
      />
      <TextFieldElement
        name="url"
        label="Url"
        validation={{
          pattern: {
            value: URL_REGEXP,
            message: "Url is invalid!"
          }
        }}
      />
      <Button
        type="submit"
      <Button
        type="submit"
        // 当表单所有元素都验证通过并且表单元素发生过改变后，可以点击提交按钮
        disabled={
          !formContext.formState.isValid || !formContext.formState.isDirty
        }
      >
        Submit
      </Button>
    </FormContainer>
  );
};

export default MyForm;
{% endhighlight %}

>验证触发模式 `mode`

首先，我们需要在formContext中规定`mode`属性，这个属性用来确定form何时触发验证规则。mode提供了以下5中触发方式:

{% highlight ruby %}
ValidationMode = {
    onBlur: "onBlur";
    onChange: "onChange";
    onSubmit: "onSubmit";
    onTouched: "onTouched";
    all: "all";
}
{% endhighlight %}
根据项目需求，开发者可自行选择触发方式，本例中使用的是`all`，即需要匹配所有触发方式。

>三种表单验证的方法:

1. 自定义的`required`的提示

{% highlight ruby %}
validation={{
    required: "First Name is required!"
 }}
{% endhighlight %}
2. 通过正则匹配来验证表单元素

{% highlight ruby %}
validation={{
    pattern: {
        value: URL_REGEXP,
        message: "Url is invalid!"
      }
 }}
{% endhighlight %}
3. 通过自定义的验证规则来验证表单元素：

{% highlight ruby %}
validation={{
    validate: validateLastNameLength
}}
{% endhighlight %}
通过以上三种方式，我们可以规定用户输入表单的值并提供自定义的错误提示。

>何时能够点击提交按钮

在`react-hook-form-mui`中，提供了简便的api去控制是否能够点击提交按钮。分别是：
- `formContext.formState.isValid`: 验证表单元素是否合法。
- `formContext.formState.isDirty`: 验证表单元素是否发生过改变。

通过这两种方法，我们可以很轻松地控制何时能够点击提交按钮。

#### 总结
以上是关于`React-hook-form-mui`的表单验证的的用法。希望本文会对你有所帮助。如果有什么问题，可在下方留言沟通。