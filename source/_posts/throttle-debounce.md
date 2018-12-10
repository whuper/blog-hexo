## 函数节流（throttle）与函数去抖（debounce）

### 一、前言　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

  以下场景往往由于事件频繁被触发，因而频繁执行DOM操作、资源加载等重行为，导致UI停顿甚至浏览器崩溃。

  1. window对象的resize、scroll事件

  2. 拖拽时的mousemove事件

  3. 射击游戏中的mousedown、keydown事件

  4. 文字输入、自动完成的keyup事件

  实际上对于window的resize事件，实际需求大多为停止改变大小n毫秒后执行后续处理；而其他事件大多的需求是以一定的频率执行后续处理。针对这两种需求就出现了debounce和throttle两种解决办法。

### 二、什么是debounce　　　　　　　　　　　　　　　　　　　　　　　　　　　　

 定义

　　如果用手指一直按住一个弹簧，它将不会弹起直到你松手为止。

 也就是说当调用动作n毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间。

接口定义：


		/**
		* 空闲控制 返回函数连续调用时，空闲时间必须大于或等于 idle，action 才会执行
		* @param idle   {number}    空闲时间，单位毫秒
		* @param action {function}  请求关联函数，实际应用需要调用的函数
		* @return {function}    返回客户调用函数
		*/
		debounce(idle,action)

简单实现


	var debounce = function(idle, action){
	  var last
	  return function(){
	    var _this = this, args = arguments
	    clearTimeout(last)
	    last = setTimeout(function(){
	        action.apply(_this, args)
	    }, idle)
	  }
	}

### 三、什么是throttle　　　　　　　　　　　　　　　　　　　　　　　　　　　　　 

定义

　　如果将水龙头拧紧直到水是以水滴的形式流出，那你会发现每隔一段时间，就会有一滴水流出。

　　也就是会说预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期。

接口定义：

	/**
	* 频率控制 返回函数连续调用时，action 执行频率限定为 次 / delay
	* @param delay  {number}    延迟时间，单位毫秒
	* @param action {function}  请求关联函数，实际应用需要调用的函数
	* @return {function}    返回客户调用函数
	*/
	throttle(delay,action)

简单实现


	var throttle = function(delay, action){
	  var last = return function(){
	    var curr = +new Date()
	    if (curr - last > delay){
	      action.apply(this, arguments)
	      last = curr 
	    }
	  }
	}

### 五、总结　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

   throttle和debounce均是通过减少实际逻辑处理过程的执行来提高事件处理函数运行性能的手段，并没有实质上减少事件的触发次数。

两者在概念理解上确实比较容易令人混淆，结合各js库的具体实现进行理解效果将会更好。

---

> throttle:触发-上次动作执行时间〉大于限制时间->执行动作，记录执行时间
> 
> debounce:触发-记录触发时间-上次动作触发时间〉大于限制时间-执行动作
