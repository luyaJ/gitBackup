---
title: 学习Sass
toc: true
date: 2017-07-25 21:03:33
tags: css预处理器
---

CSS 预处理器的主要目标：提供 CSS 缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性。这不是锦上添花，而恰恰是雪中送炭。

<!--more-->

目前主流的三个**预处理器**：

- Less
- Sass
- Stylus

# 安装Sass

[Ruby下载链接http://rubyinstaller.org/downloads](http://rubyinstaller.org/downloads)

安装过程中请注意勾选 `Add Ruby executables to your PATH`

安装完成后运行cmd输入`ruby -v`测试有没有安装成功。

若安装成功，使用`Ruby`自带的`RubyGems`系统安装Sass和Compass

    gem install sass
    gem install compass

这样就安装好了！

如下sass常用更新、查看版本、sass命令帮助等命令：

    gem update sass
    sass -v
    sass -h

# 编译Sass

Sass编译有很多种方式，如命令行编译模式、sublime插件`SASS-Build` 、编译软件`koala` 、前端自动化软件`codekit` 、Grunt打造前端自动化工作流`grunt-sass` 、Gulp打造前端自动化工作流`gulp-ruby-sass`等。

## 命令行编译：

    //单文件转换命令
    sass input.scss output.css
    //单文件监听命令
    sass --watch input.scss:output.css
    //如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
    sass --watch app/sass:public/stylesheets

## 四种编译排版

    //未编译样式
    .box {
        width: 300px;
        height: 400px;
        &-title {
            height: 30px;
            line-height: 30px;
        }
    }

### nested 编译排版格式：

    /*命令行内容*/
    sass style.scss:style.css --style nested

    /*编译过后样式*/
    .box {
        width: 300px;
        height: 400px; }
        .box-title {
            height: 30px;
            line-height: 30px; }

### expanded 编译排版格式

    /*命令行内容*/
    sass style.scss:style.css --style expanded

    /*编译过后样式*/
    .box {
        width: 300px;
        height: 400px;
    }   
    .box-title {
        height: 30px;
        line-height: 30px;
    }

### compact 编译排版格式

    /*命令行内容*/
    sass style.scss:style.css --style compact

    /*编译过后样式*/
    .box { width: 300px; height: 400px; }
    .box-title { height: 30px; line-height: 30px; }

### compressed 编译排版格式

    /*命令行内容*/
    sass style.scss:style.css --style compressed

    /*编译过后样式*/
    .box{width:300px;height:400px}.box-title{height:30px;line-height:30px}

# 在webstrom中使用sass
file ☛ settings  ☞ Tools  ☞ File Watchers  双击配置Sass

# 嵌套CSS规则

sass文件:

    #content {
        article {
            h1 { color: #333 }
            p { margin-bottom: 1.4em }
        }
        aside { background-color: #EEE }
    }

编译成css后：

    #content article h1 { color: #333 }
    #content article p { margin-bottom: 1.4em }
    #content aside { background-color: #EEE }

## 父选择器的标识符&

    article a {
        color: blue;
        &:hover { color: red }
    }

当包含父选择器标识符的嵌套规则被打开时，他不会像后代选择器那样进行拼接，而是 & 被父选择器直接替换：

    article a { color: blue; }
    article a:hover { color: red }

## 子组合选择器和同层组合选择器： > 、 + 和 ~ 
	
    article section { margin: 5px }
    article > section { border: 1px solid #ccc }

第一个选择器会选择article下的**所有**命中section选择器的元素。第二个选择器只会选择article下**紧跟着的子元素**中命中section选择器的元素。

    header + p { font-size: 1.1em }

在上例中，你可以用**同层相邻组合选择器** + 选择header元素后紧跟的p元素。

    article ~ article { border-top: 1px dashed #ccc }

你也可以用**同层全体组合选择器** ~ ，选择所有跟在article后的同层article元素，不管它们之间隔了多少其他元素。

例子：

    article {
        ~ article { border-top: 1px dashed #ccc }
        > section { background: #eee }
        dl > {
        dt { color: #333 }
        dd { color: #555 }
        }
        nav + & { margin-top: 0 }
    }	

sass解开组合之后：

    article ~ article { border-top: 1px dashed #ccc }
    article > footer { background: #eee }
    article dl > dt { color: #333 }
    article dl > dd { color: #555 }
    nav + article { margin-top: 0 }

## 嵌套属性

重复编写`border-style`  `border-width`  `border-color`等非常烦人，所以在sass中，只要像以下这样书写就行：

    nav{
        border: {
        style: solid;
        width: 1px;
        color: #ccc;
        }
    }

***规则：*** 把属性名从中划线-的地方断开，在根属性后边添加一个冒号:，紧跟一个{ }块，把子属性部分写在这个{ }块中。

***优点：*** 属性和选择器嵌套是非常伟大的特性，因为它们不仅大大减少了你的编写量，而且通过视觉上的缩进使你编写的样式结构更加清晰，更易于阅读和开发。
	
# 导入Sass文件

随着你的样式表越来越多，我们要把大量样式分拆到多个文件中，使用 **@import** 规则。

## 使用sass部分文件

举例来说，你想导入 `themes/_night-sky.scss` 这个局部文件里的变量，你只需在样式表中写 `@import "themes/night-sky";`

## 默认变量值;

一般情况下，你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值。举例说明：

    $link-color: blue;
    $link-color: red;
    a {
        color: $link-color;
    }

在上边的例子中，超链接的`color`会被设置为`red`。这可能并不是你想要的结果，假如你写了一个可被他人通过`@import`导入的sass库文件，你可能希望导入者可以定制修改sass库文件中的某些值。使用sass的`!default`标签可以实现这个目的。它很像css属性中`!important`标签的对立面，不同的是`!default`用于变量，含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。

    $fancybox-width: 400px !default;
    .fancybox {
        width: $fancybox-width;
    }

在上例中，如果用户在导入你的sass局部文件之前声明了一个`$fancybox-width`变量，那么你的局部文件中对`$fancybox-width`赋值`400px`的操作就无效。如果用户没有做这样的声明，则`$fancybox-width`将默认为`400px`。

## 静默注释

    body {
        color: #333;  // 这种注释内容不会出现在生成的css文件中
        padding: 0;  /* 这种注释内容会出现在生成的css文件中 */
    }

# 混合器

混合器使用 `@mixin` 标识符定义。

    @mixin rounded-corners {
        -moz-border-radius: 5px;
        -webkit-border-radius: 5px;
            border-radius: 5px;
    }

然后就可以在样式表中通过 `@include` 来使用这个混合器。
	
    notice {
        background-color: green;
        border: 2px solid #00aa00;
        @include rounded-corners;
    }

sass最终生成：
	
    .notice {
        background-color: green;
        border: 2px solid #00aa00;
        -moz-border-radius: 5px;
        -webkit-border-radius: 5px;
        border-radius: 5px;
    }
	