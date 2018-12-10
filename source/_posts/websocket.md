---
title: websocket技术
date: 2016-06-16

---

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

**本质上是一个基于 TCP 的协议。**

HTTP 协议是一种无状态的、无连接的、单向的应用层协议。它采用了请求/响应模型。通信请求只能由客户端发起，服务端对请求做出应答处理。

这种通信模型有一个弊端：HTTP 协议无法实现服务器主动向客户端发起消息。

这种单向的请求，如果服务器有连续的状态变化，客户端要获知就非常麻烦。大多数 Web 应用程序将通过频繁的异步JavaScript和XML（AJAX）请求实现长轮询。轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）。

### 相对传统http的优势
* WebSocket是HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。
* 在WebSocket API中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。
* 浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。
* 获取 Web Socket 连接后，通过 send() 方法来向服务器发送数据，并通过 onmessage 事件来接收服务器返回的数据。


#### 创建WebSocket对象。

	var Socket = new WebSocket(url, [protocol] );
	
### 属性
* Socket.readyState	只读属性 readyState 表示连接状态，可以是以下值：

	1. - 表示连接尚未建立。
	1. - 表示连接已建立，可以进行通信。
	2. - 表示连接正在进行关闭。
	3. - 表示连接已经关闭或者连接不能打开。
	
* Socket.bufferedAmount	只读属性 bufferedAmount 已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数。

### 事件

* open	  	连接建立时触发
* message  客户端接收服务端数据时触发
* error	  	通信发生错误时触发
* close	  	连接关闭时触发

### 方法

* Socket.send()	使用连接发送数据
* Socket.close()	关闭连接


示例

	// 初始化一个 WebSocket 对象
	var ws = new WebSocket("ws://localhost:9998/echo");
	
	// 建立 web socket 连接成功触发事件
	ws.onopen = function () {
	  // 使用 send() 方法发送数据
	  ws.send("发送数据");
	  alert("数据发送中...");
	};
	
	// 接收服务端数据时触发事件
	ws.onmessage = function (evt) {
	  var received_msg = evt.data;
	  alert("数据已接收...");
	};
	
	// 断开 web socket 连接成功触发事件
	ws.onclose = function () {
	  alert("连接已关闭...");
	};
	

socket.io 是基于 WebSocket 的 C-S 实时通信库
