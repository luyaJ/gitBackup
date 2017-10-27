---
title: CSS_齐舞
toc: true
date: 2017-09-16 23:07:25
tags: CSS
categories: Web前端
---

参考视频：[齐舞学院](https://t.75team.com/video)

<!--more-->

# 层叠、继承和CSS单位

## 选择器的特异度

| 选择器 | 内联 | id个数 | （伪）类个数 | 标签个数 | 特异度 |
| ----- | :--: | :---: | :--------: | :----: | :---: |
| #nav .list li a:link | 0 | 1 | 2 | 2 | 0122 |
| .hd ul.links a | 0 | 0 | 2 | 2 | 0022 |

可以理解为特异度 `0122` > `0022`，所以第一个的优先级大于第二个的优先级。

再举一个例子：

```js
<article>
    <h1 class="title">选择器的特异度</h1>
</article>
<style>
    .title{ color: blue; }
    article h1{ color:red; }
</style>
```

这里可以理解为`.title`的特异度是`0010`，`article h1`的特异度是`0002`。所以字体颜色是蓝色。

## 简单选择器的特异度级别（由低到高）

* level 0：*
* level 1：标签选择器、伪元素
* level 2：类、伪类、属性
* level 3：id
* level 4：内联

伪元素：`::before`,`::after`,`::first-line`,`::first-letter`。

伪类有：`:first-child`,`:link`,`:visited`,`:hover`,`:active`,`:focus`,`:lang`,`:right`,`:left`,`:first`。

## 属性匹配

```js
<button class="btn">普通按钮</button>
<button class="btn btn-primary">主要按钮</button>
<style>
    .btn{ display: inline-block;border: none;background: #e6e6e6;color: #333; }
    .btn.btn-primary{ color: #fff;background: #218de6; }
</style>
```

这里`.btn.btn-primary`会覆盖掉`.btn`,颜色和背景颜色为`#fff和#218de6;`,所以第二个按钮背景颜色为蓝色，字体颜色为白色。

## important

```js
<button class="btn">普通按钮</button>
<button class="btn btn-primary">主要按钮</button>
<style>
    .btn{ display: inline-block;border: none;background: #e6e6e6;color: #333 !important; }
    .btn.btn-primary{ color: #fff;background: #218de6; }
</style>
```

给`.btn`的`color`属性增加了`!important`,那么该属性不会被改变，所以第二个按钮的背景颜色为蓝色，字体颜色为#333。

如果想要覆盖掉`.btn`的`color`值，那么也给`.btn.btn-primary`中的`color`添加`!important`。

## Cascading（继承）

某些属性会自动继承其父元素的计算值，除非显式指定一个值。

## 长度单位

* 绝对单位
 * px：像素，对应显示器的一个像素点
 * in：英寸
 * 英寸cm：厘米
 * 厘米mm：毫米
 * 毫米pt：磅（1pt等于1/72英寸）
 * 英寸pc：1pc等于12pt

* 相对单位
 * em：相对于该元素的一个font-size
 * rem：相对于html元素的font-size
 * vh：浏览器窗口高度的1%
 * vw：浏览器窗口宽度的1%
 * vmin：vh和vm中的较小值
 * vmax：vh和vm中的较大值

## HSL颜色模型

使用Hue、Saturation、Lightness三个数字来表示颜色。

* Hue：色相（H）是色彩的基本属性，就是平常所说的颜色名称，如红色、黄色等。取值范围是0-360（0代表红色）。
* Saturation：饱和度（S）是指色彩的纯度，越高色彩越纯，低则逐渐变灰。取值范围0-100%。
* Lightness：越高颜色越亮。取值范围是0-100%。
* 例如：hsl(0,50%,50%)、hsla(120,50%,50%,0.5)。

# 盒模型

## box-sizing

语法：`box-sizing: border-box | content-box | inherit`,初始值为`content-box`。

## overflow

语法：`overflow: visible | hidden | scroll | auto`,默认值为`visible`。

* visible：默认值。内容不会被修剪，会呈现在元素框之外。
* hidden：内容会被修剪，并且其余内容是不可见的。
* scroll：内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
* auto：如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。

## visibility

语法：`visible | hidden | collapse`,默认值为`visible`。

即使不可见的元素也会占据页面上的空间。请使用 "display" 属性来创建不占据页面空间的不可见元素。

## Generated Content

```js
<a href="http://example.com">访问链接</a>
<style>
    a::before{
        content: '\2693';
    }
    a::after{
        content: '(' attr(href) ')';
    }
</style>
```

![::before::after](http://ot4r4qnml.bkt.clouddn.com/before.png)

# 排版细节

## vertical-align

* 定义盒子所处的行盒（line box）的垂直对齐关系
* 语法：`vertical-align: baseline | sub | super | top | text-top | middle | bottom | text-bottom | <percentage> | <length>`
* 初始值为`baseline`
* 百分比相对于元素自身的行高

![vertical-align](http://otf35fo4u.bkt.clouddn.com/vertical.png)

## box-shadow

`box-shadow`不再是简单的用来制造阴影效果，还可以利用它来制作小图标的一部分。

# 布局

`float`,`position`,`inline-block`,`table`,`flex`,`grid`

## 水平居中

* 行级元素：`text-align: center;`
* 块级元素：`margin: auto;`

## 垂直居中

* 单行文字：`line-height`
* 行级盒子：`vertical-align: middle;`
* 绝对定位：`top: 50%;left: 50%;`

## 表格

用下面的方式可以做自适应：
```js
display: table                 /* <table> */
display: table-cell            /* <td> */
display: table-row             /* <tr> */
display: table-column          /* <col> */
display: table-column-group    /* <colgroup> */
display: table-footer-group    /* <tfoot> */
display: table-header-group    /* <thead> */
```

举例：
```js
<nav>
    <a href="">home</a>
    <a href="">html</a>
    <a href="">css</a>
    <a href="">javascript</a>
    <a href="">http</a>
</nav>
<style>
    nav{ width: 100%; display: table; border-collapse: collapse;line-height: 3; }
    nav a{ display: table-cell; text-align: center; border: 1px solid #fff; background: #00b3ee;}
</style>
```

# 高级选择器

## :target伪类

可以使用`:target`伪类做tab选项卡功能。

```js
<nav>
    <a href="#p1">html</a>
    <a href="#p2">css</a>
    <a href="#p3">javascript</a>
</nav>
<p id="p1">htmlhtmlhtml</p>
<p id="p2">csscsscss</p>
<p id="p3">javascriptjavascript</p>

<style>
    p{ display: none; }
    p:target{ display: block; }
</style>
```

## :lang伪类

* 元素匹配上指定语言时的状态
* 浏览器通过lang属性获得语言信息







