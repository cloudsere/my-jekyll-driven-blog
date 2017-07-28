---
layout: article
title:  "日文分词和TF-IDF处理"
date:   2017-1-23 12:12:07 +0800
categories: js
---

上一篇文章AKB唱了啥是分析的结果，这篇文章主要用于回顾总结爬虫、日文分词和TF-IDF处理的知识点。

<br>

#### 首先是爬虫

<br>

爬虫使用的是非常强大的**urllib.request**和**beautifulsoup**库

<br>

**准备**

<br>

这个网站的设计非常好，不仅禁止右键，也有基础的反爬虫处理，所以我需要伪装自己的请求成为浏览器请求。可以在检查元素的`Network`中查看自己的`User-Agent`

<img src="{{site.baseurl}}/img/akbkashi/notes/4.png" style="width: 90%;display:block;"/>

然后就可以在使用`urllib.request.Request`时指定`headers`参数

{% highlight ruby %}
headers = {'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36'}
{% endhighlight %}

除了伪装成浏览器外，我还需要番羽墙，这里需要使用的是**socks**和**sockshandler**，同时自己也需要有一个能番羽墙的工具。

{% highlight ruby %}
opener = urllib.request.build_opener(SocksiPyHandler(socks.SOCKS5,"127.0.0.1",1080))
urllib.request.install_opener(opener)
{% endhighlight %}

这样我的请求就不会被防火墙拦截了。

<br>

**需求**

1.获得歌词列表

<br>

<img src="{{site.baseurl}}/img/akbkashi/notes/1.png" style="width: 90%;display:block;"/>
使用网站的搜索功能得到每个团体的歌词列表，因为想要研究五个团体，所以需要做五次搜索。这里主要是为了获得每个团体的歌词列表。因为只有五个团体，所以可以先手动搜索，用一个`list` 存储这五个url，再进行遍历。

当然，也可以利用网址直接在代码中进行搜索。当我输入“AKB48”后，网址会跳转到`"http://www.uta-net.com/search/?Aselect=1&Bselect=3&Keyword=akb48&sort=&pnum=1"`

注意在数据库中检索时使用的`Keyword`索引，可以通过改变搜索关键词来实现检索。最后的`pnum`是页码，可以利用这个实现翻页。

<br>

2.通过歌词列表获得歌词页

<br>

<img src="{{site.baseurl}}/img/akbkashi/notes/2.png" style="width: 90%;display:block;"/>
从截图可以看到，网页呈现歌词列表的方式非常简洁和友好，是以表格的形式呈现的。但并不是一张大表，而是划分成了几个表，所以需要使用`find_all("tbody")`而不是`find_("tbody")`。

除了这个细节要注意外，还要注意偶尔会有表格没有使用`tbody`标签而是用了`table`标签，所以干脆最开始直接使用`find_all('tr')`，省去先获得表，再获得行的麻烦了。

这个网站的结构非常清晰，所以我很方便的获得了每首歌的歌词页面链接，链接信息就存储在`曲名`单元格的`a`标签的`href`中，可以使用`find("a")['href']`获得。

<br>

3.从歌词页获取歌词文本

<br>

<img src="{{site.baseurl}}/img/akbkashi/notes/3.png" style="width: 90%;display:block;"/>
可以发现，这里的歌词全部是以图片的形式呈现在网页上，我要做的就是把图片的歌词信息变成可编辑的文本保存到本地。同时，在歌词页上有我需要的发售时间、歌手信息，这些也需要一并获取。

把img的src复制到浏览器后，我发现这个img其实只是一个透明像素点，是用来遮挡下面真正的歌词图片的。（从中我们也可以学到如何保护自己网站上的文字。。。。）

获取真正的图片的src后，打开来看到歌词使用svg的`text`标签呈现在网页中的

<img src="{{site.baseurl}}/img/akbkashi/notes/5.png" style="width: 90%;display:block;"/>
鼠标直接右键复制有点麻烦，但是爬虫就很简单了，使用`find('g').text`就可以轻松得到文字了。


<br>

#### 接着是分词

通过爬虫总计获得了1000+首歌的歌词，接下来要做的第一步处理就是分词。

分词我使用的是日语分词器[`MeCab`](http://taku910.github.io/mecab/)

通过`pip install mecab-python3`在python3中安装MeCab，我安装时不用国内的镜像会报响应超时的错误，所以要加上参数`-i https://pypi.tuna.tsinghua.edu.cn/simple`

接下来就可以通过`import MeCab`来使用了

在这里注意建立分词器的时，python3不能直接写成`MeCab.Tagger('-Ochasen')`

需要这样写：

{% highlight ruby %}
m = MeCab.Tagger ('-d /usr/local/lib/mecab/dic/ipadic')  
m.parse('')
{% endhighlight %}

否则在使用`parseToNode`时会报错

接下来就可以开始分词了，在分词的同时也统计一下个数

{% highlight ruby %}
def count_word(df):
    e = df['Lyric'][972:1096]
    dic_n = {}
    dic_v = {}
    dic_a = {}
    m = MeCab.Tagger ('-d /usr/local/lib/mecab/dic/ipadic')  
    m.parse('')
    for s in e:
        if isinstance(s,str) == False:
            continue
        node = m.parseToNode(s)
        while node:
            word=node.feature.split(',')[0]
            key = node.surface
            if word=='名詞' and key !="(" and key !=")":
                dic = dic_n
                print ("<", key, "> (n)")
            elif word=='動詞':
                dic = dic_v
                print ("<", key, "> (v)")
            elif word=='形容詞':
                dic = dic_a
                print ("<", key, "> (a)")
            else:
                node = node.next
                continue
            if key in dic:
                dic[key] += 1
            else:
                dic[key] = 1
            node = node.next
{% endhighlight %}

这样就可以完成分词和词语个数的统计了

<br>


#### 最后是TF-IDF

这里使用的是**sklearn**

在这里想要唠叨一下，在人看来，不同的语言之间真的是天差地别的，但当完成分词后，这些语言在计算机的眼中，应该就都只是可以简单进行统计计算的字符串吧。（让我想到电影**降临**里的七肢桶）

{% highlight ruby %}
vectorizer = CountVectorizer(min_df = 1)
transformer = TfidfTransformer()
tfidf = transformer.fit_transform(vectorizer.fit_transform(output))
word = vectorizer.get_feature_names()
weight = tfidf.toarray()
{% endhighlight %}

因为只是很基础的需求，所以直接使用[官网文档](http://scikit-learn.org/stable/modules/feature_extraction.html)里的例子就可以了，最后输出

{% highlight ruby %}
for i in range(len(weight)):
		for j in range(len(word)):
			if weight[i][j] >= 0.03:
				temp = str(word[j]) + "   " + str(weight[i][j])
				outputf[i].append(temp)
{% endhighlight %}

