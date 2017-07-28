---
layout: article
title:  "使用beautifulsoup抓取大众点评广州美食"
date:   2016-10-1 10:32:27 +0800
categories: js
---
#### 伪装浏览器
大众点评是反爬虫的，所以需要伪装成浏览器。
首先引入需要用到的模块
{% highlight ruby %}
import urllib.request
from bs4 import BeautifulSoup
{% endhighlight %}

接着就是伪装
{% highlight ruby %}
headers = {
    'User-Agent':'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6'
}
url = "http://www.dianping.com/search/category/4/10/p" 
page = urllib.request.Request(url, headers = headers)
{% endhighlight %}

***

#### 使用bs4获取店铺链接
{% highlight ruby %}
soupDp = BeautifulSoup(response,'lxml')
shop = soupDp.find_all(attrs={'data-hippo-type':'shop'})
{% endhighlight %}

***

#### 遍历
基本功能做完以后就需要遍历，游客登录可以查看50页的店铺
{% highlight ruby %}
for m in range(0, 50):
	url = "http://www.dianping.com/search/category/4/10/p" + str(m)
{% endhighlight %}

***

#### 结果
抓取了750条店铺链接～有了这些链接，获取店铺的名称、地址等等就很方便啦
![有帮助的截图]({{ site.url }}/img/dianping.png)
