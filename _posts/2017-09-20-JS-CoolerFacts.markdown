---
layout: post
title: JS 那些你不知道的知识(1)
date: 2017-9-20 14:36:24.000000000 +09:00
---

### 那些你不知道的关于 JS 的冷知识

#### 1.大小写

HTML 不区分大小写 `onclick=onClick`

#### 2.命名

变量命名必须是以字母，下划线（\_）或者美元符号（\$）命名的。

#### 3.分号的应用

{% highlight ruby %}
var a
a
=
3
console.log(a)
{% endhighlight %}
会被解析为 `var a;a=3;consle.log(a);`
js 不是在所有换行处补分号的 只有在缺少分号时无法正确解析的时候才会补分号。

{% highlight ruby %}
var y=x+f
(a+b).toString()
{% endhighlight %}
解析为：`var y=x+f(a+b).toString()`

```
例外：
1. 在 break，continue，return 后面，会强制加分号
例如 ：
return
true;
会被解析为 return;true;
2. "++"和"--" 会作为下一行代码的前缀
例如：
x
++
y
这段代码将解析为"x;++y",而不是"x++;y"
```

#### 4.Number

`1/0 Infinity（无穷大）`

`2\*Number.MAX_VALUE = Infinity`
NaN 和任何值都不相等，包括自己：`x=NaN; x==NaN=>false; x!=x=>true`

`0===-0 =>true`

`0.3-0.2=0.09999999999999998`

`0.4-0.3=0.10000000000000003 0.2-0.1=0.1`

#### 5.String 拼接

"long\ <br>
Line"=>"longline"

#### 6.String Methods

```
var s="hello, world"
s.substring(1,4)=>"ell"
s.slice(1,4)=>"ell"
s.substr(1,4)=>"ello"
s.indexOf("l",3)=>3 注释：第三个字母开始后 l 出现在字符串中的位置
```

#### 7.Undefined and Null

`null == undefined =>true`
null 和 undefined 是它自由类的唯一一个成员, undefined 表示未定义是全局变量, null 是一个特殊的对象值 null 是关键字

#### 8.包装对象

```
var s="test";
s.len="lll";
var t= s.len =>t=undefined;
```

#### 9. 对象

即使两个对象包含同样的属性及相同的值，他们也是不相等的。各个索引元素完全相等的两个数组也不相等。

#### 10. 转换和相等性

以下所有的值都会 true

```
Null==undefined
"0" ==0
0 == false
"0 " == false
```

#### 11.显示类型转换

`var x= 3 ; x+"" =>"3" =>String(x)`

`var x="3" +x =>3 =>Number(x)`

`var x=3 !!x =>true =>Boolean(x)`
