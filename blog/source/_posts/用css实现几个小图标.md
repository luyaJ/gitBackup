---
title: 用css实现几个小图标
date: 2017-07-21 13:25:59
tags: CSS
toc: true
categories: Web前端
---

### 1. 三角形

<!--more-->

设置一个块元素大小为0，采用`border`属性，制作如下各种图形：

	//1.双色正方形
	height: 0;
	width: 0;
	border-bottom: 100px solid indianred;
	border-left: 100px solid chocolate;

![双色正方形](http://otf35fo4u.bkt.clouddn.com/zfx.png)
	
	//2.三色矩形
	border-right: 100px solid darkcyan;

![三色矩形](http://otf35fo4u.bkt.clouddn.com/jx.png)

	//3.四色正方形
	border-top: 100px solid plum;

![四色正方形](http://otf35fo4u.bkt.clouddn.com/zfx1.png)

	//4.三角形
	height: 0;
	width: 0;
	border-bottom: 100px solid indianred;
	border-left: 100px solid transparent;
	border-right: 100px solid transparent;

![三角形](http://otf35fo4u.bkt.clouddn.com/sjx.png)

### 2.陌陌轮廓

使用圆角属性制作~直接上代码
	
	height: 100px;
	width: 100px;
	background: darkgoldenrod;
	border-radius: 50% 50% 0;

![momo](http://otf35fo4u.bkt.clouddn.com/momo.png)

进一步拓展，制作成小水滴：

	height: 100px;
	width: 100px;
	background: sandybrown;
	border-radius: 50% 50% 0;
	transform: rotate(45deg);

### 3.扭曲的正方形

	height: 100px;
	width: 100px;
	background: mediumslateblue;
	border-radius: 20px 12px 40px / 0.5em 3em;

其中的` border-radius: 2em 1em 4em / 0.5em 3em; `等价于：

	border-top-left-radius: 2em 0.5em;
	border-top-right-radius: 1em 3em;
	border-bottom-right-radius: 4em 0.5em;
	border-bottom-left-radius: 1em 3em;

![扭曲的正方形](http://otf35fo4u.bkt.clouddn.com/zgx2.png)

***注释:*** 如果省略 bottom-left，则与 top-right 相同。如果省略 bottom-right，则与 top-left 相同。如果省略 top-right，则与 top-left 相同。

### 4.箭头

	.demo{
		width: 0;
		height: 0;
		border-top: 15px solid transparent;
		border-right: 15px solid red;
		-webkit-transform: rotate(10deg);
		-o-transform: rotate(10deg);
		-moz-transform: rotate(10deg);
		-ms-transform: rotate(10deg);
	}
	.demo:after{
		content: "";
		position: absolute;
		border-top: 5px solid red;
		border-radius: 20px 0 0 0;
		top: -18px;
		left: -10px;
		width: 15px;
		height: 15px;
		-webkit-transform: rotate(45deg);
		-moz-transform: rotate(45deg);
		-ms-transform: rotate(45deg);
		-o-transform: rotate(45deg);
	}

![arrow](http://otf35fo4u.bkt.clouddn.com/arrow.png)
	
### 5.小星星

思想：三个三角形拼接而成，使用`transform`调整方向。

	.star{
		margin: 100px 0;
		color: red;
		height: 0;
		width: 0;
		border-right: 100px solid transparent;
		border-bottom: 70px solid darkkhaki;
		border-left: 100px solid transparent;
		-webkit-transform: rotate(35deg);
		-o-transform: rotate(35deg);
		-moz-transform: rotate(35deg);
		-ms-transform: rotate(35deg);
	}
	.star:before{
		content: '';
		height: 0;
		width: 0;
		border-bottom: 80px solid darkkhaki;
		border-left: 30px solid transparent;
		border-right: 30px solid transparent;
		position: absolute;
		top: -56px;
		left: -77px;
		-webkit-transform: rotate(-35deg);
		-moz-transform: rotate(-35deg);
		-ms-transform: rotate(-35deg);
		-o-transform: rotate(-35deg);
	}
	.star:after{
		content: '';
		width: 0;
		height: 0;
		border-right: 100px solid transparent;
		border-bottom: 70px solid darkkhaki;
		border-left: 100px solid transparent;
		position: absolute;
		color: red;
		top: 10px;
		left: -115px;
		-webkit-transform: rotate(-70deg);
		-moz-transform: rotate(-70deg);
		-ms-transform: rotate(-70deg);
		-o-transform: rotate(-70deg);
	}

![star](http://otf35fo4u.bkt.clouddn.com/star.png)	

### 6.心形

	.heart{
		width: 100px;
		height: 90px;
	}
	.heart:before,
	.heart:after {
		position: absolute;
		content: '';
		width: 50px;
		height: 80px;
		background: darksalmon;
		border-radius: 50px 50px 0 0;
		left: 50px;
		-webkit-transform: rotate(-45deg);
		-o-transform: rotate(-45deg);
		-ms-transform: rotate(-45deg);
		-moz-transform: rotate(-45deg);
		transform: rotate(-45deg);
		-webkit-transform-origin: 0 100%;
		-moz-transform-origin: 0 100%;
		-ms-transform-origin: 0 100%;
		-o-transform-origin: 0 100%;
		transform-origin : 0 100%;
	}
	.heart:after{
		left: 0;
		-webkit-transform: rotate(45deg);
		-o-transform: rotate(45deg);
		-ms-transform: rotate(45deg);
		-moz-transform: rotate(45deg);
		transform: rotate(45deg);
		-webkit-transform-origin: 100% 100%;
		-moz-transform-origin: 100% 100%;
		-ms-transform-origin: 100% 100%;
		-o-transform-origin: 100% 100%;
		transform-origin :100% 100%;
	}

![heart](http://otf35fo4u.bkt.clouddn.com/heart.png)	

### 7.十字架

	.cross{
		height: 100px;
		width: 20px;
		background: mediumseagreen;
		margin-left: 50px;
		position: relative;
	}
	.cross:after{
		background: mediumseagreen;
		content: '';
		height: 20px;
		position: absolute;
		width: 80px;
		top: 30px;
		left: -30px;
	}

![cross](http://otf35fo4u.bkt.clouddn.com/cross.png)
	

	
