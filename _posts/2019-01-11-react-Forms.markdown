---
layout: post
title: React 表单（Forms）详解
date: 2019-01-11 14:36:24.000000000 +09:00
---

如果希望 React 的表单和 HML 表单的默认行为不一致，需要使用到一种称为“受控组件(Controlled Components)”的技术。

### 受控组件

在 HTML 中，表单元素如 input，select 等根据用户输入更新。而在 React 中，可变状态一般保存在组件的 state 属性中，并通过 setState()方法更新。
我们可以通过是 React 的 state 成为"单一数据源"来结合这两个形式。然后 React 组件也可以控制在用户输入之后的行为。这种形式，其值由 React 控制的输入表单元素，称之为“受控组件”，即 Controlled Components.

#### 一. Input

如我们想在提交时记录名称，我们可以将表单写为受控组件：

```
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value.toUpperCase()});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input bgcolor=black type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

> 简单来说，表单元素都以类似的方式工作，他们都接受一个 value 属性可以用来实现一个受控组件。

> 注：您可以将一个数组传递给 value 属性，允许你在 select 标签中选择多个选项：
> `<select multiple={true} value={['B','C']}>`

##### 二. file input 标签

在 React 中，一个 type 为 file 的 input 和一个普通的 input 类似，担忧一个重要的区别：**它是只读的(read-only)。**(不能以编程方式设置值)。相反应该使用 FileAPI 与文件进行交互。
以下实例显示了如何使用一个 ref 来访问提交处理程序中的文件：

```
class FileInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(
      this
    );
  }
  handleSubmit(event) {
    event.preventDefault();
    alert(
      `Selected file - ${
        this.fileInput.files[0].name
      }`
    );
  }

  render() {
    return (
      <form
        onSubmit={this.handleSubmit}>
        <label>
          Upload file:
          <input
            type="file"
            ref={input => {
              this.fileInput = input;
            }}
          />
        </label>
        <br />
        <button type="submit">
          Submit
        </button>
      </form>
    );
  }
}
```

##### 三. 处理多个输入元素

当需要处理多个受控的`input`元素时，可以为每个元素添加一个`name`属性，并且让函数根据`event.target.name`的值来选择要做什么。

```
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```
