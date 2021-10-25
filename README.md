# 一些前端常见题目
## HTML
1.如何理解HTML语义化？<br>
--让人更容易读懂，增加代码可读性；让搜索引擎更容易读懂，比如有p, h1 <br>
2.默认情况下，哪些HTML标签是块级元素，哪些是内联元素？<br>
--diplay:block/table;有div, h1, table, ul, ol, p等</br>
--diplay:inline/inline-block; 有span, img, input, button 挨着一起直到换行为止</br>
## CSS
key: 布局，定位，图文样式，响应式，CSS3<br>
<b> 布局</b></br>
1.盒模型的宽度怎么计算？<br>
``` javasciript
<!-- 问：div1的offsetWidth是多大？
如果要将offesWidth保持100px要怎么做？-->
<style>
#div1  {
	width: 100px;
	padding: 10px;
	border: 1px solid #ccc;
	margin: 10px;
}
</style>
<div id="div1">
</div>
```
答：offsetWidth = （内容宽度width + 内边距padding+ 边框border），无外边距，结果是122px。
加上boder-sizing =  border-box即可保持100px，压缩的是width
2.margin纵向重叠问题。<br>
```js
<!-- AAA和BBB的距离有多少-->
<style>
	p {
		font-size: 16px;
		line-height: 1;
		margin-top: 10px;
		margin-bottom: 15px;
	}
<style>
<p>AAA<p>
<p><p>
<p><p>
<p>BBB<p>
```
答： 相邻元素的margin-top和margin-bottom会重叠，
空白内容的`<p>`也会重叠

3.margin负值的问题。<br>
答：top向上移动，left向左移动，right则右侧元素向左移动，自身元素不受影响，bottom同理。

4.BFC理解和应用<br>

答：block format context 块级格式化上下文
一块独立渲染区域，内部元素的渲染不会影响边界以外的元素
形成BFC条件：float不是none，positon不是absolute或fixed， overflow不是visible，display是flex inline-block。

常见应用，清除浮动：

```js
<style type="text/css">
	.container {
		background-color: #f1f1f1;
	}
	.left {
		float: left;
	}
</style>
======================
<div class= "container">
		<img src = "https://i0.hdslb.com/bfs/archive/9e5f278027ae7f1e1933b6e4002870361da6c20b.png">
		<p>这是一段文字……</p>
</div>
```
目标效果不一致
![脱离文档流](https://user-images.githubusercontent.com/86351198/138242384-0617ec42-12bc-4a14-b62d-20dec1fe3d18.jpg)
改： 两个元素样式加上class，overflow：hiden


5.float布局的问题（圣杯布局，双飞翼布局），手写clearfix<br>

圣杯布局 &双飞翼布局目的：三栏布局，中间一栏最先加载和渲染（内容最重要）；两侧内容固定，中间内容随着宽度自适应；一般用于PC网页。
```js
<style>
//圣杯布局
        body {
            min-width: 550px;
        }
        #header {
            text-align: center;
            background-color: #f1f1f1;
        }
        #container {
            padding-left: 200px;
            padding-right: 150px;
        }
        #container .column {
            float: left;
        }
        #center {
            background-color: #ccc;
            width: 100%;
        }
        #left {
            background-color: yellow;
            width: 200px;
            margin-left: -100%;
            position: relative;
            right: 200px;
        }
        #right{
            background-color: red;
            width: 150px;
            margin-right: -150px;
        }
        #footer {
            text-align: center;
            background-color: #f1f1f1;
            /* clear: both; */
        }
        /* 手写clearfix */
        .clearfix {
            content:'';
            display: table;
            clear: both;
        }
    </style>
==================
<body>
    <div id="header">this is header</div>
    <div id="container" class="clearfix">
        <div id="center" class="column">this is center</div>
        <div id="left" class="column">this is left</div>
        <div id="right" class="column">this is right</div>
    </div>
    <div id="footer">this is footer</div>
</body>
```

```js
//双飞翼布局
<style>
        body {
            min-width: 550px
        }
        .col {
            float: left;
        }
        #main {
            width: 100%;
            height: 200px;
            background-color: #ccc;
        }

        #main-wrap {
            margin: 0 190px
        }
        #left {
            width: 190px;
            height: 200px;
            background-color: #0000ff;
            margin-left: -100%;
        }
        #right {
            width: 190px;
            height: 200px;
            background-color: #ff0000;
            margin-left: -190px;
        }

    </style>
===========================
<body>
    <div id="main" class="col">
        <div id="main-wrap">
            this is main
        </div>
    </div>

    <div id="left" class="col">
        this is left
    </div>

    <div id="right" class="col">
        this is right
    </div>
</body>
```

6.flex画色子<br>
```js
<style>
        .box {
            width: 200px;
            height: 200px;
            border: 2px solid #ccc;
            border-radius: 10px;
            padding: 20px;
            display: flex;
            justify-content: space-between;
        }
        .item {
            display: block;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: #666;
        }
        .item:nth-child(2) {
            align-self: center;
        }
        .item:nth-child(3) {
            align-self: flex-end;
        }
    </style>
=============================
<body>
    <div class="box">
        <span class="item"></span>
        <span class="item"></span>
        <span class="item"></span>
    </div>
</body>
```
<b> 定位</b></br>
1.absolu和relative分别依据什么定位<br>
答：relative根据自身，absolute根据最近一层的定位元素定位（定位元素：abosolute relative fixed 或 body）


2.居中对齐的方式有哪些？<br>
答：
水平：inline用text-align:center; block用：margin：auto；abosolute用：left：50%+ margin-left负值。
垂直：inline的line-height值等于height值；
absolute的top：50%+ margin-top负值
absolute的transform(-50%, -50%)
absolute的top，left，bottom，right= 0 + margin：auto
3.line-height的继承问题。<br>
p标签的行高为40px，这个百分比略特殊，line-height为px值是直接继承，倍数值则直接乘以p标签的font-size值
```js
<style>
	body {
		font-size: 20px;
		line-height: 200%;
	}
	p {
		font-size: 16px;
	}
</style>
==========================
<body>
	<p>AAA</p>
</body>
```
<b> 响应式</b></br>
1.什么是rem？<br>
答： px是绝对长长度； em是相对长度相对于父元素；rem相对于根元素常用于响应式布局。

2.如何实现响应式<br>
媒体查询media-query，根据不同的的屏幕宽度设置根元素font-size + rem

```js
<style>
@media only screen and (max-width: 374px) {
   html{
      font-size: 86
  }
}

@media only screen and(min-width: 375px) {
  html{
    font-size: 100px
  }
}

@meia only screen and (min-width: 414px){
  html {
    font-size: 110px 
  }
}
body {
  font-size: 0.16rem
}
#dive {
  width: 1rem
}
<style>
```
<b> CSS3</b></br>
1.动画<br>


## Javascript
### part1 数据类型
数值类型：
引用类型：
typeof可判断所有数值类型，引用类型只能是object， instanceof可以区分arry和obj
浅拷贝：
```js
// 浅拷贝way1
let obj = {
	name: 'xiaoming',
	user: {
		name: 'xiaoming777'
	}
}
let a = obj
let b = obj
a.name = 'Marry'
console.log(b.name)//Marry
```
for/in + 递归执行深拷贝：
```js
let user = {
	name: 'xiaoming'
}

let user2 = {}
for (const key in obj){
	user2[key] = obj[key]
}

user2.name = 'Marry'
console.log(user2)
console.log(user)
```
```js
//浅拷贝way2
//Object.assin函数可简单地实现浅拷贝，是将两个对象的属性叠加后面对象属性，会覆盖前面对象同名属性
let user = {
	name: 'xiaoming'
}
let user2 = {
	stu:Object.assign({}, user)
}
user2.stu.name = 'Marry'
console.log(user.name)//xiaoming
```

```js
// 浅拷贝way3，使用展示语法
let user = {
	name: 'xiaoming'
}
let user2 = {...obj}
user2.name = 'Marry'
console.log(user)
console.log(use2)
```



```js
/**
 * 深拷贝
 *
 * @param {*} [obj={}]
 * @return {*} 
 */
function deepClone(obj = {}) {
    if(typeof obj !== 'object' || obj == null || obj === 'symbol') {
        return obj
    }
    // 初始化返回结果
    let result = obj instanceof Array ? [] : {}
    
    for(let key in obj) {
        //保证key不是原型的属性
        if(obj.hasOwnProperty(key)){
	// 这里的递归为了应对属性里面还有子属性
            result[key] = deepClone(obj[key])
        }
    }
    // for循环改成这样也行
    /*
    	for(const [key, value] of Object) {
	//Object.enries(obj)为了保持数组跟对象输出格式相同
	result[key] = typeof value == 'object' ? copy(object[key]): value
	}
    */
    //返回结果
    return result
}
```

### Part2 类&继承&原型链

类：
```js
// 父类
class People {
    constructor(name) {
        this.name = name
    }
    breathe() {
        console.log(`${this.name} is breathing`)
    }
}

class Student extends People {
    constructor(name, number) {
        super(name)
        this.number = number
    }
    sayHi() {
        console.log(
            `姓名 ${this.name}, 序号 ${this.number}`
        );
    }
}

//实例化
const xiaoming = new Student('小明', 100)
console.log(xiaoming.name);
console.log(xiaoming.number)
xiaoming.breathe()

const Marry = new Student('玛丽', 288)
console.log(Marry.name);
console.log(Marry.number)
Marry.sayHi()

```

原型，
```js
//沿用上一部分代码，内容省略……
console.log(typeof Student)//function
console.log(typeof People)//function
console.log(xiaoming.__proto__)//People {}
console.log(Student.prototype)//People {}
console.log(xiaoming.__proto__ == Student.prototype)//true

```
每个class都有显式原型prototype
每个实例都有隐式原型__proto__
实例的__proto__指向对应class的prototype

执行规则： 实例获取属性或执行方法时，现在自身属性和方法找，找不到再找不到从__proto__上找

原型链
![原型链](https://user-images.githubusercontent.com/86351198/138540874-66083fe9-531f-44cf-b811-c39ed1bbfb77.jpg)

```js
console.log(Student.prototype.__proto__)//{}
console.log(People.prototype)//{}
console.log(People.prototype === Student.prototype.__proto__)//true
console.log(xiaoming.hasOwnProperty('name'))//原型链上是否有这个属性
```


手写简易jQury考虑插件和扩展性代码演示。
```js
class jQuery {
    constructor(selector) {
        const result = document.querySelectorAll(selector)
        const length = result.length
        for(let i = 0; i < length; i++) {
            this[i] = result[i]
        }
        this.length = length
        this.selector = selector
    }
    get(index){
        return this[index]
    }
    each(fn) {
        for(let i = 0; i < this.length; i++) {
            const elem = this[i]
            fn(elem)
        }
    }
    on(type, fn) {
        return this.each(elem => {
            elem.addEventListener(type, fn, false)
        })
    }
    // 扩展很多 DOM API

}

/** 
 * 
 <body>
    <p>一段文字 1</p>
    <p>二段文字 2</p>
    <p>三段文字 3</p>
    <script src="./jquery-demo.js"></script>
</body>
*/
// const $p = new jQuery('p')
// $p.get(1)
// $p.each((elem) => console.log(elem.nodeName))
// $p.on(click, ()=> alert('clicke'))

//插件
jQuery.prototype.dialog = function(info) {
    alert(info)
}

const $p = new jQuery('p')
$p.dialog()

// 造轮子
class myJQuery extends jQuery {
    constructor(selector) {
        super(selector)
    }
    //扩展自己的方法
    addClass(className) {

    }
    style(className) {

    }
    style(data) {
        
    }
}



```

### Part3 作用域和闭包
例题1 this的不同应用场景如何取值
例题2 手写bind函数
例题3 实际开发中闭包对的应用场景，举例说明

```js
// 创建10个`<a>`标签，点击打的时候弹出来对应的序号
let i, a
for (i =  0; i < 10; i++) {
	a = document.createElement('a')
	a.innerHTML = i + '<br>'
	a.addEventListener('click', function(e) {
		e.preventDefault()
		alert(i)
	})
	document.body.appendChild(a)
}
```

作用域和自由变量
作用域分类：全局作用域，函数作用域，块级作用域
自由变量： 一个变量在当前作用域没有定义，但被使用了。向上级作用域一层层依次寻找，知道被找到位置。如果全局作用域没有赵傲，则xxx is not define

```js
// 对齐为一个框，不可逃出框外使用变量
//第1个框
	let a = 0
	function fn1() {
		// 第2个框
		let a1 = 100
		function fn2() {
			// 第3个框
			let a2 = 200
			function fn3() {
				// 第4个框
				let a3 = 300
				return a + a1 + a2 + a3
			}
			fn3()
		}
		fn2()
	}
	fn1()
```

闭包
闭包是作用域应用对的特殊情况：函数作为参数被传递，函数作为返回值被返回。
字面描述：闭包是能够读取其他函数内部变量的函数，常用于嵌套函数。
注意：所有的自由变量的查找，是在函数定义的地方，向上级作用域查找，不是在执行的地方，详见下面两个例子。

```js
// 函数作为返回值
function create() {
    let a = 100
    return function() {
        console.log(a)
    }
}

const fn = create()
const a = 200
fn() // 100
```


```js
// 函数作为参数
function print(fn) {
    let a = 100
    fn()
}
const a = 200
function fn() {
    console.log(a)
}

print(fn)//200 自由变量会往所定义对的当前作用域找，找不到就往上一级作用域找
```


this
跟自由变量不同，this的值是在执行时候确认，而不是在定义时候
用法： 作为普通函数-window，使用call apply bind-传入的参数，作为对象方法调用-对象本身，在class方法调用-实例，箭头函数-上一级作用域。
```js
function fn1() {
    console.log(this)
}
fn1()//window

// call 是返回对象
fn1.call({x: 100}) // { x: 100 }

// bind 是返回函数
const fn2 = fn1.bind({ x: 200 })
fn2() // { x: 200 }
```
对比两段代码：
```js
const zhangsan = {
    name: '张三',
    sayHi() {
        // this 即当前对象
        console.log(this)
    },
    wait(){
        setTimeout(function() {
            // this === window
            console.log(this)
        })
    }
}
```

```js
const zhangsan = {
	name:'zhangsan',
	sayHi() {
		// this即当前对象
		console.log(this)
	},
	waitAgain() {
		setTimeOut(() => {
			// this 即当前对象
			// 因为箭头函数的this永远是上一作用域的值
			console.log(this)
		})
	}
}
```
```js
class People {
	constructor(name) {
		this.name = name
		this.age = 20
	}
	sayHi() {
        // 指的是当前执行的实例
		console.log(this)
	}
}
const zhangsan = new People('zhansan')
zhangsan.age = 15
zhangsan.sayHi()// People { name: 'zhangsan', age: 15 }
```

手写bind函数
```js
// 原 bind 使用
function fn1(a, b, c) {
    console.log('this', this)
    console.log(a, b, c)
    return 'this is fn1'
}

const fn2 = fn1.bind({ x: 100}, 10, 20, 30) 
const res = fn2()
console.log(res)
// this { x: 100 }
// 10, 20, 30
// this is fn1
```

```js
// 模拟bind
Function.prototype.bind1 = function() {
    // 将参数拆解为数组
    const args = Array.prototype.slice.call(arguments)
    // 获取this（数组第一项）
    const t = args.shift()// 取出第一项
    // fn1.bind(...) 中的fn1
    const self = this 
    // 返回一个函数
    return function () {
        return self.apply(t, args)
    }
}

function fn1(a, b, c) {
    console.log('this', this)
    console.log(a, b, c)
    return 'this is fn1'
}

const fn2 = fn1.bind1({ x: 100}, 10, 20, 30) 
const res = fn2()
console.log(res)
```
实际开发中闭包的应用： 隐藏数据，cache-demo
```js
// 闭包隐藏数据，只提供API
function createCache() {
    const data = {} // 闭包中的数据，被隐藏，不被外界访问
    return {
        set: function (key, val) {
            data[key] = val
        },
        get: function (key) {
            return data[key]
        }
    }
}

// 如果外界不通过set&get想要修改数据
 console.log(data.b = 200);
 //这样在createCache的作用域外访问，会报错data is not defined

const c = createCache()
c.set('a', 100)
console.log(c.get('a'))//100

```


### Part4 异步和单线程

题目1 同步和异步的区别？（为什么会有异步）
题目2 手写Promise加载一张图片。
题目3 前端使用异步的场景有哪些？

```js
// 输出结果是：13542
console.log(1)
setTimeout(function(){
    console.log(2)
}, 1000)
console.log(3)
setTimeout(function(){
    console.log(4)
}, 0)


```

单线程和异步
JS是单线程语言，只能做一件事
浏览器和nodejs 已经支持JS启动进程，如Web Worker
JS和DOM 渲染共同一个线程，因为JS可以修改DOM结构
日常需求：遇到等待（网络请求，定时任务）不能卡住，所以需要用到异步
异步不会阻塞代码执行，同步会

```js
// 异步
console.log(100)
setTimeout(() => {
	console.log(200)
}, 1000)
console.log(300)
// 输出结果是100 300 200
```

```js
// 同步
console.log(100)
alert(200)
console.log(300)
```

应用场景
网络请求如ajax图片加载
定时任务如setTimeout

```js
//ajax
console.log('start')
$.get('./data1.json', function(data1) {
	console.log(data1)
})
console.log('end')
```
```js
console.log('start')
let img = document.createElement('img')
img.onload = function() {
	console.log('loaded')
}
img.src = 'xxx.png'
console.log('end')
```
```js
// setTinterval
console.log(100)
setInterval(function() {
	console.log(200)
})
console.log(300)
// 输出结果：100 300 200 ……（后续一直隔一秒出现200）
```

callback hell & Promise

```js
// callback hell
// 获取第一份数据
$.get(url, (data1) => {
    console.log(data1)
    // 获取第二份数据
    $.get(url, (data2) => {
        console.log(data2)
        // 获取第三份数据
        $.get(url, (data3) => {
            console.log(data3)
            // 还可能获取更多的数据
        })
    })
})
```

Promise能避免callback hell
```js
// 改善callback hell 如下
function getData(url) {
    return new Promise((resolve, reject) =>{
        $.ajax({
            url,
            success(data) {
                resolve(data)
            },
            error(err) {
                reject(err)
            }
        })
    })
}

const url1 = '/data1.json'
const url2 = '/data2.json'
const url3 = '/data3.json'
getData(url1).then(data1 => {
    console.log(data1)
    return getData(url2)
}).then(data2 => {
    console.log(data2)
    return getData(url3)
}).then(data3 => {
    console.log(data3)
}).catch(err => console.error(err))
```


手写Promise加载一张图片代码演示
```js
function loadImg(src) {
    const p = new Promise(
        (resolve, reject) => {
            const img = document.createElement('img')
            img.onload = () => {
                resolve(img)
            }
            img.onerror = () => {
                reject(new Error(`图片加载失败 ${src}`))
            }
            img.src = src
        }
    )
    return p
}

const url = 'https://tse3-mm.cn.bing.net/th/id/OIP-C.aN5oqiBcLVBLL8gio7cJzAHaEo?pid=ImgDet&rs=1'

loadImg(url).then(img => {
    console.log(img.width)
    return img
}).then(img => {
    console.log(img.height)
}).catch(ex => console.error(ex))
```

手写载入两张图片
```js
function loadImg(src) { ... }
const url1= 'https://tse3-mm.cn.bing.net/th/id/OIP-C.aN5oqiBcLVBLL8gio7cJzAHaEo?pid=ImgDet&rs=1'
const url2 ='https://tse4-mm.cn.bing.net/th/id/OIP-C.EoEXQakiR-SUW-fTVWYnrgHaE6?pid=ImgDet&rs=1'

loadImg(url1).then(img1 => {
    console.log(img1.width)
    return img1 //普通对象
}).then(img1 => {
    console.log(img1.height)
    return img2 // promise实例
}).then(img2 => {
    console.log(img2.width)
    return img2
}).then(img2 => {
    console.log(img2.height)
})
// 可以后续加then多张图片。
```


### Part5 JS Web API

 DOM
 vue和React框架封装了DOM操作
题目1 DOM是哪种数据结构
题目2 DOM操作常用的API
题目3 attr 和property的区别
题目4 一次性插入多个DOM节点，考虑性能问题

DOM本质
HTML通过浏览器解析生成一个树结构

DOM节点操作
获取节点方式：
```js
<style>
    .container {
        border: 1px solid #ccc
    }
    .red {
        color: red
    }
</style>
<body>
    <div id="div1" class="container">
        <p>一段文字 1</p>
        <p>二段文字 2</p>
        <p>三段文字 3</p>
    </div>
    <div id="div2" class="container">
        <img src="https://tse3-mm.cn.bing.net/th/id/OIP-C.aN5oqiBcLVBLL8gio7cJzAHaEo?pid=ImgDet&rs=1">
    </div>
    <script src="./test.js"></script>
</body>

==========
const div = document.getElementById('div1')
console.log('div1',div)

const divList = document.getElementsByTagName('div')//集合
console.log('divList.length', divList.length)
console.log('divList[1]', divList[1])

const containerList = document.getElementsByClassName('contianer')//集合
console.log('containerList.length', containerList.length)
console.log('containerList[1]', containerList[1])

const pList = document.querySelectorAll('p')//标签
console.log('pList', pList)
```


property & attribute
property修改对象属性，不会体现在html结构中，尽量用这个
attribute修改html属性，会改变html结构，这个耗费性能一些
两者都可能引起DOM重新渲染
```js
// html部分同上
const pList = document.querySelectorAll('p')
const p1 = pList[0]

//property 形式
p1.style.width = '100px'
console.log(p1.style.width)//100px
p1.className = 'red'
console.log(p1.className)//red
console.log(p1.nodeName)//P
console.log(p1.nodeType)//1 //nodeType 1 是元素节点，2是属性节点，3是文本节点

//attribute 能真正在DOM里面添加
p1.setAttribute('data-name','dataname1')
console.log(p1.getAttribute('data-name'))//dataname1
// 还可以实现样式
p1.setAttribute('style', 'font-size: 50px;')
console.log(p1.getAttribute('style'))	


```


DOM结构操作
新增、插入节点
获取子元素列表&父元素
```js
//html部分
<body>
    <div id="div1" class="container">
        <p id="p1">一段文字 1</p>
        <p>二段文字 2</p>
        <p>三段文字 3</p>
    </div>
    <div id="div2" class="container">
        <img src="https://tse3-mm.cn.bing.net/th/id/OIP-C.aN5oqiBcLVBLL8gio7cJzAHaEo?pid=ImgDet&rs=1">
    </div>
    <script src="./test.js"></script>
</body>
===========================
const div1 = document.getElementById('div1')
const div2 = document.getElementById('div2')

//新建节点
const newP = document.createElement('p')
newP.innerHTML = 'this is p1'

// 插入节点
div1.appendChild(newP)

// 移动节点
const p1 = document.getElementById('p1')
div2.appendChild(p1)


// 获取子元素列表
const divChildNodes = div1.childNodes
console.log(div1.childNodes)// 这里打印有文本元素，需要去掉
//去掉的过程
const div1ChildNodesP = Array.prototype.slice.call(div1.childNodes).filter(child =>{
    if(child.nodeType === 1) {// 元素的nodeType为1
        return true
    }
    return false
})
console.log('divChildNodeP', div1ChildNodesP);


// 删除节点
div1.removeChild(div1ChildNodesP[0])

```



DOM性能问题

尽量避免频繁的DOM操作，频繁操作会导致卡顿
对DOM查询做缓存
频繁操作改为一次性操作

```js
//不缓存DOM 查询结果
for (let = 0; i < document.getElementsByTagName('p').length; i++) {
	// 每次循环都会计算length，频繁进行DOM查询
}

// 缓存 DOM 查询结果
const pList = document.getElementsByTagName('p')
const length = pList.length
for (let i = 0; i < lenght; i++) {
	//缓存length，只进行一次 DOM 查询
}
```
 
 ```js
 //html 部分
 <body>
 	<ul id="list">
		
	</ul>
 </body>
 ==============================
 //将频繁操作改为一次性操作
 const listNode = document.getElementById('list')
 
 // 创建一个文档片段，此时还没有插入到 DOM 树中
 const frag = document.createDocumentFragment()
 
 // 执行插入
 for (let x = 0; x < 10; x++) {
 	const li = document.createElement("li")
	li.innerHTML = "List item " + x
	frag.appendChild(li)
 }
 
 // 都完成后，在插入到DOM树中
 listNode.appendChild(frag)
 
 ```
 
 BOM-browser object model
 题目1 如何识别浏览器的类型
 题目2 分析拆解url各个部分
 
 BOM操作： navigator（重要）， screen，location（重要），history
 ```js
 
// navigator 检测浏览器
const ua = navigator userAgent
const isChrome = ua.indexOf('Chrome')
console.log(isChrome)

//screen
console.log(screen.width)
console.log(screen.height)

//location 拆解url各个部分
console.log(location.href)
console.log(location.protocol)
console.log(location.pathname)
console.log(location.search)
console.log(location.hash)

//history
history.back()
history.forward()
```
 
 
 
 事件
  题目1 编写一个通用的事件监听函数
  题目2 描述事件冒泡的流程
  题目3 无限下拉的图片列表，如何监听每个图片的点击
 keywords：事件绑定，事件冒泡，事件代理
 
 ```js
 // 事件绑定例子
 const btn = document.getElementsById('btn1')
 btn.addEventListener('click'm event => {
 	console.log('clicked')
 })
 ```
 
 ```js
 //html部分
/* 
<body>
    <botton id="btn1">
        一个按钮
    </button>
</body> 
*/
//通用的事件绑定函数
function bindEvent(elem, type, fn) {
    elem.addEventListener(type, fn)
}

const a = document.getElementById('btn1')
bindEvent(a, 'click', e => {
    // console.log(event.target)
    e.preventDefault()
    alert('clicked')
})
 
 ```
 
 事件冒泡
 ```js
 // html部分
 /*
 <body>
    <button id="btn1">一个按钮</button>
    <div id="div1">
        <p id="p1">激活</p>
        <p id="p2">取消</p>
        <p id="p3">取消</p>
        <p id="p4">取消</p>
    </div>
    <div id="div2">
        <p id="p5">取消</p>
        <p id="p6">取消</p>
    </div>
    <script src="./test.js"></script>
</body>
*/

function bindEvent(elem, type, fn) {
    elem.addEventListener(type, fn)
}

const body = document.body
bindEvent(body, 'click', event => {
    console.log('body clicked')
    console.log(event.target)
})

const div2 = document.getElementById('div2')
bindEvent(div2, 'click', event => {
    // 因为冒泡的规则泡泡从小变大，点击这个元素时候先显示子元素，在显示body
    console.log('div2 clicked')
    console.log(event.target) 
 ```
 
 冒泡的应用
 ```js
 //html部分同上，省略
function bindEvent(elem, type, fn) {
    elem.addEventListener(type, fn)
}

const p1 = document.getElementById('p1')
bindEvent(p1, 'click', event => {
    console.log('激活')
    //阻止冒泡，这样击激活时候只显示'激活'，而不是显示'激活 取消'
    event.stopPropagation()
})

const body = document.body
bindEvent(body, 'click', event => {
    console.log('取消')
    console.log(event.target)
})
 ```

 事件代理
 代码简洁，减少浏览器内存占用，不要滥用
 ```js
 //html部分
 /*
 <body>
   <div id="div3">
       <a href="#">a1</a>
       <a href="#">a2</a>
       <a href="#">a3</a>
       <a href="#">a4</a>
   </div>
   <button>
       点击增加一个a标签
   </button>
    <script src="./test.js"></script>
</body>
 */
 
 =================
 function bindEvent(elem, type, fn) {
    elem.addEventListener(type, fn)
}
// 代理绑定
const div3 = document.getElementById('div3')
    bindEvent(div3, 'click', event => {
        event.preventDefault();
        const target = event.target
        if (target.nodeName === 'A') {
            alert(target.innerHTML)
        }
    })
 
 ```
 
 ```js
 // 通用的事件监听函数
 function bindEvent(elem, type, selector, fn) {
    // 因为普通绑定只有三个参数，需要判断
    if( fn == null) {
        fn = selector
        selector = null
    }
    elem.addEventListener(type, event => {
        const target = event.target
        if(selector) {
            // 这是代理，通过call去改变this
            if(target.matches(selector)){
                fn.call(target, event)
            }
        } else {
            // 这是普通绑定, 通过call去改变this
            fn.call(target, event)
        }
    })
}

// 普通绑定
const btn1 = document.getElementById('btn1')
bindEvent(btn1, 'click', function(event){
    // console.log(event.target)// 获取触发的元素
    event.preventDefault()
    alert(this.innerHTML)
})

// 代理绑定
const div3 = document.getElementById('div3')
bindEvent(div3, 'click','a',function(event) {
    event.preventDefault()
    alert(this.innerHTML)// 如果非要用箭头函数，那这里的this就改为event.target
})
 ```
 
 事件冒泡的流程：
 基于DOM树形结构，事件会顺着出发元素往上冒泡
 应用场景：代理
 
 无限下拉图片列表，如何监听每个图片的点击：
 事件代理，用e.target获取触发元素，用matches来判断是否触发元素
 
 
 
 
 ajax
 
 什么是跨域（同源策略）
 ajax请求时，浏览器要求当前网页和server必须同源（安全），同源：协议，域名，端口，三者都必须一致，否则浏览器会拦住。
 如这两个三者都不同：前端：http://a.com:8080/; server: https://b.com/api/xxx;
 
 加载图片 css js可无视同源策略：
 ```html
 <img src=跨域的图片地址/> (如果有防盗链可能图裂)
 <link href=跨域的css地址/>
 <script src=跨域的js地址></script>
 
 <img/> 可用于统计打点，可使用第三方统计服务
 <link/><script> 可以使用CDN，CDN一般是外域
 <script>可实现JSONP
```
跨域： 所有的跨域都必须经过server端允许和配合；未经serveer端就实现跨域，说明浏览器有漏洞，危险信号。
跨域可以通过JSONP，CORS的方式实现


手写一个简易的ajax
```js
/* 这个过分简陋了
function ajax(url, successFn) {
    const xhr = new XMLHttpRequest()
    xhr.open("GET", url, true)
    xhr.onreadystatechange = function () {
        if(xhr.readyState === 4) {
            if(xhr.status === 200) {
                successFn(xhr.responseText())
            }
        }
    }
    xhr.send(null)
}
*/
```
```js
// 手写ajax
function ajax(url) {
    const p = new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()
        xhr.open('GET', url, true)
        xhr.onreadystateChange = function() {
            if(xhr.readyState === 4) {
                if(xhr.status ===200) {
                    resolve(
                        JSON.parse(xhr.responseText)
                    )
                } else if (xhr.status === 404 || xhr.staus === 500) {
                    reject(new Error('404 not found'))
                }
            }
        }
	xhr.send(null)
    })
    return p
}

const url = '/data/test.json'

ajax(url)
.then(res => console.log(res))
.catch(err => console.log(err))
```

ajax的API：
使用jquery的ajax
```js
//jQuery 完整 ajax示例
$(function(){
    //请求参数
    var list = {}
    $.ajax({
        //请求方式
        type:"POST",
        //请求的媒体类型
        contentType:"application/json;charset=UTF-8",
        //请求地址
        url: "http://127.0.0.1/admin/list",
        //数据，json字符串
        data: JSON.stringify(list),
        //请求成功
        success:function(result) {
            console.log(result)
        },
        //请求失败，包含具体的错误信息
        error:function(e){
            console.log(e.status)
            console.log(e.responseText)
        }
    })
})
```

使用fetch--参考MDN

使用axios--参考axios官方文档
 
 
 JSONP
 访问 https://xxx.com/，服务端一定返回一个html文件吗？-不一定，可以拼接任何内容返回，只要符合html格式。同理，&lt;script src= &quot;https://xxx.com/getData.js&quot; &gt; 也不定返回js文件。
规则：```<script>``` 可以绕过跨域限制；服务器可以任意动态拼接数据返回；所以，<script>就可以获得跨域的数据，只要服务端愿意返回。
	
jsonp原理如下
&lt;body&gt;
    &lt;p&gt;一段文字&lt;/p&gt;
    &lt;script src=&quot;./test.js&quot;&gt;&lt;/script&gt;
    &lt;script&gt;
	//原本是window.callback,js文件内容：
	/*
	callback ({name:'zhangsan')},
	因为url里面callback=abc,
	所以改为 abc({ name: 'zhangsan'})
	*/
	
        window.abc = function(data) {
            // 这是我们跨域得到信息
            console.log(data)
        }
    &lt;/script&gt;
    &lt;!-- http-server -p 8081 --&gt;
    &lt;script src=&quot;http://localhost:8082/json.js?username=zhangsan&callback=abc&quot;&gt;&lt;/script&gt;
&lt;/body&gt;
	 
```js
// 封装上述显式输入方法，jQuery实现jsonp
$.ajax({
    url: 'http://localhost:8882/x-orgin.json',
    dataType: 'jsonp',
    jsonpCallback: 'callback',
    success: function(data) {
        console.log(data)
    }
})
```
	
	
 CORS（服务端支持）
CORS-服务器设置http header
```js
// 第二个参数填写允许跨域的域名称，不建议直接写"*"
response.setHeader("Access-Control-Allow-Origin", "http://localhost:8011")
response.setHeader("Access-Control-Allow-Headers", "X-Request-With")
response.setHeader("Access-Control-Allow-Methods", "PUT,POSH,GET,DELETE,OPTIONS")	

// 接收跨域的cookies
response.setHeader("Access-Control-Allow-Credentials","true")
```
 
	
	
	
 存储
描述cookie localStorage sessionStorage的区别
cookies：本身用于浏览器和server通讯，属于http的一部分；
	被“借用” 到本地存储来；
	可用document.cookie = '...'来修改，如document.cookie='a=100;b=200'；
	缺点：只能存4k，http请求时需要发送到服务端，增加请求数据量；只能用		document.cookie='...'来修改
loacalStorage & sesseionStorage:
	HTML5专门为存储设计，最大可存5M；
	API简单易用setItem getItem，如localStore.setItem('a',100) loacalStorage.getItem('a') sessionStorage.setItem('b', '200') sessionStorage.getItem('b')
	不会随着http请求被发送出去;
	localStorage数据会永久存储，除非代码或手动删除;
	sessionStorage存储只存在于当前会话，浏览器关闭则清空；
	一般用localStorage会更多一些
	
防抖 debounce
监听一个输入框的，文字变化后出发change事件
直接用keyup事件，则会频发触发change事件
防抖：用户输入结束或暂停时，才会触发change事件，否则输入框每次输入一个字母就要更新
```js
/*
html部分
<body>
    <input type="text" id="name">
    
    <script src="./test.js"></script>
</body>
*/
// 抖动实例
const input1 = document.getElementById('input1')
input1.addEventListener('keyup', function() {
    console.log(input1.value)
})

```
防抖
```js
const input1 = document.getElementById('input1')
// let timer = null
// input1.addEventListener('keyup', function() {
//     if(timer) {
//         clearTimeout(timer)
//     }
//     timer = setTimeout(() => {
//         console.log(input1.value)

//         //清空定时器
//         timer = null
//     }, 500)
// })

// 防抖 封装
function debounce(fn, delay = 500) {
    //timer是在闭包中的
    let timer = null
    return function(){
        if(timer) {
            clearTimeout(timer)
        }

        timer = setTimeout(() => {
            fn.apply(this, arguments)
            timer = null
        }, delay)
    }
    
}
input1.addEventListener('keyup', debounce(function(){
    console.log(input1.value)
}),600)
```

	
节流
拖拽一个元素时，要随时拿到该元素被拖拽的位置，直接用drag事件，则会频繁触发，很容易导致卡顿。节流无论拖拽速度多快，都会每隔100ms触发一次
	
```js
/*
<style>
        #div {
            border:1px solid #ccc;
            width: 200px;
            height: 100px;
        }
</style>
<body>
    <div id="div" draggable="true">可拖拽</div>

    <script src="./test.js"></script>
</body>
*/
	
const div1 = document.getElementById('div')

let timer = null
// div1.addEventListener('drag', function(e){
//     if(timer){
//         return
//     }
//     timer = setTimeout(() => {
//         console.log(e.offsetX, e.offsetY)
//         timer = null
//     }, 500)
// })

// 节流
function throttle(fn, delay = 100){
    return function(){
        if(timer) {

        }
        timer = setTimeout(() => {
            fn.apply(this, arguments)
            timer = null
        },delay)
    }
}

div1.addEventListener('drag', throttle(function(e) {
    console.log(e.offsetX, e.offsetY)
}))

```
	
