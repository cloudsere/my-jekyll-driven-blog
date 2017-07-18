---
layout: article
title:  "Angular Shopping List"
date:   2017-7-17 20:59:27 +0800
categories: js
---

今天看完了w3上的angular教程，然后自己把最后一个小demo做了一下
<a href = "https://codepen.io/cloudsere/full/mwYBed"> 看这里 </a>  

​下面是控制器部分：
~~~javascript
var app = angular.module("shopList",[]);//[]一定要有
app.controller('shopListCtrl',function($scope){
	$scope.shopItem = ['apple', 'banana','bear'];
	$scope.addItem = function(){
		if(!$scope.newItem){ return;}
		if($scope.shopItem.indexOf($scope.newItem) == -1){
			$scope.shopItem.push($scope.newItem);
		}else{
			$scope.errorTxt = "这个已经有了哦";
		}
	};
	$scope.deleteItem = function(x){
		$scope.shopItem.splice(x,1);
	}
})
~~~

比较重要的就是掌握angular的基本语法，感觉和vue是相当的类似，一个添加删除的小demo做起来很快

其次就是浮动后面一定要清除浮动 !important


