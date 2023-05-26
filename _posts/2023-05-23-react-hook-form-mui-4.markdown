---
layout: post
title: React-hook-form-mui（四）：与后端数据联调
date: 2023-05-23 09:59:24.000000000 +09:00
tags:  React MUI react-hook-form-mui
---

**关键词** `React` `MUI`  `react-hook-form-mui`

### 前言
在上一篇文章中，我们介绍了`react-hook-form-mui`的表单验证功能。本文将从实际项目出发，介绍在真实项目中，如何与后端数据联调并初始化页面数据。
`react-hook-form-mui`提供了 `reset`方法，当页面初始化的时候，调用后端数据，并通过reset方法，初始化表单的数据。

### Demo
以下代码简单的讲解了最基础的一种与后端联调的方式：

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

  const [loading, setLoading] = useState<boolean>(false);

  // 使用 watch 属性获取表单数据
  const { watch } = formContext;
  const formData = watch();

  const onSubmit = (data) => {
    console.log(data);
  };

  React.useEffect(() => {
    setLoading(true);
    //获取后端数据
    getUserSetting<UserSettings>.then(
      (res: UserSettings) => {
        // 重置表单数据
        formContext.reset(res);
        setLoading(false);
      }
    );
  }, []);

  return (
    <FormContainer
      formContext={formContext}
      onSuccess={(data) => {
        onSubmit(data);
      }}
    >
      <TextFieldElement name="firstName" label="First Name" />
      <TextFieldElement name="lastName" label="Last Name" />
      <Button type="submit">Submit</Button>
    </FormContainer>
  );
};

export default MyForm;
{% endhighlight %}

在以上demo中，我们获取完后端数据后，通过`reset`方法重置表单数据，从而达到初始化页面的目的。

###总结
以上是关于`React-hook-form-mui`的页面初始化的讲解。希望本文会对你有所帮助。如果有什么问题，可在下方留言沟通。