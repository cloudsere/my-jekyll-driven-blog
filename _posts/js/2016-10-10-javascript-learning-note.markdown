---
layout: article
title:  "javascript学习笔记"
date:   2016-10-10 10:32:27 +0800
categories: js
---
> 写作学习笔记读作长文档编辑的一篇博文。人比较懒就把博客新开的一些话写在这里吧。去年开始学着用jekyll和github pages搭建博客，照着youtube一个视频搭建了一个以后彻底荒废。。。直到阿里云催我续费才想起来。现在想着把博客重新用起来，所以用了两天的时间换了样式，添加了分页的功能，明天再加一个评论的功能吧。不说太多计划，免得flag立的飞起，还有太多要学习的东西呢。如果你看到了这个小小的博客，我想说，欢迎来到我的小世界。

####attr()函数
{% highlight ruby %}
jQueryObject.attr( attributeName [, value ] ) 
{% endhighlight %}
设置或返回指定属性attributeName的值。如果指定了value参数，则表示设置属性attributeName的值为value；如果没有指定value参数，则表示返回属性attributeName的值。

***

####严格等于`===`
下面的规则用来判断两个值是否===相等：  

1. 如果类型不同，就[不相等]  
2. 如果两个都是数值，并且是同一个值，那么[相等]；(！例外)的是，如果其中至少一个是NaN，那么[不相等]。（判断一个值是否是NaN，只能用isNaN()来判断）  
3. 如果两个都是字符串，每个位置的字符都一样，那么[相等]；否则[不相等]。  
4. 如果两个值都是true，或者都是false，那么[相等]。  
5. 如果两个值都引用同一个对象或函数，那么[相等]；否则[不相等]。 
 6. 如果两个值都是null，或者都是undefined，那么[相等]。 

***

####等于`==`
根据以下规则：  

1. 如果两个值类型相同，进行 === 比较。  
2. 如果两个值类型不同，他们可能相等。根据下面规则进行类型转换再比较：     

* a 如果一个是null、一个是undefined，那么[相等]。     
* b 如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。     
* c 如果任一值是 true，把它转换成 1 再比较；如果任一值是 false，把它转换成 0 再比较。     
* d 如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。js核心内置类，会尝试valueOf先于toString；例外的是Date，Date利用的是toString转换。非js核心的对象，令说（比较麻烦，我也不大懂）  
* e、任何其他组合，都[不相等]。

***

####`arrayObject.shift()`
`shift()` 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。
如果数组是空的，那么 shift() 方法将不进行任何操作，返回 undefined 值。请注意，该方法不创建新数组，而是直接修改原有的 arrayObject。

***

####函数声明、函数表达式、匿名函数
`函数声明:`function fnName () {…};使用function关键字声明一个函数，再指定一个函数名，叫函数声明。

`函数表达式:` var fnName = function () {…};使用function关键字声明一个函数，但未给函数命名，最后将匿名函数赋予一个变量，叫函数表达式，这是最常见的函数表达式语法形式。

`匿名函数:`function () {}; 使用function关键字声明一个函数，但未给函数命名，所以叫匿名函数，匿名函数属于函数表达式，匿名函数有很多作用，赋予一个变量则创建函数，赋予一个事件则成为事件处理程序或创建闭包等等。

函数声明和函数表达式不同之处在于:
Javascript引擎在解析javascript代码时会’函数声明提升’（Function declaration Hoisting）当前执行环境（作用域）上的函数声明，而函数表达式必须等到Javascirtp引擎执行到它所在行时，才会从上而下一行一行地解析函数表达式。

函数表达式后面可以加括号立即调用该函数，函数声明不可以，只能以fnName()形式调用 。

在function前面加！、+、 -甚至是逗号等到都可以起到函数定义后立即执行的效果，而（）、！、+、-、=等运算符，都将函数声明转换成函数表达式，消除了javascript引擎识别函数表达式和函数声明的歧义，告诉javascript引擎这是一个函数表达式，不是函数声明，可以在后面加括号，并立即执行函数的代码。
加括号是最安全的做法，因为！、+、-等运算符还会和函数的返回值进行运算，有时造成不必要的麻烦.
javascript中没用私有作用域的概念，如果在多人开发的项目上，你在全局或局部作用域中声明了一些变量，可能会被其他人不小心用同名的变量给覆盖掉，根据javascript函数作用域链的特性，可以使用这种技术可以模仿一个私有作用域，用匿名函数作为一个“容器”，“容器”内部可以访问外部的变量，而外部环境不能访问“容器”内部的变量，所以( function(){…} )()内部定义的变量不会和外部的变量发生冲突，俗称“匿名包裹器”或“命名空间”。

***

####for/in循环
{% highlight ruby %}
var person = {fname:”John”, lname:”Doe”, age:25} 
for (x in person){     
	txt = txt + person[x] 
}
{% endhighlight %}

***

####至少执行一次的循环 do/while循环
{% highlight ruby %}
do {   x = x + i;   i++; } while(i<5);
{% endhighlight %}
***

####break/continue语句
break语句用于跳出循环，跳出上一层for循环
continue语句用于跳过某一次迭代，然后继续循环中的下一个迭代
typeof,null,undefined
typeof “John” // 返回string
typeof undefined //undefined
typeof null //object
null === undefined // false
null == undefined //true

***

####javascript数据类型
5种不同的数据类型
string
number //type of NaN 返回number
object //type of [1, 2, 3, 4]返回object
function
boolean
3种对象类型
object
array
date
2个不包含任何值的数据类型
null
null用于对象，对象只有被定义后才可能为null，否则为undefined
undefined
undefined用于变量、属性和方法
检测一个对象是否存在 if(typeof myObj !== “undefined” && myObj !== null)
可以用constructor属性返回javascript变量的构造函数
var myDate = new Date(); myDate.constructor// 返回函数 Date()    { [native code] }
将数字/布尔/日期转换为字符串
String(x)
x.toString() //当你尝试输出一个对象或一个变量时 JavaScript 会自动调用变量的 toString() 方法
将字符串/布尔／日期转换为数字
Number(“3.14”) //3.14
Number(false) //0
d = new Date(); Number(d); //返回1404568027739 //和d.getTime()效果一样
运算符＋ var y = “5” var x = +y //x就是5

***

####javascript正则表达式
search()
var str = “Visit w3cschool”; var n = str.search(/w3cschool/i) //i表示搜索不区分大小写
replace()
var str = “Visit Microsoft”; var res = str.replace(/microsoft/i, “w3cschool”) var res1 = str.replace(“Microsoft”, “w3cschool”)
test()
test()用于检测一个字符串是否匹配某个模式，正则表达式.test(字符串)
exec()
正则表达式.exec(字符串)，用于检索字符串中正则表达式的匹配

***

####javascript错误
当JavaScript 引擎执行 JavaScript 代码时，会发生错误，此时引擎会停止，并生成一个错误消息。也就是“JavaScript 将抛出一个错误”
try和catch
try{ }catch(err){    alert(err.message); }
throw语句，允许创建自定义错误，或者说，创建或抛出异常
try{    if(x > 10) throw “太大”；    if(x < 5) throw “太小”； }catch(err){    alert(err) }

***

####javascript调试
console.log()
设置断点 debugger

***

####javascript变量提升
hoisting变量提升
javascript中，函数及变量的声明都将被提升到函数的最顶部。所以变量可以在使用后声明。
x = 5; elem.innerHTML = x; var x;
但是只有变量的声明会被提升，变量的初始化不会。var x会被提升，var x = 5不会
javascript的严格模式（strict mode）不允许使用未声明的变量

***

####javascript严格模式
在脚本或函数头部添加“use strict”表达式来声明
<script> “use strict”; x = 5;//报错，x未定义 </script>
function myFunction(){ “use strict”; y = 3.14;//报错 }
"use strict" 指令在 JavaScript 1.8.5 (ECMAScript5) 中新增。
严格模式不允许删除变量或对象 delete x；
不允许删除函数
不允许变量重名
不允许使用八进制 var x = 010;
不允许使用转义字符 var x = \010
变量名不能是eval arguments public

***

####javascript浮点型数据
javascript中的所有数据都以64位浮点型数据float来存储
<script> var x = 0.1; var y = 0.2; var z = x + y; document.getElementById("demo").innerHTML = z; //0.30000000000000004 </script>
可以用整数的乘除法来解决 var z = (x * 10 + y * 10) / 10

***

####javascript字符串换行
var x = "Hello World!”; //报错
var x = “Hello \ World!”; //字符串换行需要使用反斜杠
javascript中分号是可选的。如果是一个不完整的语句，比如var，javascript将尝试读取第二行的语句：power = 10；但由于return自身是一个完整的语句，所以javascript将自动关闭语句。

####javascript程序块作用域
在每个代码块中 JavaScript 不会创建一个新的作用域，一般各个代码块的作用域都是全局的。

***

####javascript标准
所有的现代浏览器完全支持 ECMAScript 3（ES3，JavaScript 的第三版，从 1999 年开始）。
ECMAScript 4（ES4）未通过。
ECMAScript 5（ES5，2009 年发布），是 JavaScript 最新的官方版本。

***

####javascript:void(0)
void func() javascript: void func() void(func()) javascript:void(func())
<a href="javascript:void(0)">单击此处什么也不会发生</a>
<a href=“javascript:void(alert(“warning!!!”))”>点我！</a>
href = “#”和href=“javascript:void(0)”是不一样的 #包含一个位置信息，默认的#top是网页的上端，当页面很长的时候可以用#来确认页面的具体位置，使用#+id

***

####javascript代码规范
camelCase
全局变量和常量为大写 (UPPERCASE )
通常运算符 ( = + - * / ) 前后需要添加空格
通常使用 4 个空格符号来缩进代码块
复杂语句的通用规则:
将左花括号放在第一行的结尾。
左花括号前添加一空格。
将右花括号独立放在一行。
分号是用来分隔可执行JavaScript语句。 由于函数声明不是一个可执行语句，所以不以分号结束。

***

####自调用函数
如果表达式后面紧跟 () ，则会自动调用。
(function (){     var x = “Hello”; })();

***

####js函数是对象
function myFunction(){     return arguments.length; }//argument 对象包含了函数调用的参数数组,返回函数调用时接收的参数个数
function myFunction(a,b){     return a * b; } var txt = myFunction.toString();
indexOf 
lastIndexOf

***
