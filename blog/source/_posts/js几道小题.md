---
title: JS几道小题
data: 15/7/2017 17:20 
tags: JavaScript
toc: true
categories: Web前端
---

厦门实训js几道小题练习。

<!--more-->

## 2017.7.6日厦门实训js作业 

### 例题1：定义两个整数变量，交换两个变量的值

```js
var a = prompt("请输入a的值");
var b = prompt("请输入b的值");
var t;

t = a;   //t=2
a = b;  //a=3
b = t;  //b=2
alert("交换后a的值：" + a + ",交换后b的值：" + b);
</script>
```

### 例题２：定义三个整数变量，输出一次最大值

```js
var a = prompt("输入第一个数");
var b = prompt("输入第二个数");
var c = prompt("输入第三个数");
var max;

if(a>=b){
	max = a;
} else {
	max = b;
}
if(max<c){
	max = c;
}
alert("最大的值是：" + max);
```

### 例题3：输入三个数从小到大的输出

```js	
var a = prompt("输入第一个数");
var b = prompt("输入第二个数");
var c = prompt("输入第三个数");
var min,max,mid;

if(a>=b){
    if(b>=c){
        max = a;mid = b;min = c;
    } else{
        max = a;mid = c;min = b;
    }
} else{     //a<b
    if(b>=c){
        max = b;mid = a;min = c;
    } else{
        max = b;mid = c;min = a;
    }
}
if(max<c){
    if(a>=b){
        max = c;mid = a;min = b;
    } else{
        max = c;mid = b;min = a;
    }
}
alert("从小到大排序：" + min + "<" + mid +  "<" + max);
```

额....这个写的不是很好，傻瓜式...以后再说

### 例题4：定义整数的成绩变量，按区间输出等级:

```js
var score = prompt("输入成绩：");
var sco = parseInt(score / 10);
switch(sco){
    case 0 :
        alert("不及格！"); break;
    case 1 :
        alert("不及格！"); break;
    case 2 :
        alert("不及格！"); break;
    case 3 :
        alert("不及格！"); break;
    case 4 :
        alert("不及格！"); break;
    case 5 :
        alert("不及格！"); break;
    case 6:
        alert("D"); break;
    case 7:
        alert("C"); break;
    case 8:
        alert("B"); break;
    case 9:
        alert("A"); break;
    case 10:
        alert("A"); break;
    default:
        alert("输入有误！"); break;
}
```

### 例题5：定义一个变量表示年份，判断是否是闰年
	
```js
<!--闰年的条件是:能被4整除,但是不能被100整除,或者能被四百整除-->
var year = prompt("输入一个年份：");

if((year%4==0) && (year%100!==0) || (year%400==0)) {
    alert(year+"年是闰年");
} else {
    alert(year+"年是平年");
}
```

### 例题6：输出100以内的质数

```js
//2,3,5,7,11,13,17,19,23...
//只能被1和自己本身除
for(var n=2;n<=100;n++){
    for(var m=2;m<n;m++){
        if(n%m == 0){
            break;   //跳出当前循环
        }
    }
    if(m>=n){
        document.write( n + ",");
    }
}
```

## 2017.7.7日厦门实训js作业 --循环语句

### 例题1：输出乘法口诀表
	
```js
var num;
for(var i=1;i<10;i++){
    for(var j=1;j<10;j++){
        if(j>i){
            break;
        } else {
            num = i*j;
            document.write(j + "*" + i + "=" + num + "&nbsp;&nbsp;&nbsp;&nbsp;");
        }
    }
    document.write("<br/>");
}
```
	
### 例题2：输出10行10列表格

```js
<button id="btn">1.点击出现表格</button>
<table id="table" cellspacing="0" cellpadding="10" border="1"></table>
   
window.onload = function() {
var btn = document.getElementById("btn");
btn.onclick = out;
	function out(){
		var table = document.getElementById("table");
		for(var j=1;j<=10;j++){
			var tr = document.createElement("tr");
			for(var i=1;i<=10;i++){
				var td = document.createElement("td");   //创建元素节点eleNode
				td.innerHTML = i;
				tr.appendChild(td);   //将元素节点td添加到tr中
			}
		table.appendChild(tr);
		}
	}
}
```

***自己在作业的基础上拓展了一下：获取任意行列的表格并输出***

```js
请输入行数：<input id="rows" /><br/><br/>
请输入列数：<input id="cols" /><br/><br/>
<button id="btn1">2.点击出现你输入的表格</button><br><br/>
<table id="table1" cellspacing="0" cellpadding="10" border="1"></table>

var btn1 = document.getElementById("btn1");
btn1.onclick = out1;
function out1(){
    var table1 = document.getElementById("table1");
    var rows = document.getElementById("rows").value;
    var cols = document.getElementById("cols").value;

    for(var j=1;j<=cols;j++){
        var tr1 = document.createElement("tr");
        for(var i=1;i<=rows;i++){
            var td1 = document.createElement("td");
             td1.innerHTML = i;
            tr1.appendChild(td1);
        }
        table1.appendChild(tr1);
    }
}
```

### 例题3：在上例表格中改变奇数行的背景

```js
<button id="btn">点击后奇数行有背景颜色</button>
<table id="table" border="1" cellpadding="10px" cellspacing="0"></table>

window.onload = function () {
    var btn = document.getElementById("btn");
    btn.onclick = out2;
    function out2() {
        var table = document.getElementById("table");
        for (var j = 1; j <= 10; j++) {
            var tr = document.createElement("tr");
            if (j%2 != 0) {    //判断是否为奇数行
                tr.style.backgroundColor = "red";
            }
            for (var i = 1; i <= 10; i++) {
                var td = document.createElement("td");
                td.innerHTML = i;
                tr.appendChild(td);
            }
            table.appendChild(tr);
        }
    }
}
```

### 例题4：输出序列1,1,2,3,5,8,13...... N个数

```js
var a=1;b=1;
if(a=1){
    document.write(a + " ");
}
if(b=1){
    document.write(b + " ");
}
for(var n=0;n<500;n++){
    n = a + b; //1+1=2 ;n=2 n=3 n=5
    a = b;    //a=1; a=1; a=2; a=3;
    b = n; //b=1 b=2; b=3; b=5;
    ocument.write(n + " ");
}
```

此段代码不够完善...待完善

## 2017.7.10日厦门实训js作业

### 例题1：动态的显示当前时间

```js
function startTime(){
	var today = new Date();
	var year = today.getFullYear();
	var month = today.getMonth() + 1;
	var day = today.getDate();
	var h = today.getHours();
	var m = today.getMinutes();
	var s = today.getSeconds();
	m = checkTime(m);
	s = checkTime(s);

	document.getElementById("time").innerHTML ="现在是北京时间：" + year + "年" + month + "月" + day + "日" + " " + h + ":" + m + ":" + s ;

	t = setTimeout(function(){startTime()},500);
}
function checkTime(i){
    if(i<10){
        i = "0" + i;
    }
    return i;
}
	
<body onload="startTime()">
	<div id="time"></div>
</body>
```

### 例题2：计算器实现加减乘除（最简单的）

```js
数字1：<input id="num1"><br/><br/>
数字2：<input id="num2"><br/><br/>
<button onclick="add()">加法</button>
<button onclick="del()">减法</button>
<button onclick="mul()">乘法</button>
<button onclick="division()">除法</button><br/><br/>
结果为：<input id="result" value="">

<script>
	function add() {
		var num1 = document.getElementById("num1").value;
		var num2 = document.getElementById("num2").value;
		var result = parseFloat(num1) + parseFloat(num2);
		document.getElementById("result").value = result;
	}
	function del() {
		var num1 = document.getElementById("num1").value;
		var num2 = document.getElementById("num2").value;
		var result = parseFloat(num1) - parseFloat(num2);
		document.getElementById("result").value = result;
	}
	function mul() {
		var num1 = document.getElementById("num1").value;
		var num2 = document.getElementById("num2").value;
		var result = num1 * num2;
		document.getElementById("result").value = result;
	}
	function division() {
		var num1 = document.getElementById("num1").value;
		var num2 = document.getElementById("num2").value;
		var result = num1 / num2;
		document.getElementById("result").value = result;
	}
</script>
```