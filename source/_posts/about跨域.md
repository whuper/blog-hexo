
---
title: 跨域问题总结
date: 2017-09-22

---


## 造成跨域的两种情况

造成跨域的两种策略浏览器的同源策略会导致跨域，这里同源策略又分为以下两种

1. DOM同源策略：禁止对不同源页面DOM进行操作。这里主要场景是iframe跨域的情况，不同域名的iframe是限制互相访问的。
1. XmlHttpRequest同源策略：禁止使用XHR对象向不同源的服务器地址发起HTTP请求。

> 只要协议、域名、端口有任何一个不同，都被当作是不同的域，之间的请求就是跨域操作。

### 为什么要有跨域限制

了解完跨域之后，想必大家都会有这么一个思考，为什么要有跨域的限制，浏览器这么做是出于何种原因呢。其实仔细想一想就会明白，跨域限制主要是为了安全考虑。

> AJAX同源策略主要用来防止CSRF攻击。如果没有AJAX同源策略，相当危险，我们发起的每一次HTTP请求都会带上请求地址对应的cookie，那么可以做如下攻击：

1. 用户登录了自己的银行页面 http://mybank.com，http://mybank.com向用户的cookie中添加用户标识。
1. 用户浏览了恶意页面 http://evil.com。执行了页面中的恶意AJAX请求代码。
1. http://evil.com向http://mybank.com发起AJAX HTTP请求，请求会默认把http://mybank.com对应cookie也同时发送过去。
1. 银行页面从发送的cookie中提取用户标识，验证用户无误，response中返回请求数据。此时数据就泄露了。
1. 而且由于Ajax在后台执行，用户无法感知这一过程。

#### DOM同源策略也一样，如果iframe之间可以跨域访问，可以这样攻击：

1. 做一个假网站，里面用iframe嵌套一个银行网站 http://mybank.com。
1. 把iframe宽高啥的调整到页面全部，这样用户进来除了域名，别的部分和银行的网站没有任何差别。
1. 这时如果用户输入账号密码，我们的主网站可以跨域访问到http://mybank.com的dom节点，就可以拿到用户的输入了，那么就完成了一次攻击。

所以说有了跨域跨域限制之后，我们才能更安全的上网了。

作者：黄家兴
链接：https://www.zhihu.com/question/26376773/answer/244453931
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 跨域的解决方式
#### 跨域资源共享
CORS是一个W3C标准，全称是”跨域资源共享”（Cross-origin resource sharing）。 

大体流程。

1. 对于客户端，我们还是正常使用xhr对象发送ajax请求。唯一需要注意的是，我们需要设置我们的xhr属性withCredentials为true，不然的话，cookie是带不过去的哦，设置： xhr.withCredentials = true;
1. 对于服务器端，需要在 response header中设置如下两个字段:Access-Control-Allow-Origin: http://www.yourhost.comAccess-Control-Allow-Credentials:true这样，我们就可以跨域请求接口了。

####  jsonp实现跨域

基本原理就是通过动态创建script标签,然后利用src属性进行跨域。

#### 服务器代理
浏览器有跨域限制，但是服务器不存在跨域问题，所以可以由服务器请求所要域的资源再返回给客户端。

>服务器代理是万能的。

####使用postMessage实现页面之间通信

信息传递除了客户端与服务器之前的传递，还存在以下几个问题：

1. 页面和新开的窗口的数据交互。
1. 多窗口之间的数据交互。
1. 页面与所嵌套的iframe之间的信息传递。

window.postMessage是一个HTML5的api，允许两个窗口之间进行跨域发送消息。这个应该就是以后解决dom跨域通用方法了，具体可以参照MDN。

