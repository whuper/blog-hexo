---

title: ES6新特性
date: 2020-04-12

---

## ECMAScript 6(ES2015 )

European Computer Manufacturers Association Script, 欧洲计算机制造商协会

### 1 ECMAScript 和 JavaScript 的关系
前者是后者的规格，后者是前者的一种实现

### 6 Babel 转码器

Babel 是一个广泛使用的 ES6 转码器，可以将 ES6 代码转为 ES5 代码，从而在现有环境执行。这意味着，你可以用 ES6 的方式编写程序，又不用担心现有环境是否支持。

	// 转码前
	input.map(item => item + 1);
	
	// 转码后
	input.map(function (item) {
	  return item + 1;
	});

上面的原始代码用了箭头函数，Babel 将其转为普通函数，就能在不支持箭头函数的 JavaScript 环境执行了。

### 1.变量声明const和let


> 不存在变量提升
var命令会发生”变量提升“现象，即变量可以在声明之前使用，值为undefined。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。

**为了纠正这种现象，let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。**

const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。

**const的作用域与let命令相同：只在声明所在的块级作用域内有效。**

我们都是知道在ES6以前，var关键字声明变量。无论声明在何处，都会被视为声明在函数的最顶部(不在函数内即在全局作用域的最顶部)。这就是函数变量提升例如:

	  function aa() {
	    if(bool) {
	        var test = 'hello man'
	    } else {
	        console.log(test)
	    }
	  }

以上的代码实际上是：
	
	  function aa() {
	    var test // 变量提升
	    if(bool) {
	        test = 'hello man'
	    } else {
	        //此处访问test 值为undefined
	        console.log(test)
	    }
	    //此处访问test 值为undefined
	  }
  
所以不用关心bool是否为true or false。实际上，无论如何test都会被创建声明。

接下来ES6主角登场：		
我们通常用let和const来声明，let表示变量、const表示常量。	
let和const都是块级作用域。怎么理解这个块级作用域？

在一个函数内部
在一个代码块内部
> 说白了 {}大括号内的代码块即为let 和 const的作用域。
看以下代码：

	  function aa() {
	    if(bool) {
	       let test = 'hello man'
	    } else {
	        //test 在此处访问不到
	        console.log(test)
	        //ReferenceError: Can't find variable: test
	    }
	  }
**let的作用域是在它所在当前代码块，但不会被提升到当前函数的最顶部。**

再来说说const。

    const name = 'lux'
    name = 'joe' //再次赋值此时会报错
说一道面试题

    var funcs = []
    for (var i = 0; i < 10; i++) {
        funcs.push(function() { console.log(i) })
    }
    funcs.forEach(function(func) {
        func()
    })

这样的面试题是大家常见，很多同学一看就知道输出 10 十次
但是如果我们想依次输出0到9呢？两种解决方法。直接上代码。

    // ES5告诉我们可以利用闭包**(错误,应该叫立即执行函数)**解决这个问题
    var funcs = []
    for (var i = 0; i < 10; i++) {
        func.push((function(value) {
            return function() {
                console.log(value)
            }
        }(i)))
    }
    // es6
    for (let i = 0; i < 10; i++) {
        func.push(function() {
            console.log(i)
        })
    }

达到相同的效果，es6简洁的解决方案是不是更让你心动！！！

### 2.模板字符串

es6模板字符简直是开发者的福音啊，解决了ES5在字符串功能上的痛点。

第一个用途，基本的字符串格式化。将表达式嵌入字符串中进行拼接。用${}来界定。

    //es5 
    var name = 'lux'
    console.log('hello' + name)
    //es6
    const name = 'lux'
    console.log(`hello ${name}`) //hello lux
    
第二个用途，在ES5时我们通过反斜杠(\)来做多行字符串或者字符串一行行拼接。ES6反引号(``)直接搞定。

    // es5
    var msg = "Hi \
    man!
    "
    // es6
    const template = `<div>
        <span>hello world</span>
    </div>`
对于字符串es6当然也提供了很多厉害的方法。说几个常用的。

    // 1.includes：判断是否包含然后直接返回布尔值
    let str = 'hahay'
    console.log(str.includes('y')) // true
    // 2.repeat: 获取字符串重复n次
    let s = 'he'
    console.log(s.repeat(3)) // 'hehehe'
    //如果你带入小数, Math.floor(num) 来处理

### 3.函数  箭头函数

函数默认参数

在ES5我们给函数定义参数默认值是怎么样？

    function action(num) {
        num = num || 200
        //当传入num时，num为传入的值
        //当没传入参数时，num即有了默认值200
        return num
    }
但细心观察的同学们肯定会发现，num传入为0的时候就是false， 此时num = 200 与我们的实际要的效果明显不一样

ES6为参数提供了默认值。在定义函数时便初始化了这个参数，以便在参数没有被传递进去时使用。

    function action(num = 200) {
        console.log(num)
    }
    action() //200
    action(300) //300

### 函数的快捷写法

ES6很有意思的一部分就是函数的快捷写法。也就是箭头函数。

箭头函数最直观的三个特点。

不需要function关键字来创建函数
省略return关键字
继承当前上下文的 this 关键字
//例如：

    [1,2,3].map( x => x + 1 )
    
//等同于：

    [1,2,3].map((function(x){
        return x + 1
    }).bind(this))
说个小细节。

当你的函数有且仅有一个参数的时候，是可以省略掉括号的。

当你函数返回有且仅有一个表达式的时候可以省略{}；

例如:

    var people = name => 'hello' + name
    //参数name就没有括号
作为参考

    var people = (name, age) => {
        const fullName = 'h' + name
        return fullName
    } 
    //如果缺少()或者{}就会报错

### 4.拓展的对象功能

对象初始化简写

ES5我们对于对象都是以键值对的形式书写，是有可能出现键值对重名的。例如：

    function people(name, age) {
        return {
            name: name,
            age: age
        };
    }
**键值对重名**，ES6可以简写如下：

    function people(name, age) {
        return {
            name,
            age
        };
    }
ES6 同样改进了为对象字面量方法赋值的语法。ES5为对象添加方法：

    const people = {
        name: 'lux',
        getName: function() {
            console.log(this.name)
        }
    }
ES6通过省略冒号与 function 关键字，将这个语法变得更简洁

    const people = {
        name: 'lux',
        getName () {
            console.log(this.name)
        }
    }
ES6 对象提供了Object.assign()这个方法来实现浅复制。

Object.assign()可以把任意多个源对象自身可枚举的属性拷贝给目标对象，然后返回目标对象。第一参数即为目标对象。在实际项目中，我们为了不改变源对象。一般会把目标对象传为{}

    const obj = Object.assign({}, objA, objB)
### 5.更方便的数据访问--解构

数组和对象是JS中最常用也是最重要表示形式。为了简化提取信息，ES6新增了解构，这是将一个数据结构分解为更小的部分的过程

ES5我们提取对象中的信息形式如下：

    const people = {
        name: 'lux',
        age: 20
    }
    const name = people.name
    const age = people.age
    console.log(name + ' --- ' + age)
是不是觉得很熟悉，没错，在ES6之前我们就是这样获取对象信息的，一个一个获取。现在，解构能让我们从对象或者数组里取出数据存为变量，例如

    //对象
    const people = {
        name: 'lux',
        age: 20
    }
    const { name, age } = people
    console.log(`${name} --- ${age}`)
    //数组
    const color = ['red', 'blue']
    const [first, second] = color
    console.log(first) //'red'
    console.log(second) //'blue'
    
### 6.Spread Operator 展开运算符

ES6中另外一个好玩的特性就是Spread Operator 也是三个点儿...接下来就展示一下它的用途。

组装对象或者数组

    //数组
    const color = ['red', 'yellow']
    const colorful = [...color, 'green', 'pink']
    console.log(colorful) //[red, yellow, green, pink]
    
    //对象
    const alp = { fist: 'a', second: 'b'}
    const alphabets = { ...alp, third: 'c' }
    console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"
	}
有时候我们想获取数组或者对象除了前几项或者除了某几项的其他项

    //数组
    const number = [1,2,3,4,5]
    const [first, ...rest] = number
    console.log(rest) //2,3,4,5
    //对象
    const user = {
        username: 'lux',
        gender: 'female',
        age: 19,
        address: 'peking'
    }
    const { username, ...rest } = user
    console.log(rest) //{"address": "peking", "age": 19, "gender": "female"
	}
对于 Object 而言，还可以用于组合成新的 Object 。(ES2017 stage-2 proposal) 当然如果有重复的属性名，右边覆盖左边

    const first = {
        a: 1,
        b: 2,
        c: 6,
    }
    const second = {
        c: 3,
        d: 4
    }
    const total = { ...first, ...second }
    console.log(total) // { a: 1, b: 2, c: 3, d: 4 }
### 7.import 和 export

import导入模块、export导出模块


### 8. Promise

在promise之前代码过多的回调或者嵌套，可读性差、耦合度高、扩展性低。通过Promise机制，扁平化的代码机构，大大提高了代码可读性；用同步编程的方式来编写异步代码，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。
说白了就是用同步的方式去写异步代码。

发起异步请求

    fetch('/api/todos')
      .then(res => res.json())
      .then(data => ({ data }))
      .catch(err => ({ err }));
今天看到一篇关于面试题的很有意思。

    setTimeout(function() {
      console.log(1)
    }, 0);
    new Promise(function executor(resolve) {
      console.log(2);
      for( var i=0 ; i<10000 ; i++ ) {
        i == 9999 && resolve();
      }
      console.log(3);
    }).then(function() {
      console.log(4);
    });
    console.log(5);
Excuse me？这个前端面试在搞事！

当然以上promise的知识点，这个只是冰山一角。需要更多地去学习应用。

### 9.Generators

生成器（ generator）是能返回一个迭代器的函数。生成器函数也是一种函数，最直观的表现就是比普通的function多了个星号*，在其函数体内可以使用yield关键字,有意思的是函数会在每个yield后暂停。

这里生活中有一个比较形象的例子。咱们到银行办理业务时候都得向大厅的机器取一张排队号。你拿到你的排队号，机器并不会自动为你再出下一张票。也就是说取票机“暂停”住了，直到下一个人再次唤起才会继续吐票。

OK。说说迭代器。当你调用一个generator时，它将返回一个迭代器对象。这个迭代器对象拥有一个叫做next的方法来帮助你重启generator函数并得到下一个值。next方法不仅返回值，它返回的对象具有两个属性：done和value。value是你获得的值，done用来表明你的generator是否已经停止提供值。继续用刚刚取票的例子，每张排队号就是这里的value，打印票的纸是否用完就这是这里的done。

    // 生成器
    function *createIterator() {
        yield 1;
        yield 2;
        yield 3;
    }
    
    // 生成器能像正规函数那样被调用，但会返回一个迭代器
    let iterator = createIterator();
    
    console.log(iterator.next().value); // 1
    console.log(iterator.next().value); // 2
    console.log(iterator.next().value); // 3
那生成器和迭代器又有什么用处呢？

围绕着生成器的许多兴奋点都与异步编程直接相关。异步调用对于我们来说是很困难的事，我们的函数并不会等待异步调用完再执行，你可能会想到用回调函数，（当然还有其他方案比如Promise比如Async/await）。

生成器可以让我们的代码进行等待。就不用嵌套的回调函数。使用generator可以确保当异步调用在我们的generator函数运行一下行代码之前完成时暂停函数的执行。

那么问题来了，咱们也不能手动一直调用next()方法，你需要一个能够调用生成器并启动迭代器的方法。就像这样子的

    function run(taskDef) { //taskDef即一个生成器函数

        // 创建迭代器，让它在别处可用
        let task = taskDef();

        // 启动任务
        let result = task.next();
    
        // 递归使用函数来保持对 next() 的调用
        function step() {
    
            // 如果还有更多要做的
            if (!result.done) {
                result = task.next();
                step();
            }
        }
    
        // 开始处理过程
        step();
    
    }
生成器与迭代器最有趣、最令人激动的方面，或许就是可创建外观清晰的异步操作代码。你不必到处使用回调函数，而是可以建立貌似同步的代码，但实际上却使用 yield 来等待异步操作结束。


## 箭头函数

### 基本用法

	var f = () => 5;
	// 等同于
	var f = function () { return 5 };
	
	var sum = (num1, num2) => num1 + num2;
	// 等同于
	var sum = function(num1, num2) {
	  return num1 + num2;
	};

如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。

	var sum = (num1, num2) => { return num1 + num2; }
由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号。

	var getTempItem = id => ({ id: id, name: "Temp" });

箭头函数有几个使用注意点。

1 箭头函数没有自己的 this，其内部的 this 绑定到它的外围作用域。对象内部的箭头函数若有this，则指向对象的外围作用域。

	window.color = "red";
	//let 声明的全局变量不具有全局属性，即不能用window.访问
	let color = "green";
	let obj = {
	    color: "blue",
	　　 getColor: () => {
	　　　　 return this.color;//this指向window
	　　 }
	};
	let sayColor = () => {
	    return this.color;//this指向window
	};
	obj.getColor();//red
	sayColor();//red

2 箭头函数无法使用 call（）或 apply（）来改变其运行的作用域。

## export default 和 export 区别：

* 1.export与export default均可用于导出常量、函数、文件、模块等
* 2.你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用
* 3.在一个文件或模块中，export、import可以有多个，export default仅有一个
* 4.通过export方式导出，在导入时要加{ }，export default则不需要

---
	1.export
	//a.js
	export const str = "blablabla~";
	export function log(sth) { 
	  return sth;
	}
	对应的导入方式：
		
	//b.js
	import { str, log } from 'a'; //也可以分开写两次，导入的时候带花括号
		
	2.export default
	//a.js
	const str = "blablabla~";
	export default str;
	对应的导入方式：
		
	//b.js
	import str from 'a'; //导入的时候没有花括号
	
**使用export default命令，为模块指定默认输出，这样就不需要知道所要加载模块的变量名**

	//a.js
	let sex = "boy";
	export default sex（sex不能加大括号）
	//原本直接export sex外部是无法识别的，加上default就可以了.但是一个文件内最多只能有一个export default。
	其实此处相当于为sex变量值"boy"起了一个系统默认的变量名default，自然default只能有一个值，所以一个文件内不能有多个export default。

---
	// b.js
	本质上，a.js文件的export default输出一个叫做default的变量，然后系统允许你为它取任意名字。所以可以为import的模块起任何变量名，且不需要用大括号包含
	import any from "./a.js"
	import any12 from "./a.js" 
	console.log(any,any12)   // boy,boy



## 函数调用中使用展开运算符
在以前我们会使用apply方法来将一个数组展开成多个参数：

	function test(a, b, c) { }
	var args = [0, 1, 2];
	test.apply(null, args);
如上，我们把args数组当作实参传递给了a,b,c，这边正是利用了Function.prototype.apply的特性。

### 不过有了ES6，我们就可以更加简洁地来传递数组参数：

	function test(a,b,c) { }
	var args = [0,1,2];
	test(...args);
我们使用...展开运算符就可以把args直接传递给test()函数。

### 数组字面量中使用展开运算符

在ES6的世界中，我们可以直接加一个数组直接合并到另外一个数组当中：

	var arr1=['a','b','c'];
	var arr2=[...arr1,'d','e']; //['a','b','c','d','e']

### 展开运算符也可以用在push函数中，可以不用再用apply()函数来合并两个数组：

	var arr1=['a','b','c'];
	var arr2=['d','e'];
	arr1.push(...arr2); //['a','b','c','d','e']

### 用于解构赋值
解构赋值也是ES6中的一个特性，而这个展开运算符可以用于部分情景：

	let [arg1,arg2,...arg3] = [1, 2, 3, 4];
	arg1 //1
	arg2 //2
	arg3 //['3','4']
展开运算符在解构赋值中的作用跟之前的作用看上去是相反的，将多个数组项组合成了一个新数组。

不过要注意，解构赋值中展开运算符只能用在最后：

	let [arg1,...arg2,arg3] = [1, 2, 3, 4]; //报错
类数组对象变成数组
展开运算符可以将一个类数组对象变成一个真正的数组对象：

	var list=document.getElementsByTagName('div');
	var arr=[..list];
list是类数组对象，而我们通过使用展开运算符使之变成了数组。


## CommonJs模块规范和ES6模块规范

#### 一.CommonJs模块：

模块化规范中，每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

首先我们要明白一个前提，CommonJS模块规范和ES6模块规范完全是两种不同的概念。


CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。

**加载某个模块，其实是加载该模块的module.exports属性**

Node应用由模块组成，采用CommonJS模块规范。

- module.exports =  ...   :      '只能输出一个，且后面的会覆盖上面的' ，

- exports. ...  : ' 可以输出多个'，

- 运行阶段确定接口，运行时才会加载模块，

- 模块是对象，加载的是该对象，

- 加载的是整个模块，即将所有的接口全部加载进来，

- 输出是值的拷贝，即原来模块中的值改变不会影响已经加载的该值，

- this 指向当前模块


#### 二.ES6模块：

模块输出方式：export  和 export default


模块输入方式：import ... from ...

- export   :      '可以输出多个，输出方式为 {}' ，

- export  default : ' 只能输出一个 ，可以与export 同时输出，但是不建议这么做'，

- 解析阶段确定对外输出的接口，解析阶段生成接口，

- 模块不是对象，加载的不是对象，

- 可以单独加载其中的某个接口（方法），

- 静态分析，动态引用，输出的是值的引用，值改变，引用也改变，即原来模块中的值改变则该加载的值也改变，

- this 指向undefined


需要特别注意的是，**export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。**

```
	// 报错
	export 1;
	
	// 报错
	var m = 1;
	export m;

```    

上面两种写法都会报错，因为没有提供对外的接口。第一种写法直接输出 1，第二种写法通过变量m，还是直接输出 1。**1只是一个值，不是接口**。

正确的写法是下面这样。

	// 写法一
	export var m = 1;
	
	// 写法二
	var m = 1;
	export {m};
	
	// 写法三
	var n = 1;
	export {n as m};

## Promise的基本用法

声明一个Promise对象

	// 方法1
	let promise = new Promise ( (resolve, reject) => {
	    if ( success ) {
	        resolve(a) // pending ——> resolved 参数将传递给对应的回调方法
	    } else {
	        reject(err) // pending ——> rejectd
	    }
	} )
	
	// 方法2
	function promise () {
	    return new Promise ( function (resolve, reject) {
	        if ( success ) {
	            resolve(a)
	        } else {
	            reject(err)
	        }
	    } )
	}


---

	The woods are lovely,dark and deep.
	But I have promises to keep.
	And miles to go before I sleep。		
	
	--Robert Frost


