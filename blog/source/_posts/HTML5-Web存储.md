---
title: HTML5 Web存储
toc: true
date: 2017-07-24 14:02:21
tags: HTML5
categories: Web前端
---

HTML5使用JavaScript来存储和访问数据，提供了两种在客户端存储数据的新方法：

<!--more-->

- localStorage --没有时间限制的数据存储。第二天、第二周或下一年之后，数据依然可用。
- sessionStorage --针对一个session的数据存储。当用户关闭浏览器窗口后，数据会被删除。

## localStorage方法

对用户访问页面的次数进行计数：

	<script>
		if (localStorage.pagecount) {
			localStorage.pagecount=Number(localStorage.pagecount) +1;
		} else {
			localStorage.pagecount=1;
		}
		document.write("Visits "+ localStorage.pagecount + " time(s).");
	</script>

## sessionStorage方法

用户在当前 session 中访问页面的次数进行计数：

	<script>
		if(sessionStorage.pageCount){
			sessionStorage.pageCount = Number(sessionStorage.pageCount) +1;
		 } else {
			sessionStorage.pageCount = 1;
		}
		 document.write("visits: " + sessionStorage.pageCount + " 次");
	</script>

## 如何工作?

早在1995,2010年，浏览器存储是利用cookie在浏览器上存储信息，但cookie仅限4k数据。

现如今的浏览器都很慷慨，都会提供5~10M（每个域）的存储空间。创建HTML5的本地存储时还充分考虑了Web应用（和移动应用）。所谓**本地存储**，就是指你的应用可以把数据存储在浏览器上，从而减少与服务器之间所需的通信。

### 如何做到的？

1. 使用这个API，页面可以在浏览器的本地存储中存储一个或多个键/值对；
2. 然后用键来获取相应的值；

键/值对，就是`key: "pet" `, `key`就是键，`pet`就是对。

#### 获取方法和设置方法(getItem和setItem)

`setItem`方法，用于存储某个数据，只能是string类型的数据项。

    localStorage.setItem("name","luya");
    localStorage.setItem("age",21);
    alert(localStorage.getItem("name"));
    //alert(parseInt(localStorage.getItem("age")));
    //输出：luya

#### 看成一个关联数组

    //为键赋值
    localStorage["name"] = "luya";
    //获取一个键存储的值,相当于getItem方法
    var name = localStorage["name"];

### 另外两个特性

`length`属性，localStorage中有多少数据项；`key`方法，给出localStorage中各个数据项的键。

    for(var i=0;i<localStorage.length;i++){
        var key = localStorage.key(i);
        var value = localStorage[key];
        alert(value);
    }

## 可以怎样使用？

- 在我的新Twitter客户端，为了提高效率，我要用localStorage把Twitter搜索结果缓存起来。用户搜索时，会先搜索本地结果。这对移动用户很有帮助。
- 用户存储播放列表（影片、音乐等）
- 使用sessionStorage实现购物车
- 有一个非常酷的游戏，他可以在两个不同的浏览器窗口同时工作，使用localStorage完成状态同步


## JSON

- JavaScript对象表示法（JavaScript Object Notation）
- JSON是存储和交换文本信息的语法，类似XML；JSON比XML更小、更快、更易解析。

JSON API只有两个方法：`stringify` 和 `parse`

