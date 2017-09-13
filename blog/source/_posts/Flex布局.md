---
title: Flex布局
toc: true
date: 2017-08-17 17:15:15
tags: css
categories: web前端
---

Flex是Flexible Box的缩写，意为“弹性布局”，用来为盒状模型提供最大的灵活性。

<!--more-->

## 容器的属性

![容器图](http://otf35fo4u.bkt.clouddn.com/flex.png)

### flex-direction

该属性决定主轴的方向，即项目排列方向。

语法：`flex-direction: row | row-reverse | column | column-reverse`

`row`为默认值，主轴为水平方向，起点在左端。

### flex-wrap

默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

语法：`flex-wrap: nowrap | wrap | wrap-reverse`

`nowrap`为默认值，不换行；`wrap`换行，第一行在上方；`wrap-reverse`换行，第一行在下面。

### flex-flow

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

### justify-content

该属性定义了项目在主轴上的对齐方式。

语法：`justify-content: flex-start | flex-end | center | space-between | space-around`

![主轴对其方式](http://otf35fo4u.bkt.clouddn.com/justify-content.png)

### align-items

该属性定义项目在交叉轴上如何对齐。

语法：`align-items: flex-start | flex-end | center | baseline | stretch`

![交叉轴对齐方式](http://otf35fo4u.bkt.clouddn.com/align-items.png)

`baseline`项目的第一行文字的基线对齐;`stretch`为默认值，如果项目未设置高度或设为auto，将占满整个容器的高度。

### align-content

`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

语法：`align-content: flex-start | flex-end | center | space-between | space-around | stretch`

![对齐](http://otf35fo4u.bkt.clouddn.com/align-content.png)

## 项目属性

### flex-grow

`flex-grow`属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大

```html
.item{ flex-grow: <number>; /*default 0*/ }
```

### flex-shrink

`flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。(负值对该属性无效)

```html
.item{ flex-shrink: <number>; /*default 1*/ }
```

### flex-basis

`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

```html
.item{ flex-basis: <length>; /*default auto*/ }
```

### flex

`flex`属性是`flex-grow`, `flex-shrink`和`flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

```html
.item{ flex: none | [<'flex-grow'>] <'<flex-shrink>'> || <'flex-basis'> }
```

该属性有两个快捷值：`auto (1 1 auto) `和` none (0 0 auto)`。建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关。

[阮一峰 Flex布局](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
[Flex布局 实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)