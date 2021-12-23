---
layout: post
title: JS 那些你不知道的知识(3)
date: 2017-10-04 14:36:24.000000000 +09:00
tags: JS
---

#### 1. 相等和不等运算符

"=="和"==="运算符用于比较两个值是否相等。"==="也成为严格相等运算符（strict equality）（有时也称作恒等运算符（identity equality），它用来检测两个操作数是否严格相等。"=="运算符称作相等运算符（equality operation），它用来检测两个操作数是否相等，这里的相等的定义非常宽松，可以允许进行类型转换。

严格相等运算符“===”首先计算其操作数的值，然后比较这两个值，比较过程没有任何类型转换。

- 如果两个值类型不相同，则它们不相等
- 如果两个值都是 null 或者都是 undefined，则它们不相等
- 如果两个值都是布尔值 true 或者布尔值 false，则它们相等
- 如果其中一个值是 NaN，或者两个值都是 NaN,则它们不相等。NaN 和其他任何职都是不相等的，包括它本身！通过 x!==x 判断 x 是否是 NaN，只有在 x 为 NaN 的时候，这个表达式的值才为 true。
- 如果两个值为数字且数值相等，则它们相等。如果一个值为 0，另一个值为-0，则它们同样相等。
- 如果两个值为字符串，且所含的对应位上的 16 位数完全相等，则他们相等。如果它们的长度或内容不同，则它们不等。两个字符串肯能含义完全一样且所显示出的字符也一样，但具有不同编码的 16 位值。JavaScript 并不对 Unicode 进行标准化的转换，因此像这样的字符串通过“===”和“==”运算符的比较结果也不相等。
- 如果两个引用值指向同一个对象、数组或函数，则它们是相等的。如果指向不同对象，则它们是不大能的，尽管两个对象具有完全一样的属性。

相等运算符“==”和恒等运算符相似，单相等运算符的比较并不严格。如果两个操作数不是同一类型的，那么相等运算符会尝试进行一些类型转换，然后进行比较：
相等运算符“==”和恒等运算符相似，单相等运算符的比较并不严格。如果两个操作数不是同一类型的，那么相等运算符会尝试进行一些类型转换，然后进行比较：

- 如果两个操作数的类型相同，则和上文所述的严格相等的比较规则一样。如果严格相等，那么比较结果为相等。如果他们不严格相等，则比较结果为不相等
- 如果两个操作数类型不同，“==”相等操作符也可能会认为它们相等。检测相等将会村手如下规则和类型转换：
  - 如果一个值是 null，另一个是 undefined，则他们相等。
  - 如果一个值是数字，另一个是字符串，先将它们转换为数字在进行比较。如果其中一个值是 false，则将其转换为 0 在进行比较。
  - 如果一个值是对象，另一个值是数字或字符串，则先按规则将对象转换为原始值，在进行比较。
  - 其他不同类型之间的比较均不相等。

#### 2. in 运算符

in 运算符希望它的左操作数是一个字符串或可以转换为字符串，希望他的右侧操作数是一个对象。如果右侧的对象拥有一个名为做操作数值的属性名，那么表达式返回 true，例如：

{% highlight ruby %}
var point = { x : 1, y : 2};
“x” in point //=> true:对象有一个名为“x”的属性
“z” in point //=> false:对象中不存在一个名为“z”的属性
“toString” in point //=> true:对象继承了 toString()方法
var data = [7,8,9];
“0” in data //=>true:数组包含元素“0”；
1 in data //=>true:数字转换为字符串；
3 in data //=>false:没有索引为 3 的元素；
{% endhighlight %}

#### 3. instanceof 运算符

Instanceof 运算符希望左操作数是一个对象，右操作数标识对象的类。如果左侧的对象是右侧类的实例，则表达式返回 true，否则返回 false。

{% highlight ruby %}
var d = new Date();
d instanceof Date; //计算结果为 true，d 是由 Date()创建的
d instanceof Object; //计算结果为 true，所有的对象都是 Object 的实例
d instanceof Number; //计算结果为 false，d 不是一个 Numbder 对象

var a = [1, 2, 3];
a instanceof Array; //计算结果为 true，a 是一个数组
a instanceof Object; //计算结果为 true，所有数组都是对象
a instanceof RegExp; //计算结果为 true，数组不是正则表达式
{% endhighlight %}

#### 4. 逻辑与(&&)

- 例 1：
  {% highlight ruby %}
  var o = { x : 1}
  var p = null
  o && o.x //=>1 : o 是真值，因此返回值为 o.x
  p && p.x //=>null: p 是假值，因此将其返回，而并不去计算 p.x
  {% endhighlight %}

- 例 2：以下两行代码完全等价
  {% highlight ruby %}
  i. if(a==b) stop();
  ii. (a==b) && stop();
  {% endhighlight %}

#### 5. 逻辑非(!)

- 对于 p 和 q 取任何值，这两个等式都永远成立。
  {% highlight ruby %}
  i. !(p && q) === !p || !q
  ii. !(p || q) === !p && !q
  {% endhighlight %}

#### 6. 全局 eval()

直接调用 eval()，是在上下文作用域内执行。其他间接调用则是使用全局对象作为其上下文作用域。如下：
{% highlight ruby %}
var geval = eval;
var x = 'global',
y = 'global';
function f() {
var x = 'local';
eval("x += ' changed';");
return x;
}
function g() {
var y = 'local';
geval("y += ' changed';");
return y;
}
console.log(f(), x); //=>local changed global
console.log(g(), y); //=>local global changed
{% endhighlight %}

#### 7. Delete 运算符

- 例 1：
  {% highlight ruby %}
  var a = [1, 2, 3];
  delete a[2];
  2 in a; //=>false: 元素 2 在数组中已经不存在了
  a.length; //=>3:注意，数组长度并没有改变，尽管上一行代码删除了这个元素，但删除操作留下了一个洞，世纪上并没有修改数组的长度，因此 a 数组的长度仍然是 3。
  a; //=> [1, 2, empty]

  {% endhighlight %}

- 例 2： delete 属性并不能删除通过 var 生命的变量。在严格模式下，会抛出一个语法错误（SyntaxError）异常。在非严格模式下，这些 delete 操作都不会报错，只是简单地返回 false，以表明操作数不能执行删除操作。
  {% highlight ruby %}
  var o = { x: 1, y: 2 };//=>定义一个变量，初始化为对象
  delete o.x; //=>删除一个对象属性，返回 true
  typeof o.x; //=>属性不存在，返回"undefined"
  delete o.x; //=>删除不存在的属性，返回 true
  delete o; //=>不能删除通过 var 声明的变量，返回 false；严格模式下，将抛出异常
  delete 1; //=>参数不是一个左值，返回 true
  this.x = 1; //=>给全局对象定义一个属性，这里没有使用 car
  delete x; //=>试图删除 x，在费严格模式下返回 true；在严格模式下会抛出异常，这时使用 delete this.x 来代替
  x; //=>运行是错误，没有定义 x
  {% endhighlight %}
