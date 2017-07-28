---
layout: article
title:  "一道Freecodecamp的题"
date:   2016-10-29 20:59:27 +0800
categories: js
---

今天介绍一道Freecodecamp的题的解法～好久没有更新了，主要右眼有点飞蚊症心情低落。。。不过解出了一道以前不会的题还是很开心的，犀牛书真的很管用！

这道题来自[FreeCodeCamp](https://www.freecodecamp.cn)，一个很棒的GitHub免费编程学习项目，题目如下：   


写一个 function，它遍历一个对象数组（第一个参数）并返回一个包含相匹配的属性-值对（第二个参数）的所有对象的数组。如果返回的数组中包含 source 对象的属性-值对，那么此对象的每一个属性-值对都必须存在于 collection 的对象中。

例如，如果第一个参数是 `[{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }]`，第二个参数是 `{ last: "Capulet" }`，那么你必须从数组（第一个参数）返回其中的第三个对象，因为它包含了作为第二个参数传递的属性-值对。  

​    

下面是我的解法：

~~~javascript
function where(collection, source) {
  var arr = [];
  outer:for(var i = 0; i < collection.length; i++){
    for(var prop in source){
      if(!(prop in collection[i])) continue outer;
      if(source[prop] !== collection[i][prop]) continue outer;
    }
    arr.push(collection[i]);
  }
  return arr;
}
~~~

  
    

这里最重要的就是给第一层的for循环设置一个标签，然后用`continue labelname`的格式，重新开始一次循环。  



其次就是我注意到题目中有的对象的属性的值是`null`，所以我没有采用`if(!collection[i][prop])`的写法，也没有采取`if(collection[i][prop] == undefined)`的写法，在`==`的判断中，`null`和`undefined`是相等的，但我也没有用严格相等。 而是采用了 `string in object`的语法，它不仅可以检测某个object有没有属性`string`，还可以分辨一个属性是否显式的赋值了`undefined`。  

题目给的提示是使用`obejct.hasOwnProperty()`还有`Object.keys()`，不过我觉得不需要用到这两个方法～欢迎提供其他解法！





