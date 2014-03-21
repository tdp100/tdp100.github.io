---
layout: post
title:  "IE8 'Invalid Argument' in Raphael.js"
date:   2014-03-21 12:28:40
categories: javascript
---

##IE 8问题:
---
网页错误详细信息

用户代理: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; aff-kingsoft-ciba; .NET4.0C; .NET4.0E)`<br/>
时间戳: Fri, 21 Mar 2014 02:39:45 UTC<br/>
消息: **参数无效**。<br/>
行: 11<br/>
字符: 23822<br/>
代码: 0<br/>
`URI: http://localhost:63342/tenant/lib/tiny/tiny-lib/raphael.js`<br/>

##原因分析
---

1.问题出现的代码：<br/>
{% highlight javascript %}
R._engine.initWin = function (win) {
    var doc = win.document;
    doc.createStyleSheet().addRule(".rvml", "behavior:url(#default#VML)");      //BUG
    try {
        !doc.namespaces.rvml && doc.namespaces.add("rvml", "urn:schemas-microsoft-com:vml");
        createNode = function (tagName) {
            return doc.createElement('<rvml:' + tagName + ' class="rvml">');
        };
    } catch (e) {
        createNode = function (tagName) {
            return doc.createElement('<' + tagName + ' xmlns="urn:schemas-microsoft.com:vml" class="rvml">');
        };
    }
};
{% endhighlight %}

2.原因：         <br/>
<p>IE 限制如下:[Remarks](http://msdn.microsoft.com/en-us/library/ms531194\(v=vs.85\).aspx)</p>
You can create up to 31 styleSheet objects with the createStyleSheet method. After that, the method returns an "Invalid Argument" exception. To create additional objects, use createElement and append them to the HEAD of the document as follows:

<p>Raphael也已经有[issuse](https://github.com/DmitryBaranovskiy/raphael/issues/178)跟踪</p>

##解决方案
---

* 压缩程序中的css文件，减少stylesheet数量
* 修改Raphael源码
{% highlight javascript %}
R._engine.initWin = function (win) {
    var doc = win.document;
    if (doc.styleSheets.length >= 31) {
        window.console.log("doc.stylesheets nub >= 31");
        doc.styleSheets[0].addRule(".rvml", "behavior:url(#default#VML)");
    }
    else {
        doc.createStyleSheet().addRule(".rvml", "behavior:url(#default#VML)");
    }
    try {
        !doc.namespaces.rvml && doc.namespaces.add("rvml", "urn:schemas-microsoft-com:vml");
        createNode = function (tagName) {
            return doc.createElement('<rvml:' + tagName + ' class="rvml">');
        };
    } catch (e) {
        createNode = function (tagName) {
            return doc.createElement('<' + tagName + ' xmlns="urn:schemas-microsoft.com:vml" class="rvml">');
        };
    }
};
{% endhighlight %}