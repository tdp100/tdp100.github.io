---
layout: post
title:  "git-cannot-pull&push"
date:   2014-03-18 12:03:40
categories: git
---

为什么git不能直接提交代码了
==========================
问题：
---
在本地仓库中使用<code>git pull origin</code>

出现如下错误信息
{% highlight javascript %}
fatal: unable to access 'http://host:port/xxx.git/': The requested URL returned error: 502
{% endhighlight %}

原因：
---
由于git服务器是内网，而配置本地代理后就pull&push不了了

1）环境变量中配置了http_proxy变量
{% highlight javascript %}
echo %http_proxy%
http://137.3.6.19:8080
{% endhighlight %}

2) 在本地仓库中设置了全局的http.proxy
{% highlight javascript %}
git config --global -l
http.proxy=http://user:pwd@proxy:port
{% endhighlight %}

解决办法：
------
1)删除环境变量http_proxy

2)将http.proxy设置成局部变量，并在内网仓库中不配置http.proxy属性
