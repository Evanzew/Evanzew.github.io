---
layout: post
title: JS 那些你不知道的知识(2)
date: 2017-9-21 14:36:24.000000000 +09:00
---

### 那些你不知道的关于 JS 的冷知识

#### 1.显示类型转换

```
parseInt(".1") =>NaN: 整数类型不能以.开始
parseInt("$72.47") =>NaN: 数字不能以$开头
```

#### 2.对象转换为原始值

```
var now= ew Date();
typeof(now + 1)  //=>String,"+"将日期转换为字符串
typeof(now + 1)  //=>Number,"-"使用对象到数字的转换
now == now.toString()  // >=> true: 隐式的和显式的字符串转换
now > (now -1)    // => true: ">" 将日期转换为数字

```

#### 3.声明提前

JavaScript 函数里声明的所有变量（但不涉及赋值）都被提前至函数体的顶部。来看下经典的代码例子：

{% highlight ruby %}
Var scope="global";
function f(){
console.log(scope);
var scope="local";
console.log(scope);
}
{% endhighlight %}

#### 4.作为属性的变量

当使用 var 声明一个变量时，创建的这个属性是不可配置的。如果没有使用 var 给一个变量赋值的话，js 会自动创建一个全局变量。以这种方式创建的变量是可配置属性，并可以删除。
{% highlight ruby %}
var truevar =1;
fakervar =2;
delete truevar ; //=> false: 变量没有被删除
delete fakervar; // => true：变量被删除
{% endhighlight %}

#### 5.运算符优先级

标题为 A 的列表式运算符的结合性，L（从左至右）或 R（从右至左），标题为 N 的列表式操作数的个数。
![Chart](../../assets/images/Operation chart.png)

#### 6.运算顺序

var a =1; b=(a++)+a; 将如何计算结果？

```
1）	计算b
2）	计算a++（设为c值     //a=1
3）	计算a               //a=2
4）	计算c+a            	//1+2

```

#### 7.求余运算

求余运算也叫做模运算，模就是余数。求余运算符的操作数通常都是整数，但是也适用于浮点数，例如:
`6.5%2.1结果是0.2`
