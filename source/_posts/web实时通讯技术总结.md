
---
title: Web端即时通讯技术
date: 2017-11-11

---


## Web端即时通讯技术

> 传统Web的通信原理
> 
> 浏览器本身作为一个瘦客户端，不具备直接通过系统调用来达到和处于异地的另外一个客户端浏览器通信的功能。这和我们桌面应用的工作方式是不同的，通常桌面应用通过socket可以和远程主机上另外一端的一个进程建立TCP连接，从而达到全双工的即时通信。
> 
> 浏览器从诞生开始一直走的是客户端请求服务器，服务器返回结果的模式，即使发展至今仍然没有任何改变。所以可以肯定的是，要想实现两个客户端的通信，必然要通过服务器进行信息的转发。例如A要和B通信，则应该是A先把信息发送给IM应用服务器，服务器根据A信息中携带的接收者将它再转发给B，同样B到A也是这种模式，如下所示：

常见的实现即时通讯实现方法

> 轮询：客户端定时向服务器发送Ajax请求，服务器接到请求后马上返回响应信息并关闭连接。 
> 优点：后端程序编写比较容易。 
> 缺点：请求中有大半是无用，浪费带宽和服务器资源。 
> 实例：适于小型应用。
> 
> 
> 长轮询：客户端向服务器发送Ajax请求，服务器接到请求后hold住连接，直到有新消息才返回响应信息并关闭连接，客户端处理完响应信息后再向服务器发送新的请求。 
> 优点：在无消息的情况下不会频繁的请求，耗费资源小。 
> 缺点：服务器hold连接会消耗资源，返回数据顺序无保证，难于管理维护。 
> 实例：WebQQ、Hi网页版、Facebook IM。
> 
> 长连接：在页面里嵌入一个隐蔵iframe，将这个隐蔵iframe的src属性设为对一个长连接的请求或是采用xhr请求，服务器端就能源源不断地往客户端输入数据。 
> 优点：消息即时到达，不发无用请求；管理起来也相对方便。 
> 缺点：服务器维护一个长连接会增加开销。 
> 实例：Gmail聊天

基于web实现IM软件依然要走浏览器请求服务器的模式，这种方式下，针对IM软件的开发需要解决如下三个问题：

#### 1 双全工通信：
即达到浏览器拉取（pull）服务器数据，服务器推送（push）数据到浏览器；
#### 2 低延迟：
即浏览器A发送给B的信息经过服务器要快速转发给B，同理B的信息也要快速交给A，实际上就是要求任何浏览器能够快速请求服务器的数据，服务器能够快速推送数据到浏览器；
#### 3 支持跨域：
通常客户端浏览器和服务器都是处于网络的不同位置，浏览器本身不允许通过脚本直接访问不同域名下的服务器，即使IP地址相同域名不同也不行，域名相同端口不同也不行，这方面主要是为了安全考虑。


### 从技术实现上，大致有4种：

1. Ajax短轮询
1. Comet 别名叫(彗星)
1. Websocket
1. SSE（Server-sent Events）

##  1. Ajax短轮询

传统的web应用要想与服务器交互，必须提交一个表单（form），服务器接收并处理传来的表单，然后返回全新的页面，因为前后两个页面的数据大部分都是相同的，这个过程传输了很多冗余的数据、浪费了带宽。于是Ajax技术便应运而生。

> Ajax是Asynchronous JavaScript and XML的简称，由Jesse James Garrett 首先提出。这种技术开创性地允许浏览器脚本（JS）发送http请求。

> XMLHttpRequest 是 AJAX 的基础。
> XMLHttpRequest 对象
> 所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。

创建 XMLHttpRequest 对象：

	var xml = new XMLHttpRequest();
	
老版本的ie就不再列举了，毕竟是快要被淘汰的东西，不具有可持续性。。。

这种浏览器端的小技术毕竟还是基于http协议，http协议要求的请求/响应的模式也是无法改变的，除非http协议本身有所改变。

**具体实现：用定时器，每间隔一定的时间，就发送请求，获取数据。。。简单粗暴，造成很多资源浪费**


---
## Comet(基于 HTTP长连接的“服务器推”技术)

是一种hack技术

> Ajax通信方式是http协议的经典使用方式，要想取得数据，必须首先发送请求。在Low Latency要求比较高的web应用中，只能增加服务器请求的频率。
> 
> Comet则不同，客户端与服务器端保持一个长连接，只有客户端需要的数据更新时，服务器才主动将数据推送给客户端。
Comet的实现主要有两种方式：

1. 基于Ajax的长轮询（long-polling）
1. 基于 Iframe 及 htmlfile 的流（http streaming）



#### 1 基于Ajax的长轮询（long-polling）方式

浏览器发出XMLHttpRequest 请求，服务器端接收到请求后，会阻塞请求直到有数据或者超时才返回，浏览器JS在处理请求返回信息（超时或有效数据）后再次发出请求，重新建立连接。在此期间服务器端可能已经有新的数据到达，服务器会选择把数据保存，直到重新建立连接，浏览器会把所有数据一次性取回。

在XHR对象的readySate为4的时候，表示服务器已经返回数据，本次连接已断开，再次请求服务器建立连接。

#### 2 基于 Iframe 及 htmlfile 的流（http streaming）方式

iframe 是很早就存在的一种 HTML 标记， 通过在 HTML 页面里嵌入一个隐蔵帧，然后将这个隐蔵帧的 **SRC 属性设为对一个长连接的请求**，服务器端就能源源不断地往客户端输入数据。

在第一种方式中，浏览器在收到数据后会直接调用JS回调函数，但是这种方式该如何响应数据呢？可以通过在返回数据中嵌入JS脚本的方式，如`<script type="text/javascript">js_func(“data from server ”)</script>`，服务器端将返回的数据作为回调函数的参数，浏览器在收到数据后就会执行这段JS脚本。

但是这种方式有一个明显的不足之处：IE、Morzilla Firefox 下端的进度栏都会显示加载没有完成，而且 IE 上方的图标会不停的转动，表示加载正在进行，并且底部也显示正在加载，这对于一个产品来讲用户体验是不好的

#### 2.1 基于 Iframe的优化

谷歌想出了一中hack方式。就是在IE中，动态生成一个**htmlfile对象**，这个对象ActiveX形式的com组件，它实际上就是一个在内存中实现的HTML文档，通过将生成的iframe添加到这个内存中的HTMLfile中，并利用iframe的数据流通信方式达到上面的效果。同时由于HTMLfile对象并不是直接添加到页面上的，所以并没有造成浏览器显示正在加载的现象。

这种方法应用到了 gmail+gtalk 产品中

[http://www.52im.net/thread-334-1-1.html](http://www.52im.net/thread-334-1-1.html)


## WebSocket


在上面的这些解决方案中，都是利用浏览器单向请求服务器或者服务器单向推送数据到浏览器这些技术组合在一起而形成的hack技术，在HTML5中，为了加强web的功能，提供了websocket技术，它不仅是一种web通信方式，也是一种应用层协议。

它提供了浏览器和服务器之间原生的双全工跨域通信，通过浏览器和服务器之间建立websocket连接（实际上是TCP连接）,在同一时刻能够实现客户端到服务器和服务器到客户端的数据发送。


首先是客户端new 一个websocket对象，该对象会发送一个http请求到服务端，服务端发现这是个webscoket请求，会同意协议转换，发送回客户端一个101状态码的response，以上过程称之为一次握手，经过这次握手之后，客户端就和服务端建立了一条TCP连接，在该连接上，服务端和客户端就可以进行双向通信了。

这时的双向通信在应用层走的就是ws或者wss协议了，和http就没有关系了。

所谓的ws协议，就是要求客户端和服务端遵循某种格式发送数据报文（帧），然后对方才能够理解。

客户端参考代码

	window.onload=function(){
	        var ws=new WebSocket("ws://127.0.0.1:8088");
	        var oText=document.getElementById('message');
	        var oSend=document.getElementById('send');
	        var oClose=document.getElementById('close');
	        var oUl=document.getElementsByTagName('ul')[0];
	        ws.onopen=function(){
	            oSend.onclick=function(){
	                if(!/^\s*$/.test(oText.value)){
	                    ws.send(oText.value);
	                }
	            };
	 
	        };
	        ws.onmessage=function(msg){
	          var str="<li>"+msg.data+"</li>";
	          oUl.innerHTML+=str;
	        };
	        ws.onclose=function(e){
	            console.log("已断开与服务器的连接");
	            ws.close();
	        }
	    }
---

WebSocket在支持它的浏览器上确实提供了一种全双工跨域的通信方案，所以在各以上各种方案中，我们的首选无疑是WebSocket。


> socket.io介绍
> 
> socket.io封装了websocket，同时包含了其它的连接方式，比如Ajax。原因在于不是所有的浏览器都支持websocket，通过socket.io的封装，不用关心里面用了什么连接方式。
> 
> 在任何浏览器里都可以使用socket.io来建立异步的连接。socket.io包含了服务端和客户端的库，如果在浏览器中使用了socket.io的js，服务端也必须同样适用。如果你很清楚你需要的就是websocket，那可以直接使用websocket。

##  SSE

SSE（Server-Sent Event，服务端推送事件）是一种允许服务端向客户端推送新数据的HTML5技术。与由客户端每隔几秒从服务端轮询拉取新数据相比，这是一种更优的解决方案。

与WebSocket相比，它也能从服务端向客户端推送数据。那如何决定你是用SSE还是WebSocket呢？概括来说，WebSocket能做的，SSE也能做，反之亦然，但在完成某些任务方面，它们各有千秋。


SSE技术提供的是从服务器单向推送数据给浏览器的功能，但是配合浏览器主动请求，实际上就实现了客户端和服务器的双向通信。它的原理是在客户端构造一个eventSource对象，该对象具有readySate属性，分别表示如下：

1. 正在连接到服务器；
1. 打开了连接；
1. 关闭了连接。

同时eventSource对象会保持与服务器的长连接，断开后会自动重连，如果要强制断开可以调用它的close方法。

监听onmessage事件，服务端遵循SSE数据传输的格式给客户端，客户端在onmessage事件触发时就能够接收到数据，从而进行某种处理，代码如下。

客户端

	var source=new EventSource('http://localhost:8088/evt');
	    source.addEventListener('message', function(e) {
	        console.log(e.data);
	    }, false);
	    source.onopen=function(){
	        console.log('connected');
	    }
	    source.onerror=function(err){
	        console.log(err);
	    }

服务端

	var http=require('http');
	var fs = require("fs");
	var count=0;
	var server=http.createServer(function(req,res){
	    if(req.url=='/evt'){
	        //res.setHeader('content-type', 'multipart/octet-stream');
	        res.writeHead(200, {"Content-Type":"tex" +
	            "t/event-stream", "Cache-Control":"no-cache",
	            'Access-Control-Allow-Origin': '*',
	            "Connection":"keep-alive"});
	        var timer=setInterval(function(){
	            if(++count==10){
	                clearInterval(timer);
	                res.end();
	            }else{
	                res.write('id: ' + count + '\n');
	                res.write("data: " + new Date().toLocaleString() + '\n\n');
	            }
	        },2000);
	 
	    };
	    if(req.url=='/'){
	        fs.readFile("./sse.html", "binary", function(err, file) {
	            if (!err) {
	                res.writeHead(200, {'Content-Type': 'text/html'});
	                res.write(file, "binary");
	                res.end();
	            }
	        });
	    }
	}).listen(8088,'localhost');

WebSocket是一种更为复杂的服务端实现技术，但它是真正的双向传输技术，既能从服务端向客户端推送数据，也能从客户端向服务端推送数据。


与WebSocket相比，SSE有一些显著的优势，便利：不需要添加任何新组件，用任何你习惯的后端语言和框架就能继续使用。你不用为新建虚拟机、弄一个新的IP或新的端口号而劳神，就像在现有网站中新增一个页面那样简单。我喜欢把这称为既存基础设施优势。
 
SSE的第二个优势是服务端的简洁。相对而言，WebSocket则很复杂，不借助辅助类库基本搞不定


WebSocket和SSE（Server-sent Events）两种技术方案都是html5引进的

参考：
[http://www.52im.net/thread-338-1-1.html](http://www.52im.net/thread-338-1-1.html)

[http://www.52im.net/forum.php?mod=viewthread&tid=338](http://www.52im.net/forum.php?mod=viewthread&tid=338)
