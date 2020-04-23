---
layout: post
title: React 片段（fragments）详解
date: 2019-01-02 14:36:24.000000000 +09:00
---

### 一. 动机

以下的例子简洁清晰的反映了 fragments 的作用，在生产过程中，经常会遇到这种情况：
首先，是在 Table 组件中插入 Columns 组件：

{% highlight ruby %}
class Table extends React.Component {
    render() {
        return (
        <table>
            <tr>
                <Columns />
            </tr>
        </table>
        );
    }
}
{% endhighlight %}

因为 React 组件最外层需要一个元素包裹，如果这时使用`div`标签包裹，那么最终生成的 HTML 将是无效的。

{% highlight ruby %}

class Columns extends React.Component {
    render() {
        return (
        <div>
            <td>Hello</td>
            <td>World</td>
        </div>
        );
    }
}
{% endhighlight %}

这时候`Fragment`就派上用场了

##### 1.1 使用

{% highlight ruby %}
class Columns extends React.Component {
    render() {
        return (
            <React.Fragment>
                <td>Hello</td>
                <td>World</td>
            </React.Fragment>
        );
    }
}
{% endhighlight %}

这时候，Table 组件就能被正确渲染如下：

{% highlight ruby %}
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
{% endhighlight %}

##### 1.2 简写语法

有一个新的，更短的语法可以用来声明片段(fragments)。他看起来像是空标签:

{% highlight ruby %}
class Columns extends React.Component {
    render() {
        return (
            <>
                <td>Hello</td>
                <td>World</td>
            </>
        );
    }
}
{% endhighlight %}

您可以像使用其他元素一样使用`<></>`，只是它不支持 键(keys) 或 属性(attributes)。

> 注：目前很多工具都不支持这个简写语法，所欲可能需要明确地使用`<React.Fragment>`，直到工具支持这个语法。

##### 1.3 带 key 的片段（fragments）

`<React.Fragment />`可以传递 key 值，如下：
{% highlight ruby %}
function Glossary(props) {
    return (
        <dl>
            {props.items.map(item => (
                <React.Fragment key={item.id}>    
                    <dt>{item.term}</dt>
                    <dd>{item.description}</dd>
                </React.Fragment>
            ))}
        </dl>
    );
}
//没有`key`，将会触发一个 key 警告
{% endhighlight %}

> 注：`key`是唯一可以传递给`Fragment`的属性。在将来，可能会增加额外的属性，比如事件处理。
