---
layout: post
title:  "how to config external jar in tomcat"
date:   2014-03-06 14:30:40
categories: jekyll update
---

当我们的jar包没有放置到tomcat中${CATALINA_HOME}/bin或者application的WEB-INF/lib目录下时，那么需要配置 ${CATALINA_HOME}/conf/catalina.properties，使tomcat在启动时加载扩展的jar包

{% highlight ruby %}
common.loader=${catalina.base}/lib,${catalina.base}/lib/*.jar,${catalina.home}/lib,${catalina.home}/lib/*.jar
{% endhighlight %}