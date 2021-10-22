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
