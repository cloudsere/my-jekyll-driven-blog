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

<hr/>

再接下来就是为了重新更新博客，在公司的电脑上再配置一遍jekyll的环境
这个过程可以想见总是会遇到问题的，把“这样做就没问题”的直通终点步骤记下来

1. 安装brew
``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```

2. 更新ruby到新版本<br/>
```brew install ruby ```

3. 安装jekyll<br/>
``` gem install jekyll ```
``` gem install jekyll-paginate ```

4. 克隆项目<br/>
``` git clone https://github.com/cloudsere/jekyll-lesson.git ```

5. cd jekyll-lesson

6. 开始开发<br/>
``` jekyll serve ```

7. git add

8. git commit -m "  "

9. git push

这样就更新了博客了！话说我忘记博客的域名在哪里买的了。。。万一要续费就完蛋惹（哭）


