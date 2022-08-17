---
title: 函数节流（throttle）与函数去抖（debounce）
date: 2016-04-16

---

### 函数节流（throttle）与函数去抖（debounce）

## 背景
鼠标的mousemove、scroll，浏览器窗口的resize事件等，都是在短时间内重复触发。以onresize事件为例，若事件处理程序需要进行修改元素宽度高度等操作，那么频繁的触发事件会导致频繁的重绘页面。

DOM操作比非DOM交互需要更多的内存和CPU时间，连续尝试进行过多的DOM相关操作可能会导致浏览器挂起，有时候甚至会崩溃。为了解决这个问题，需要使用定时器对该函数进行节流。

**函数节流**，简单地讲，就是让一个函数无法在很短的时间间隔内连续调用，只有当上一次函数执行后过了你规定的时间间隔，才能进行下一次该函数的调用

### 原理
当触发一个事件时，先setTimout让这个事件延迟一会再执行，如果在这个时间间隔内又触发了事件，就clear掉原来的定时器，再setTimeout一个新的定时器延迟一会执行。

**2018 05-09更新 (其实这个应该叫做函数去抖debounce)**

代码实现

	 function throttle(method, context) {
	     clearTimeout(methor.tId);
	     method.tId = setTimeout(function(){
	         method.call(context);
	     }， 100);
	 }
	 
调用

	window.onresize = function(){
	    throttle(myFunc);
	}
	
这样两次函数调用之间至少间隔100ms。

impress用的是另一个封装函数：

	var throttle = function(fn, delay){
	 	var timer = null;
	 	return function(){
	 		var context = this, args = arguments;
	 		clearTimeout(timer);
	 		timer = setTimeout(function(){
	 			fn.apply(context, args);
	 		}, delay);
	 	};
	 };
它使用闭包的方法形成一个私有的作用域来存放定时器变量timer。而调用方法为

	window.onresize = throttle(myFunc, 100);
	
> 两种方法各有优劣，前一个封装函数的优势在把上下文变量当做函数参数，直接可以定制执行函数的this变量；后一个函数优势在于把延迟时间当做变量（当然，前一个函数很容易做这个拓展），而且个人觉得使用闭包代码结构会更优，且易于拓展定制其他私有变量，缺点就是虽然使用apply把调用throttle时的this上下文传给执行函数，但毕竟不够灵活。

以上参考
[http://www.alloyteam.com/2012/11/javascript-throttle/](http://www.alloyteam.com/2012/11/javascript-throttle/)

### 相关问题
---

## 防止重复发送 Ajax 请求

> 最简单粗暴的是设置flag做判断，或者暂时禁掉按钮

不推荐用外部变量锁定或修改按钮状态的方式，因为那样比较难：

- 要考虑并理解 success, complete, error, timeout 这些事件的区别，并注册正确的事件，一旦失误，功能将不再可用；
- 不可避免地比普通流程要要多注册一个 complete 事件；
- 恢复状态的代码很容易和不相干的代码混合在一起；

我推荐用主动查询状态的方式（A、B，jQuery 为例）或工具函数的方式（C、D）来去除重复操作，并提供一些例子作为参考：

#### A. 独占型提交
只允许同时存在一次提交操作，并且直到本次提交完成才能进行下一次提交。

	module.submit = function() {
	  if (this.promise_.state() === 'pending') {
	    return
	  }
	  return this.promise_ = $.post('/api/save')
	}

#### B. 贪婪型提交

无限制的提交，但是以最后一次操作为准；亦即需要尽快给出最后一次操作的反馈，而前面的操作结果并不重要。

	module.submit = function() {
	  if (this.promise_.state() === 'pending') {
	    this.promise_.abort()
	  }
	  // todo
	}

比如某些应用的条目中，有一些进行类似「喜欢」或「不喜欢」操作的二态按钮。如果按下后不立即给出反馈，用户的目光焦点就可能在那个按钮上停顿许久；如果按下时即时切换按钮的状态，再在程序上用 abort 来实现积极的提交，这样既能提高用户体验，还能降低服务器压力，皆大欢喜。

#### C. 节制型提交

无论提交如何频繁，任意两次有效提交的间隔时间必定会大于或等于某一时间间隔；即以一定频率提交。

	module.submit = throttle(150, function() {
	  // todo
	})

如果客户发送每隔100毫秒发送过来10次请求，此模块将只接收其中6个（每个在时间线上距离为150毫秒）进行处理。

这也是解决查询冲突的一种可选手段，比如以知乎草稿举例，仔细观察可以发现：

编辑器的 blur 事件会立即触发保存；

保存按钮的 click 事件也会立即触发保存；

但是存在一种情况会使这两个事件在数毫秒内连续发生——当焦点在编辑器内部，并且直接去点击保存按钮——这时用 throttle 来处理是可行的。

另外还有一些事件处理会很频繁地使用 throttle，如： resize、scroll、mousemove。

#### D. 懒惰型提交

任意两次提交的间隔时间，必须大于一个指定时间，才会促成有效提交；即不给休息不干活。

	module.submit = debounce(150, function() {
	  // todo
	})

还是以知乎草稿举例，当在编辑器内按下 ctrl + s 时，可以手动保存草稿；如果你连按，程序会表示不理解为什么你要连按，只有等你放弃连按，它才会继续。

---

更多记忆中的例子方式 C 和 方式 D 有时更加通用，比如这些情况：

游戏中你捡到一把威力强大的高速武器，为了防止你的子弹在屏幕上打成一条直线，可以 throttle 来控制频率；

在弹幕型游戏里，为了防止你把射击键夹住来进行无脑游戏，可以用 debounce 来控制频率；

在编译任务里，守护进程监视了某一文件夹里所有的文件（如任一文件的改变都可以触发重新编译，一次执行就需要2秒），但某种操作能够瞬间造成大量文件改变（如 git checkout），这时一个简单的 debounce 可以使编译任务只执行一次。

而方式 C 甚至可以和方式 B 组合使用，比如自动完成组件（Google 首页的搜索就是）：

- 当用户快速输入文本时（特别是打字能手），可以 throttle  keypress 事件处理函数，以指定时间间隔来提取文本域的值，然后立即进行新的查询；

- 当新的查询需要发送，但上一个查询还没返回结果时，可以 abort 未完成的查询，并立即发送新查询；



## 表单防止重复提交

### 1 用js为添加禁用

### 2 使用Post/Redirect/Get

PRG设计模式并不适用所有的重复提交情况，比如：

1）由于服务器响应缓慢，用户刷新提交POST请求造成的重复提交。

2）用户点击后退按钮，返回到数据提交界面，导致的数据重复提交。

3）用户多次点击提交按钮，导致的数据重复提交。

4）用户恶意避开客户端预防多次提交手段，进行重复数据提交。

###3 使用session／token设置令牌

产生页面时，服务器为每次产生的Form分配唯一的随机标识号(比如时间戳)，并且在form的一个隐藏字段中设置这个标识号，同时在当前用户的Session中保存这个标识号。

当提交表单时，服务器比较hidden和session中的标识号是否相同，相同则继续，处理完后清空Session，否则服务器忽略请求。
