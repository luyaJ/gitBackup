---
title: JS实现轮播图
toc: true
date: 2017-01-31 16:10:42
tags: JavaScript
---

 这个轮播图是在2016年12月完成的，是我用js完成的第一个小东西。  

<!--more-->

### html代码实现 

    <div id="content">
		<div id="images" style="left: -600px;"> 
			<img src="images/5.jpg">
			<img src="images/1.jpg">
			<img src="images/2.jpg">
			<img src="images/3.jpg">
			<img src="images/4.jpg">
			<img src="images/5.jpg">
			<img src="images/1.jpg">
		</div>
		<div id="circle">
			<span class="on" index="1"></span>
			<span index="2"></span>
			<span index="3"></span>
			<span index="4"></span>
			<span index="5"></span>
		</div>
		<div class="btn" id="btn_l">&lt;</div>
		<div class="btn" id="btn_r">&gt;</div>
	</div>

### css部分代码实现 

 1."content" 处设置相对位置：`position:relative`; 图片展示为只显示一张图片，所以溢出content容器部分使用属性：`overflow:hidden`;

 2.左、右按钮部分代码：

	position: relative;  
	cursor: pointer;  //鼠标移上去，显示手指妆
	height: 50px;
	width: 30px;
	background: rgba(0,0,0,0.3);
	top: 50%;
	font-size: 40px;
	text-align: center;
	line-height: 50px;
	display: none;  //当鼠标移上去时，btn出现
	color: #fff;

### javascript部分代码实现

#### 点击< >按钮让图片左右移动 

	function animate(offset){
		var newLeft = parseInt(images.style.left) + offset;
		images.style.left = newLeft + 'px';
		if (newLeft<-3000) {
			images.style.left = -600 + 'px';
		}
		if (newLeft>-600) {
			images.style.left = -3000 + 'px';
		}
	}

	btn_l.onclick=function(){
		animate(600);	
	}

	btn_r.onclick=function(){
		animate(-600);
	}

if循环语句是实现图片无限循环；

这时，轮播图可以实现左右点击，图片相应变化的功能。

#### 设置轮播图的定时器 

	var timer;
	function play(){
		timer = setInterval(function() {
			btn_r.onclick()
		},3000)    //每3000ms图片自动移动
	}
	play();

使用setIntervel()设置定时器。

#### 清除轮播图的定时器

	function stop() {
		clearInterval(timer);
	}
	content.onmouseover = stop;
	content.onmouseout = play;

图片自动移动时，当把鼠标悬停在content内时，图片暂停移动。

#### 小圆点随图片变化相应变化 

	function showCircle(){
		for (var i=0;i<circle.length;i++){
			if(circle[i].className=='on'){
				circle[i].className='';
			}
		}
		circle[index-1].className='on';
	}

for循环用来清除之前circle的样式；

这时，点击< > 按钮小圆点会随图片相应点亮。

	for (var i=0;i<circle.length;i++){
		circle[i].onclick=function(){
			var clickIndex=parseInt(this.getAttribute('index'));
			var offset=600*(index-clickIndex);
			animate(offset);
			index=clickIndex;
			showCircle();       
		}
	}

利用getAttribute('')获取自定义index，之后

	btn_l.onclick=function(){
		index -= 1;
		if(index<1){
			index=5;
		}
		showCircle();
		animate(600);	
	}
	btn_r.onclick=function(){
		index += 1;
		if(index>5){
			index=1;
		}
		showCircle();
		animate(-600);
	}

