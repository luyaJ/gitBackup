---
title: JavaScript手册
toc: true
date: 2017-08-05 22:51:43
tags: JavaScript
categories: Web前端
---

JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型。

<!--more-->

## Event对象之事件句柄(Event Handlers)

### onload & onunload事件

是在用户进入或者离开页面的时候被触发。onload事件可用于检测访问者的浏览器类型和浏览器版本;onload和onunload事件可用于处理cookie。

```js
<body onload="checkCookies()">
function checkCookies() {
    if(navigator.cookieEnabled = true){
        alert("Cookies可用");
    } else {
        alert("不可用");
    }
}
</body>
```

### onchange事件

在域的内容改变时发生。常结合对输入字段的验证来使用。

支持该事件的HTML标签：`<input type="text">` , `<select>` , `<textarea>` ;

支持该事件的JavaScript对象：`fileUpload` , `select` , `text` , `textarea`;

```js
    输入你的名字：<input type="text" id="name" onchange="myFunction()">
    <p>当你离开输入框后，函数将被触发，将小写字母转为大写字母。</p>

    function myFunction() {
        var name = document.getElementById("name");
        name.value = name.value.toUpperCase();
    }
```

### onmouseover & onmouseout事件

用于在用户的鼠标移至 HTML 元素上方或移出元素时触发函数。

```js
    <div onmouseover="mOver(this)" onmouseout="mOut(this)" style="background: red;width: 120px;height: 120px;"></div>

    function mOut(obj) {
        obj.innerHTML = "Move Over Me";
    }
    function mOver(obj) {
        obj.innerHTML = "Thank you!";
    }
```

### onmousedown & onmouseup & onclick事件

onmousedown, onmouseup 以及 onclick 构成了鼠标点击事件的所有部分。首先当点击鼠标按钮时，会触发 onmousedown 事件，当释放鼠标按钮时，会触发 onmouseup 事件，最后，当完成鼠标点击时，会触发 onclick 事件。

```js
    <img id="img" onmousedown="lighton()" onmouseup="lightoff()" src="http://www.runoob.com/try/demo_source/bulboff.gif" width="100px" height="180px">
    function lightoff() {
        document.getElementById('img').src="http://www.runoob.com/try/demo_source/bulboff.gif";
    }
```

### button事件

指示当事件被触发时哪个鼠标按键被点击。语法：`event.button = 0|1|2` 。

|  参数  |  描述  |
| :----: | :----: |
| 0 | 规定鼠标左键 |
| 1 | 规定鼠标中键 |
| 2 | 规定鼠标右键 |

```html
    <body onmousedown="whichbutton(event)">
    <p>click in the document!</p>
    <script>
        function whichbutton(event) {
            if(event.button == 2){
                alert("you click the right mouse button!");
            } else {
                alert("you click the left mouse button!");
            }
        }
    </script>
    </body>
```

### clientX & clientY事件

返回当事件被触发时鼠标指针向对于浏览器页面（或客户区：当前窗口）的**水平坐标**或者是**垂直坐标**。

```html
    <body onmousedown="showCoords(event)">
    <p>click in the document!</p>
    <script>
        function showCoords(event) {
            x = event.clientX;
            y = event.clientY;
            alert("X coords: " + x + ",Y coords: " + y);
        }
    </script>
    </body>
```

### screenX & screenY事件

返回事件发生时鼠标指针相对于屏幕的**水平坐标**或者是**垂直坐标**。

## DOM Element对象

### node.appendChild(node)

向节点添加最后一个子节点:

```bash
<ul id="myList"><li>coffee</li><li>tea</li></ul>
<button onclick="myfunction()">点击</button>
<script>
    function myfunction() {
        var node = document.createElement("li");
        var textnode = document.createTextNode("water");
        node.appendChild(textnode);
        document.getElementById("myList").appendChild(node);
    }
</script>
```

从一个列表向另一个列表中移动列表项：

```bash
    <ul id="myList1"><li>coffee</li><li>tea</li></ul>
    <ul id="myList2"><li>water</li><li>milk</li></ul>
    <button onclick="myfunction()">点击</button>
    <script>
        function myfunction() {
            var node = document.getElementById("myList2").firstChild;
            document.getElementById("myList1").appendChild(node);
        }
    </script>
```

### node.attributes

返回指定节点的属性集合，即NamedNodeMap:

```bash
    <button id="btn" onclick="myFunction()" class="btn">点击</button>
    <script>
        function myFunction() {
            var x = document.getElementById("btn").attributes.length;
            document.write(x);
        }
    </script>  //输出结果：3
```

### element.childNodes

返回元素子节点的NodeList。

### node.cloneNode(deep)

创建节点的拷贝，并返回该副本。克隆所有属性以及它们的值。如果您需要克隆所有后代，请把deep参数设置true，否则设置为false。

```bash
    <ul id="myList1"><li>coffee</li><li>tea</li></ul>
    <ul id="myList2"><li>water</li><li>milk</li></ul>
    <button onclick="myFunction()">点击</button>
    <script>
    function myFunction() {
        var clone = document.getElementById("myList2").lastChild.cloneNode(true);
        document.getElementById("myList1").appendChild(clone);
    }
    </script>
```

### node.insertBefore(newnode,existingnode)

在已有子节点之前插入新的子节点:

```bash
    <ul id="myList"><li>coffee</li><li>tea</li></ul>
    <button onclick="myFunction()">点击</button>
    <script>
    function myFunction() {
        var newItem = document.createElement('li');
        var textnode = document.createTextNode('water');
        newItem.appendChild(textnode);

        var list = document.getElementById('myList');
        list.insertBefore(newItem,list.childNodes[0]);
    }
    </script>
```

### node.nextSibling

返回指定节点之后紧跟的节点，在相同的树层级中:

```js
<ul id='myList'><li id='item1'>coffee</li><li id='item2'>tea</li></ul>
x = document.getElementById('item1').nextSibling.id;
document.write(x);  //输出：item2
```

### node.parentNode

以Node对象的形式返回指定节点的父节点:

```js
<ul><li>coffee</li><li>tea</li></ul>
x = document.getElementsByTagName('li')[0];
document.write(x.parentNode.nodeName);  //输出：UL
```

## Array对象

### concat() --连接

**连接**两个或更多的数组，并返回结果。如果要进行`concat()`操作的参数是数组，那么添加的是数组中的元素，而不是数组。

```js
    var color = ["Red","Green","Blue"];
    var name = ["Luya","Awebone"];
    var kei = ["Robin"];
    document.write(color.concat(name,kei));
    //输出：Red,Green,Blue,Luya,Awebone,Robin
```

### shift() & pop() --删除

`shift()`方法用于把数组的**第一个元素**从其中**删除**，并返回第一个元素的值。

```js
    var color = ["Red","Green","Blue"];
    document.write(color+"<br>");  //输出：Red,Green,Blue
    document.write(color.shift()+"<br>");  //输出：Red
    document.write(color);  //输出：Green,Blue
```

`pop()`方法用于删除数组的**最后一个元素**并返回**删除**的元素。

```js
    var color = ["Red","Green","Blue"];
    document.write(color+"<br>");  //输出：Red,Green,Blue
    document.write(color.pop()+"<br>");  //输出：Blue
    document.write(color);  //输出：Red,Green
```

### unshift() & push() --增加

`unshift()`方法可向数组的**开头添加**一个或更多元素，并返回新的长度。

```js
    var color = ["Red","Green","Blue"];
    color.unshift("hello","world");
    document.write(color);  //输出：hello,world,Red,Green,Blue
```

`push()`方法可向数组的末尾添加一个或多个元素，并返回新的长度。

```js
    var color = ["Red","Green","Blue"];
    color.push("hello","world");
    document.write(color);  //输出：Red,Green,Blue,hello,world
```

### splice() --插入,删除,替换

语法：`array.splice(index,howmany,item1,...,itemX)`

`index`必须，规定从何处添加或删除元素；`howmany`必须，规定应该删除多少元素，必须为数字，可以为“0”，若为规定此参数，则删除从index开始到原数组结尾的所有元素；`item`可选，要添加到数组的新元素。

```js
    var color = ["Red","Green","Blue","pink"];
    color.splice(2,0,"luya","awebone");
    document.write(color);  //输出：Red,Green,luya,awebone,Blue,pink
```
```js
    var color = ["Red","Green","Blue","pink"];
    color.splice(2,1,"luya");
    document.write(color);  //输出：Red,Green,luya,pink
```

### reverse() --颠倒

`reverse()`方法用于**颠倒**数组中元素的顺序。

```js
    var color = ["Red","Green","Blue"];
    document.write(color.reverse());
    //输出：Blue,Green,Red
```

### copyWithin() --拷贝

`copyWithin()`方法用于从数组的指定位置**拷贝**元素到数组的另一个指定位置中。

语法：`array.copyWithin(target,start,end)`

`target`必须，从该位置开始替换数据；`start`必须，从该位置开始读取数据，默认为 0 。如果为负值，表示倒数；`end`可选，停止复制的索引位置(默认为array.length)。

```js
    var color = ["Red","Green","Blue","pink"];
    document.write(color.copyWithin(0,1)+"<br>"); //Green,Blue,pink,pink
    document.write(color.copyWithin(1,1)+"<br>"); //Red,Green,Blue,pink
    document.write(color.copyWithin(2,1)+"<br>"); //Red,Green,Green,Blue
    document.write(color.copyWithin(3,1)+"<br>"); //Red,Green,Blue,Green
    document.write(color.copyWithin(2,3)+"<br>"); //Red,Green,pink,pink
    //上边的document要一条一条的输出，一起输出结果会变
```

### sort() --排序

`sort()`方法用于对数组的元素进行**排序**。排序顺序可以是字母或数字，并按升序或降序。默认排序顺序为按字母升序。

***注意：*** 当数字是按字母顺序排列时"40"将排在"5"前面；使用数字排序，你必须通过一个函数作为参数来调用。

```js
    var color = ["Red","Green","Blue","pink"];
    document.write(color.sort());  //输出：Blue,Green,Red,pink
```

数字排序(升序):

```js
    var points = [40,20,1,5,12,23];
    points.sort(function (a,b){ return a-b; });
    document.write(points);  //输出：1,5,12,20,23,40
```

若要实现数字降序则改为：`return b-a` 。

### some() --检测

`some()`方法用于检测数组中的元素是否**满足指定条件**(函数提供)。会依次执行数组的每个元素，如果有一个元素满足条件，就返回`true`,剩下的元素不会在执行检测；如果没有满足条件的元素，则返回`false`。

```js
    var ages = [3,10,18,20];
    function check(age) {
        return age >= 18;
    }
    document.write(ages.some(check));  //输出：true
```

### map() --处理

`map()`方法返回一个新数组，数组中的元素为原始数组元素调用函数**处理**后的值。

***注意：*** `some()`和`map()`都不会对空数组进行检测。

```js
    var num = [1,4,16,100];
    document.write(num.map(Math.sqrt));  //输出：1,2,4,10
```

### join() --转换

`join()`方法用于把数组中的所有元素**转换**一个字符串，元素是通过指定的分隔符进行分隔的。

```js
    var color = ["Red","Green","Blue"];
    document.write(color.join(" and "));  //输出：Red and Green and Blue
```

### fill() --填充

`fill()`方法用于将一个固定值替换数组的元素。

语法：`array.fill(value,start,end)`

```js
    var color = ["Red","Green","Blue"];
    document.write(color.fill("and"));  //输出：and,and,and
```
```js
    var color = ["Red","Green","Blue","pink"];
    document.write(color.fill("and",1,3)); //输出：Red,and,and,pink
```

### filter()

`filter()`方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

```js
    var ages = [32,15,18,7,39];
    function check(age){
        return age >= 18;
    }
    document.write(ages.filter(check));  //输出：32,18,39
```

[菜鸟教程JavaScript Array对象](http://www.runoob.com/jsref/jsref-obj-array.html)

## Date对象

所有浏览器都支持date的各种方法。

```js
    var d = new Date();
    //var d = new Date(year,month);
    document.write(d.getDate());
```

|  方法  |  描述  |  备注  |
| :----: | :----: | :----: |
|getDate()|返回一个月中的某一天 (1~31)|/|
|getDay()|返回一周中的某一天 (0~6)|周日是0，周一是1，依次类推|
|getFullYear()|返回一个表示年份的4位数字|/|
|getMonth()|返回月份(0~11)|0(一月)到11(十二月)|
|getHours()|返回Date对象的小时(0~23)|0表示(午夜)|
|getMinutes()|返回Date对象的分钟(0~59)|/|
|getSeconds()|返回Date对象的秒数(0~59)|/|
|getMilliseconds()|返回Date对象的毫秒(0~999)|/|
|getTime()|返回1970年1月1日至今的毫秒数|/|
|parse()|返回1970年1月1日午夜到指定日期（字符串）的毫秒数|/|
|setDate()|设置一个月的某一天|其他的方法类似，这里就不一一列举|
|toDateString()|把Date对象的日期部分转换为字符串|输出：Fri Aug 04 2017|
|toLocaleDateString()|根据本地时间格式，把Date对象的日期部分转换为字符串|输出：2017/8/4|
|toLocaleTimeString()|根据本地时间格式，把Date对象的时间部分转换为字符串|输出：上午10:59:41|
|toLocaleString()|据本地时间格式，把Date对象转换为字符串|输出：2017/8/4 上午11:00:36|

setDate()方法:

```js
    var d = new Date();
    d.setDate(15);
    document.write(d); //今天真是的是4号，输出：Tue Aug 15 2017 10:54:13 GMT+0800 (中国标准时间)
```

getFullYear()方法：

```js
    var d = new Date();
    document.write(d.getFullYear());  //输出：2017
```

getHours()方法：

```js
    var d = new Date("July 21,2017 21:00:00");
    document.write(d.getHours());  //输出：21
```

[菜鸟教程JavaScipt Date对象](http://www.runoob.com/jsref/jsref-obj-date.html)

## Math对象

### floor(x)

对x进行下舍入，返回小于等于x的最大整数：

```js
    document.write(Math.floor(1.6));  //输出：1
```

### ceil(x)

对x进行上舍入，返回大于等于x的最大整数：

```js
    document.write(Math.ceil(1.4));  //输出：2
```

### round(x)

对x进行四舍五入：

```js
    document.write(Math.round(1.4));  //输出：1
```

### random()

返回介于 0(包含) ~ 1(不包含) 之间的一个随机数：

```bash
    <p id="demo">点击按钮随机显示1到10之间的随机数：</p>
    <button onclick="myFunction()">点击</button>
    <script>
    function myFunction() {
        var demo = document.getElementById("demo");
        demo.innerHTML = Math.floor(Math.random() * 10 + 1);
    }
    </script>
```

### max(x,y,...,n) & min()

max()返回两个指定的数中带有较大的值的那个数:

```js
document.write(Math.max(1,56,4,22,-8));  //输出：56
```

[菜鸟教程JavaScipt Math对象](http://www.runoob.com/jsref/jsref-obj-math.html)


## String对象

### substring() & slice() & substr() --提取

`substring()`用于提取字符串中介于两个指定下标之间的字符:

语法：`stringObject.substring(start,stop)`

```js
var str="Hello world!"
document.write(str.substring(3));  //输出：lo world!
document.write(str.substring(3,7));  //输出：lo w
```

`slice()`可提取字符串的某个部分，并以新的字符串返回被提取的部分:

语法：`stringObject.slice(start,end)`

```js
var str="Hello happy world!"
document.write(str.slice(6));  //输出：happy world!
document.write(str.slice(6,11));  //输出：happy
```

`substr()`可在字符串中抽取从 start 下标开始的指定数目的字符:

语法：`stringObject.substr(start,length)`

```js
var str="Hello world!"
document.write(str.substr(3));  //输出：lo world!
document.write(str.substr(3,7));  //输出：lo worl
```

### indexOf()

返回某个指定的字符串值在字符串中首次出现的位置。对大小写敏感；如果要检索的字符串值没有出现，则该方法返回-1。

```js
var str="Hello world!"
document.write(str.indexOf("Hello") + " ");
document.write(str.indexOf("World") + " ");
document.write(str.indexOf("world"));
//输出： 0 -1 6
```

## RegExp对象

`RegExp`对象表示正则表达式，它是对字符串执行模式匹配的强大工具。

直接量语法：`/pattern/attributes`

### 修饰符

| 修饰符 | 描述 |
| :---: | :--- |
| i | 执行对大小写不敏感的匹配 |
| g | 执行全局匹配(查找所有匹配而非在找到第一个匹配后停止) |
| m | 执行多行匹配 |

```js
var str = "Visit W3School";
var patt1 = /w3school/i;
document.write(str.match(patt1));  //输出：W3School
```
```js
var str="Is this all there is?";
var patt1=/is/gi;
document.write(str.match(patt1));  //输出：Is,is,is
```

### 方括号

* `[abc]`表达式：用于查找方括号之间的任何字符,方括号内的字符可以是任何字符或字符范围。

直接量语法：`/[abc]/`

```js
var str = "Is this all there is?";
var patt1 = /[a-h]/g;
document.write(str.match(patt1));  //输出：h,a,h,e,e
```

* `[^abc]`：用于查找任何不在方括号之间的字符。
* `[0-9]`：查找任何从0至9的数字。
* `[a-z]`：查找任何从小写a到小写z的字符。
* `(red|blue|green)`：查找任何指定的选项。

### 元字符

* `.` : 查找单个字符，除了换行和行结束符。

```js
var str = "That's hot!";
document.write(str.match(/h.t/g));  //输出：hat,hot
```

* `\w` : 查找单词字符。**单词字符包括：a-z、A-Z、0-9，以及下划线。**
```js
var str = 'Give 100%!';
document.write(str.match(/\w/g));  //输出：G,i,v,e,1,0,0
```

* `\W` : 查找非单词字符。（上面的代码输出：`%`）

* `\d` & `\D` : 查找数字/查找非数字字符。

* `\s` & `\S` : 查找空白字符/查找非空白字符。空白字符可以是：

  * 空格符 (space character)
  * 制表符 (tab character)
  * 回车符 (carriage return character)
  * 换行符 (new line character)
  * 垂直换行符 (vertical tab character)
  * 换页符 (form feed character)

* `\b` : 匹配单词边界。在单词边界匹配的位置，单词字符后面或前面不与另一个单词字符直接相邻。请注意，匹配的单词边界并不包含在匹配中。换句话说，匹配的单词边界的长度为零。（不要与`[\b]`混淆。）如果未找到匹配，则返回`null`。

*注意*：`\b`元字符通常用于查找位于单词的开头或结尾的匹配。

***例子：***

1.`/\bm/` 匹配 "moon" 中的 'm',返回`m`；

2.`/oo\b/` 不匹配 "moon" 中的 'oo'，因为 'oo' 后面的 'n' 是一个单词字符，返回`null`；

3.`/oon\b/` 匹配 "moon" 中的 'oon'，因为 'oon' 位于字符串的末端，后面没有单词字符，返回`oon`；

4.`/\w\b\w/` 不匹配任何字符，因为单词字符之后绝不会同时紧跟着非单词字符和单词字符，返回`null`。

* `\B` : 匹配非单词边界。匹配位置的上一个和下一个字符的类型是相同的：即必须同时是单词，或必须同时是非单词字符。字符串的开头和结尾处被视为非单词字符。如果未找到匹配，则返回`null`

*注意*：`\B`元字符通常用于查找不处在单词的开头或结尾的匹配。

* `\n` : 查找换行符。返回换行符被找到的位置。如果未找到匹配，则返回-1。

### 量词

* `n+` : 匹配包含**至少一个**`n`的任何字符串。

```js
var str = 'Hellooo World! Hello W3School!';
document.write(str.match(/o+/g));  //输出：ooo,o,o,oo
```

* `n*` : 匹配包含**零个**或**多个**`n`的任何字符串。

*例：* 对`l`进行全局搜索，包括其后紧跟的一个或多个`o`：

```js
var str = 'Hellooo World! Hello W3School!';
document.write(str.match(/lo*/g));  //输出：l,looo,l,l,lo,l
```

* `n?` : 匹配任何包含**零个**或**一个**`n`的字符串。

*例：* 对`1`进行全局搜索，包括其后紧跟的零个或一个`0`：

```js
var str = '1,100 or 1000';
document.write(str.match(/10?/g));  //输出：1,10,10
```

* `n{x}` : 匹配包含X个`n`的序列的字符串。

*例：* 对包含四位数字序列的子串进行全局搜索：

```js
var str = '100,1000 or 10000';
document.write(str.match(/\d{4}/g));  //输出：1000,1000
```

* `n{x,Y}` : 匹配包含X至Y个`n`的序列的字符串。X和Y必须是数字。

*例：* 对包含三位至四位数字序列的子串进行全局搜索：

```js
var str = '100,1000 or 10000';
document.write(str.match(/\d{3,4}/g));  //输出：100,1000,1000
```

* `n{x,}` : 匹配包含至少X个`n`的序列的字符串。

*例：* 对包含至少三位数字序列的子串进行全局搜索：

```js
var str = '100,1000 or 10000';
document.write(str.match(/\d{3,4}/g));  //输出：100,1000,1000
```

* `n$` : 匹配任何结尾为`n`的字符串。

*例：* 对字符串结尾的 "is" 进行全局搜索：

```js
var str = 'Is this his';
document.write(str.match(/is$/g));  //输出：is
```

* `^n` : 匹配任何开头为`n`的字符串。

*例：* 对字符串开头的 "is" 进行全局搜索：

```js
var str = 'Is this his';
document.write(str.match(/^is/g));  //输出：Is
```

* `?=n` : 匹配任何其后紧接指定字符串`n`的字符串。

*例：* 对其后紧跟 "all" 的 "is" 进行全局搜索：

```js
var str = 'Is this all there is';
patt1 = /is(?=all)/;
document.write(str.match(patt1));  //输出：is
```

* `?!n` : 匹配其后没有紧接指定字符串`n`的任何字符串。

*例：* 对其后没有紧跟 "all" 的 "is" 进行全局搜索：

```js
var str = 'Is this all there is';
patt1 = /is(?! all)/gi;
document.write(str.match(patt1));  //输出：Is,is
```

### 定位符

* `^` : 匹配输入字符串开始的位置。
* `$` : 匹配输入字符串结尾的位置。

### RegExp对象方法

* `compile` : 编译正则表达式。
* `exec` : 检索字符串中指定的值。返回找到的值，并确定其位置。
* `test` : 检索字符串中指定的值。返回`true`或`false`。

### 支持正则表达式的String对象的方法

* `search()`用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
* `match()`在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。
* `replace()`在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。
* `split()`把一个字符串分割成字符串数组。