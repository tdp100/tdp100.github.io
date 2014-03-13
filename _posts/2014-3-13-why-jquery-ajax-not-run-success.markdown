---
layout: post
title:  "why jquery ajax cannot run success?"
date:   2014-03-13 16:30:40
categories: javascript
---

###为什么jquery ajax不执行success回调函数？
当使用jquery ajax发送一个PUT请求时, 服务端响应码为200, 但ajax始终执行fail回调. ajax 请求代码如下：
{% highlight javascript %}
    var deffer = $.ajax({
          url:"xxx",
          type:"PUT",
          data:{}
    });
   deffer.success(function() {
       console.log("success")
   });

  deffer.fail(function() {
      console.log("fail")
  });
{% endhighlight %}

###原因
原因是http头信息设置为content-type:"application/json; charset=utf-8",而http响应体是空,并不是有效的JSON格式，ajax判断后认为是错误的而执行了fail回调。

###修改方法：
将success回调函数修改为complete回调