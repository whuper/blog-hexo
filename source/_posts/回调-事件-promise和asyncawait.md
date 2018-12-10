---
title: 回调函数 钩子(hook) promise和async await
date: 2017-09-19

---

## 回调-钩子 promise和async await

### 回调函数(callback)

回调函数的英文解释为：

> A callback is a function that is passed as an argument to another function and is executed after its parent function has completed.

翻译过来就是：回调函数是一个作为变量传递给另外一个函数的函数，它在主体函数执行完之后执行。

function A有一个参数function B，function B会在function A执行完成之后被调用执行。

### 钩子函数(hook)

> 钩子的概念源于Windows的消息处理机制，通过设置钩子，应用程序可以对所有的消息事件进行拦截，然后执行钩子函数，对消息进行想要的处理方式。

**钩子函数在捕获消息的第一时间执行**，回调函数不是

### 钩子函数和回调函数


- js派函数监听事件 => 监听函数就是所谓的钩子函数=>函数钩取事件：**函数主动找事件**=>钩子函数
 
- js预留函数给dom事件，dom事件调用js预留的函数 =>事件派发给函数：**事件调用函数**=>回调函数

### 钩子函数
	let btn = document.getElementById("btn");
	btn.onclick = () => {
	    console.log("i'm a hook");
	}

### 回调函数

	btn.addEventListener("click",() =>{
	    console.log(this.onclick);//undefined
	});

JS由于自身的特殊性（单线程），上面的两段代码都是异步的。

### Promise

JavaScript的代码都是单线程执行的。

导致JavaScript的所有网络操作，浏览器事件，都必须是异步执行。

异步执行可以用回调函数实现,但是不好看，而且不利于代码复用

所谓Promise ，简单说就是一个容器，里面保存着某个未来才回结束的事件(通常是一个异步操作）的结果。从语法上说，Promise是一个对象，从它可以获取异步操作的消息。
 
Promise 对象的状态不受外界影响

三种状态:

- pending：进行中
- fulfilled :已经成功
- rejected 已经失败

状态改变： 
Promise对象的状态改变，只有两种可能：

- 从pending变为fulfilled
- 从pending变为rejected。

这两种情况只要发生，状态就凝固了，不会再变了，这时就称为resolved（已定型）

基本用法
ES6规定，Promise对象是一个构造函数，用来生成Promise实例

	const promist = new Promise(function(resolve,reject){
	    if(/*异步操作成功*/){
	        resolve(value);
	    }else{
	        reject(error);
	    }
	})

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去； 
reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise 实例生成以后，可以用then 方法分别指定resolved状态和rejected状态的回调函数。

	promise.then(function(value){
	//success
	},function(error){
	//failure
	});

### Async/Await

- async/await是写异步代码的新方式，以前的方法有回调函数和Promise。
- async/await是基于Promise实现的，它不能用于普通的回调函数。
- async/await与Promise一样，是非阻塞的。
- async/await使得异步代码看起来像同步代码，这正是它的魔力所在。

#### Async/Await语法
示例中，getJSON函数返回一个promise，这个promise成功resolve时会返回一个json对象。我们只是调用这个函数，打印返回的JSON对象，然后返回"done"。

使用Promise是这样的:

	const makeRequest = () =>
	  getJSON()
	    .then(data => {
	      console.log(data)
	      return "done"
	    })

	makeRequest()
使用Async/Await是这样的:

	const makeRequest = async () => {
	  console.log(await getJSON())
	  return "done"
	}

	makeRequest()

**区别**:

- 函数前面多了一个aync关键字。await关键字只能用在aync定义的函数内。async函数会隐式地返回一个promise，该promise的reosolve值就是函数return的值。(示例中reosolve值就是字符串"done")

- 不能在最外层代码中使用await，因为不在async函数内。

### 为什么Async/Await更好？
#### 1.简洁

由示例可知，使用Async/Await明显节约了不少代码。

- 不需要写.then，
- 不需要写匿名函数处理Promise的resolve值，
- 不需要定义多余的data变量，还避免了嵌套代码。

这些小的优点会迅速累计起来，这在之后的代码示例中会更加明显。

#### 2.错误处理

Async/Await让try/catch可以同时处理同步和异步错误。在下面的promise示例中，try/catch不能处理JSON.parse的错误，因为它在Promise中。我们需要使用.catch，这样错误处理代码非常冗余。并且，在我们的实际生产代码会更加复杂。

	const makeRequest = () => {
	  try {
	    getJSON()
	      .then(result => {
	        // JSON.parse可能会出错
	        const data = JSON.parse(result)
	        console.log(data)
	      })
	      // 取消注释，处理异步代码的错误
	      // .catch((err) => {
	      //   console.log(err)
	      // })
	  } catch (err) {
	    console.log(err)
	  }
	}

使用aync/await的话，catch能处理JSON.parse错误:

	const makeRequest = async () => {
	  try {
	    // this parse may fail
	    const data = JSON.parse(await getJSON())
	    console.log(data)
	  } catch (err) {
	    console.log(err)
	  }
	}

- 3.条件语句
- 4.中间值
- 5.错误栈
- 6.调试
