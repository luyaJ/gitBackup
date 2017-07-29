---
title: jQuery手册
toc: true
date: 2017-07-28 23:09:06
tags: jQuery
---

jQuery是一个JavaScript函数库，极大的简化了JavaScript编程。

<!--more-->

jQuery 版本 2 以上不支持 IE6、7、8 浏览器。如果需要支持 IE6/7/8，那么请选择1.9。你还可以通过条件注释在使用 IE6/7/8 时只包含进1.9。

    <!--[if lt IE 9]>
    <script src="jquery-1.9.0.js"></script>
    <![endif]-->
    <!--[if gte IE 9]><!-->
    <script src="jquery-2.0.0.js"></script>
    <!--<![endif]-->

**添加jQuery：** 

- 官网下载 
- 可以直接从百度CDN中引用jQuery：`src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"`

## 语法

文档就绪事件：

    $(document).ready(function(){
    //代码部分
    });

这是为了防止文档在完全加载之前运行jquery代码。

## 选择器

| 语法 | 描述 |
| :----:  |  :----:  |
| $("p") | 元素选择器 |
| $("#id") | id选择器 |
| $(".class") | class选择器 |

 ![selector](http://otqcthsfv.bkt.clouddn.com/selector.PNG)

###  层级选择器

 ![selector1](http://otqcthsfv.bkt.clouddn.com/selector1.png)

## 事件

### 什么是事件？

页面对不同访问者的响应叫做事件。

### 常用事件方法

##### 1.$(document).ready()

##### 2.click() & dblclick()

##### 3.bind()

将事件和函数绑定到元素上。为被选元素添加一个或多个事件处理程序，并规定事件发生时运行的函数。

    $("button").bind("click",function(){
        $("p").show();
    });

##### 4.on() & off()

在被选元素及子元素上添加一个或多个事件处理程序。使用on()方法添加的事件处理程序适用于*当前及未来的元素*（比如由脚本创建的新元素）。

    $("p").on("click",function(){
        alert("The paragraph was clicked.");
    });

提示：如需移除事件处理程序，使用off()方法

    $("button").click(function(){
        $("p").off("click");
    });

向元素添加多个事件处理程序：

    $("p").on({
        mouseover:function(){$("body").css("background-color","lightgray");},
        mouseout:function(){$("body").css("background-color","lightblue");},
        click:function(){$("body").css("background-color","yellow");}
    });

##### 5.toggle()

用于绑定两个或多个事件处理器函数，以响应被选元素的轮流的click事件。

    <button>请点击这里，来切换不同的背景颜色</button>

    $(document).ready(function(){
        $("button").toggle(function(){
            $("body").css("background","green");}
            ,function(){
            $("body").css("background-color","red");}
            ,function(){
            $("body").css("background-color","yellow");}
        );
    });

##### 6.mousedown() & mouseup()

当鼠标指针移动到元素上方，并按下鼠标按键时，会发生mousedown事件:

    $("button").mousedown(function(){
        $("p").slideToggle();
    });

当在元素上松开鼠标按钮时，会发生mouseup事件

##### 7.focus() & blur()

当元素获得焦点时，发生focus事件;当元素失去焦点时发生blur事件:

    <input type="text" />

    $("input").focus(function(){
        $("input").css("background-color","#FFFFCC");
    });
    $("input").blur(function(){
        $("input").css("background-color","#D6D6FF");
    });

##### 8.hover()

光标悬停事件:

    $("p").hover(function(){
        $("p").css("background-color","yellow");
        },function(){
        $("p").css("background-color","pink");
    });

##### 9.1. event.target

target属性规定哪个DOM元素触发了该事件:

    <h1>这是一个标题</h1>
    <h2>这是另一个标题</h2>
    <p>这是一个段落</p>
    <div></div>

    $("p,h1,h2").click(function(event){
        $("div").html("点击事件由一个 " + event.target.nodeName + " 元素触发");
      });

##### 9.2. event.pageX & event.pageY

pageX()属性是鼠标指针的位置，相对于文档的左边缘;pageY()属性是鼠标指针的位置，相对于文档的上边缘:

    <p>鼠标指针位于：<span></span></p>

    $(document).mousemove(function(e){
        $("span").text("X:" + e.pageX + ",Y:" + e.pageY);
    })

##### 10.trigger()

触发被选元素的指定事件类型:

    <input type="text" name="FirstName" value="Hello World" /> <br/>
    <button>激活input域的select事件</button>

    $("input").select(function(){
        $("input").after("文本被选中！");
      });
      $("button").click(function(){
        $("input").trigger("select");
      });

使用Event对象来触发事件：

    $("input").select(function(){
        $("input").after("文本被选中！");
    });
    var e = jQuery.Event("select");
    $("button").click(function(){
        $("input").trigger(e);
    });

## 效果

### 动画

* animate()

语法：`animate(styles,speed,easing,callback);`

    $("button").click(function(){
        $("div").animate({left:'250px'});
    });

参数`styles`中的css样式值 见：[链接](http://www.w3school.com.cn/jquery/effect_animate.asp)

注释：使用 "+=" 或 "-=" 来创建相对动画(relative animations)

### 隐藏/显示

* hide(speed,callback);
* show(speed,callback);
* toggle(speed,callback);

`speed`参数可以取值：`slow`,`fast`,`normal`,毫秒;`callback`参数是隐藏或显示完成后所执行的函数名称。

    <p>This is a paragraph.</p>
    <button class="btn1">Toggle</button>

    $(".btn1").click(function(){
        $("p").toggle();
    });

### 淡入淡出

* fadeIn(speed,callback);
* fadeOut(speed,callback);
* fadeToggle(speed,callback);
* fadeTo(speed,opacity,callback);

`speed`参数可以取值：`slow`,`fast`,`normal`,毫秒;`opacity`必需，规定淡入或淡出的透明度，介于0.00与1.00;`callback`参数是隐藏或显示完成后所执行的函数名称。

    $(".btn1").click(function(){
      $("p").fadeTo(1000,0.4);
    });

### 滑动

* slideDown(speed,callback);
* slideUp(speed,callback);
* slideToggle(speed,callback);

`speed`参数可以取值：`slow`,`fast`,`normal`,毫秒;`callback`参数是隐藏或显示完成后所执行的函数名称。

    $(".btn1").click(function(){
        $("p").slideToggle();
    });

### stop()

停止当前正在运行的动画:

    $("#stop").click(function(){
        $("#box").stop();
    });

### 链(Chaining)

    $("#p1").css("color","red").slideUp(2000).slideDown(2000);

## jQuery HTML

### 获得内容 -- text()、html()、val()

    <p>名称: <input type="text" id="test" value="菜鸟教程"></p>
    <button>显示值</button>

    $(document).ready(function(){
    $("button").click(function(){
        alert("值为: " + $("#test").val());
        });
    });

* text() -  设置或返回所选元素的文本内容
* html() -  设置或返回所选元素的内容（包括 HTML 标记）
* val() - 设置或返回表单字段的值

### 设置或返回属性 -- attr()

1.返回属性：

    <p><a href="http://www.runoob.com" id="runoob">菜鸟教程</a></p>
    <button>显示 href 属性的值</button>

    $(document).ready(function(){
        $("button").click(function(){
            alert($("#runoob").attr("href"));
         });
    });

2.设置属性：

    $("button").click(function(){
        $("img").attr("width","180");
    });

3.设置多个属性：

    $("img").attr({width:"50",height:"80"});

### 添加元素

##### append() & prepend()

`append`是在被选元素的结尾(仍然在内部)插入内容;`prepend`是在被选元素的开头(仍然在内部)插入内容

    $("p").append(" <b>Hello world!</b>");

##### after() & before()

`after`在被选元素的之后插入内容;`before`在被选元素之前插入内容

    $("p").before("<p>Hello world!</p>");

### 删除元素

##### remove()

删除被选元素及其子元素,该方法不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。

##### empty()

从被选元素移除所有内容，包括所有文本和子节点:

    <div id="div1" style="height:100px;width:300px;border:1px solid black;background-color:yellow;">这是 div 中的一些文本。
    <p>这是在div中的一个段落。</p>
    <p>这是在div中的另外一个段落。</p>
    </div>
    <button>清空div元素</button>

    $(document).ready(function(){
        $("button").click(function(){
            $("#div1").empty();
        });
    });

### clone()

生成被选元素的副本，包含子节点、文本和属性:

    <p>This is a paragraph.</p>
    <button>复制每个 p 元素，然后追加到 body 元素</button>

    $("button").click(function(){
        $("body").append($("p").clone());
    });

### addClass() & removeClass() & toggleClass()

    <div class="div1">这是一些文本。</div>
    <button>为第一个div元素添加类</button>

    .blue{ color:blue; }
    $(document).ready(function(){
        $("button").click(function(){
            $("#div1").addClass("blue");
        });
    });

### CSS()方法

设置css属性：

    $("p").css("background-color","yellow");
    $("p").css("background-color","yellow","font-size","200%");

## 遍历

### 祖先 (向上遍历)

1.parent() 返回被选元素的直接父元素，只会向上一级对DOM树进行遍历。

2.parsents() 从父元素开始，返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。返回包含零个、一个元素或多个元素的jquery对象。

3.closest() 从当前元素开始，获得匹配选择器的第一个祖先元素，从当前元素开始沿DOM树向上。返回包含零个或一个元素的jquery对象。（和.parents有点相似）

    $('li.item-a').closest('ul').css('background-color', 'red'); //整个ul中的li元素颜色都会变红

4.parsentsUntil() 返回介于两个给定元素之间的所有祖先元素。



### 后代 (向下遍历)

1.children() 返回被选元素的所有直接子元素，该方法只会向**下一级**对DOM树进行遍历。

    $("div").children(".selected").css("color","blue");

2.find() 返回被选元素的后代元素，一路向下直到最后一个后代。

    `$('li.item-ii').find('li').css('background-color','red');`

也可以使用给定的jquery集合或元素来进行筛选：

    var $allListElements = $('li');
    $('li.item-ii').find($allListElements).css('background-color','red');

3.contents() 获得匹配元素集合中每个元素的子节点，包括文本和注释节点。

    <p>Hello<a href="#">Apple</a>,how are you doing?</p>

    <script>
        $("p").contents().filter(function(){ return this.nodeType != 1; }).wrap("<b/>");
    </script>

### 同胞 (水平遍历)

1.siblings() 返回被选元素的所有同胞元素:

    $("p").siblings(".selected").css("background", "yellow"); //查找每个 p 元素的所有类名为 "selected" 的所有同胞元素：

2.next() 返回被选元素的下一个同胞元素

### 过滤 (缩小搜索元素的范围)

1.first() 返回被选元素的首个元素。

2.last() 返回被选元素的最后一个元素。

3.eq() 返回被选元素中带有指定索引号的元素。

    .blue{ background-color:blue; }

    $("body").find("div").eq(2).addClass("blue");

如果提供负数，则指示从集合结尾开始的位置，而不是从开头开始。

4.filter() 允许您规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回。

    <div></div>
    <div class="middle"></div>
    <div class="middle"></div>
    <div></div>

    <script>
      $("div").css("background", "#c8ebcc")
        .filter(".middle")
        .css("border-color", "red");
    </script>

![filter](http://otqcthsfv.bkt.clouddn.com/filter.png)

5.not() 和filter()方法相反。

### others

1.add() 将元素添加到匹配元素的集合中:

    $("div").css("border", "2px solid red")
        .add("p")
        .css("background", "yellow");

2.each() 对jQuery对象进行迭代，为每个匹配元素执行函数。

    $("button).click(function(){
        $("li").each(function(){
            alert($(this).text())        
        });
    });

3.end() 结束当前链条中的最近的筛选操作，并将匹配元素集还原为之前的状态。

    <ul class="first">
       <li class="foo">list item 1</li>
       <li>list item 2</li>
       <li class="bar">list item 3</li>
    </ul>

    $('ul.first').find('.foo').css('background-color', 'red')
      .end().find('.bar').css('background-color', 'green');

**说明**: 这条命令链检索第一个列表中类名为foo的项目，并把它们的背景设置为红色。end()会将对象还原为调用find()之前的状态，所以第二个find()查找的是`<ul class="first">`内的`.bar'`，而不是在列表的`<li class="foo">`中查找，并将匹配元素的背景设置为绿色。最后的结果是第一个列表中的项目1和项目3被设置了带颜色的背景，而第二个列表中的项目没有任何变化。

## DOM元素方法

### index()

返回指定元素相对于其他指定元素的index位置：

    <ul>
        <li>Coffee</li>
        <li>Milk</li>
        <li>Soda</li>
    </ul>

    $("li").click(function(){
        alert($(this).index());
    });

### toArray()

以数组的形式返回jQuery选择器匹配的元素:

    x = $("li").toArray()
      for(i=0;i<x.length;i++){
        alert(x[i].innerHTML);
      }

### size()

返回被jQuery选择器匹配的元素的数量:

    $(document).ready(function(){
        alert($("li").size());
    });  //输出结果：3

### get()

获得由选择器指定的DOM元素:

    <p>This is a paragraph</p>
    <button>获得p DOM元素</button>
    <div></div>

    $("button").click(function(){
        x = $("p").get(0);
        $("div").text(x.nodeName + ": " + x.innerHTML);
    });  //输出结果：P: This is a paragraph

## AJAX！！

AJAX 是与服务器交换数据的技术，通过后台加载数据，它在不重载全部页面的情况下，实现了对部分网页的更新。

### 什么是AJAX?

AJAX = 异步JavaScript和XML (Asynchronous JavaScript and XML)

### AJAX load()方法

语法：`$(selector).load(URL,data,callback);`

    <div id="div1">使用jquery AJAX修改文本内容</div>
    <button>获取外部内容</button>

    $(document).ready(function(){
        $("button").click(function(){
            $("#div1").load("/try/ajax/demo_test.txt");
        });  //此处的url是我引用过来的
    });


