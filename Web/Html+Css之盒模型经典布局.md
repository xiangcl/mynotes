---
  title: HTML+CSS之盒模型经典布局
  tags:
    - Web前端
    - html+css
  categories: Web前端
  permalink: web-front-end/html-css-box-model
---

## css核心内容——流

### 标准流/非标准流

- 流：在现实生活中就是流水，在网页设计中就是指元素（标签）的排列方式（默认情况下，我们的元素，向网页的左上角流动）；
- 标准流：元素在网页中就像流水，排在前面的元素（标签）内容在前面出现，排在后面的元素在后面出现。
- 非标准流：当某个标签脱离了标准流（比如因为相对定位）排列，我们统称为非标准流排列。

## css核心类容——盒子模型

### 盒子模型-概念

- 要搞清楚盒子模型就必须先明白下面几个概念：
- 在网页设计中常听到的属性名：内容（content）、填充（padding）、边框（border）、边界（margin），css盒子模型都举杯这些属性。

- ![](http://i.imgur.com/3t2TA1K.png)

#### 盒子模型经典案例1：
- 实际效果：
    - ![](http://i.imgur.com/SgBg1DB.png)
- 代码：
        <!DOCTYPE html>
        <html>
        <head>
        	<title>盒子模型</title>
        	<style type="text/css">
        		#div-box1 {
        			width:200px;
        			height:200px;
        			background-color:pink;
        			border:1px solid red;
        			margin-top:10px;
        			margin-left:20px;
        			padding-top:20px;
        		}
        	</style>
        </head>
        <body>
        	<div id="div-box1">hello,world!</div>
        </body>
        </html>

- 对应的盒子大概的样子：
    - ![](http://i.imgur.com/xLxCWEJ.png)



#### 盒子模型的经典案例2：
- 实际效果：
    - ![](http://i.imgur.com/FVzVBlc.png)

- 代码：
        <!DOCTYPE html>
        <html>
        <head>
        	<title>盒子模型经典案例-优酷牛人</title>
        	<style type="text/css">
        		*{
        			margin: 0;
        			padding:0;
        		}
        		#div-box1{
        			width:600px;
        			height:500px;
        			border:1px solid #ccc;
        			margin:20px 0 0 50px;
        		}
        		.faceul{
        			list-style-type:none;
        		}
        		.faceul li{
        			width:80px;
        			height:100px;
        			border:1px solid red;
        			margin:10px 0 0 10px;
        			float:left;
        		}
        		.faceul img{
        			width:70px;
        			margin:5px 0  0 5px;
        			
        		}
        		.faceul span{
        			font-size:small;
        			margin: 2px 0 0 25px;
        			display:block;
        		}
        		.faceul a:link{
        			color:black;
        			text-decoration:none;
        		}
        		.faceul a:hover{
        			color:red;
        			text-decoration:underline;
        		}
        	</style>
        </head>
        <body>
        	<div id="div-box1">
        		<ul class="faceul">
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        			<li><img src="ali.jpg"><span><a href="#">阿狸</a></span></li>
        
        		</ul>
        	</div>
        </body>
        </html>