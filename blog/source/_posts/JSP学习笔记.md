---
title: JSP学习笔记
toc: true
date: 2017-07-23 22:19:34
tags: JavaWeb
---

JSP -- Java Server Pages

<!--more-->

### 区别jsp和servlet

1.jsp就是在html里面写java代码，servlet就是在java里面写html代码。

2.servlet在java代码中通过HttpServletResponse对象动态输出HTML内容；jsp在静态HTML内容中嵌入Java代码，Java代码被动态执行后生成HTML内容。

3.两者通过MVC双剑合璧：MVC(Model-View-Controller)

* Controller(控制器) -负责转发请求，对请求进行处理
* View(视图) -负责界面显示
* Model(模型) -业务功能编写（例如算法实现）、数据库设计以及数据存取操作实现

### 工具
myeclipse,tomcat

### 使用myeclipse
1.new -- web project;

2.输入project name (imoocjsp);

3.打开imoocjsp 下的 Webroot 文件，双击index.jsp。删除` pageEncoding="ISO-8859-1"`之后 使用 `alt+/` 选择`contentType`,将其中charset值改为`charset=utf-8`;(解决中文编码问题)

4.点击图中红色区域内图标，选择Project -- imoocjsp -- add -- Server -- tomcat7 --ok;

![](http://ot4r4qnml.bkt.clouddn.com/eclipse1.PNG)

5.启动Tomcat服务器，在地址栏输入：localhost:8080/imoocjsp/index.jsp

![](http://ot4r4qnml.bkt.clouddn.com/eclipse2.PNG)

![](http://ot4r4qnml.bkt.clouddn.com/eclipse3.PNG)

### JSP指令

用来设置与整个JSP页面相关的属性。
语法：` <%@ directive attribute="value" %> `

![](http://ot4r4qnml.bkt.clouddn.com/instruct.PNG)

* include
 * 指令: `<%@ include file="url" %>`
 * 动作：`<jsp:include page="url" flush="true|false" />` 
* page(要包含的页面)；
* flush(被包含的页面是否从缓冲区读取)


### JSP注释

* HTML注释： <!-- html注释 -->  //客户端可见
* JSP注释： <%-- html注释 --%>  //客户端不可见
* JSP脚本注释： //单行注释  / ** /多行注释

### JSP脚本

写在`<% %>` 中的叫做JSP脚本。

	<%
	out.println("大家好，JSP脚本");
	%>

### JSP声明

在JSP页面中定义变量或者方法。
语法：`<%! JAVA代码 %>`

	<%!
	String s="张三"; //声明了一个字符串变量
	int add(int a,int b){  //声明了一个返回整型函数，实现两个整数的求和
		return a+b;
	}
	%>

### JSP表达式

语法：` <%=表达式 %>  `
注意：表达式不以分号结束

### 做一个九九乘法表

	<%!
	String printMultiTable() {
	String s="";
	for(int i=1;i<=9;i++) {
	for(int j=1;j<=i;j++) {
		s+=i+"*"+j+"="+(i*j)+"&nbsp;&nbsp;&nbsp;&nbsp;";
		}
		s+="<br>";
	}
	return s;
	}
	%>
	<h1>九九乘法表</h1><br>
	<%=printMultiTable() %>

### JSP内置对象

#### out对象

* 什么事缓冲区？

Buffer,所谓缓冲区就是内存的一块区域来保存临时数据。

![](http://ot4r4qnml.bkt.clouddn.com/out%E5%AF%B9%E8%B1%A1.PNG)

	<%
	out.println("<h2>静夜思</h2>");
	out.println("床前明月光<br>");
	out.clear();
	out.println("疑是地上霜<br>");
	%>
	缓冲区大小：<%=out.getBufferSize() %>Byte<br>
	缓冲区剩余大小：<%=out.getRemaining() %><br>
	缓冲区满时：<%=out.isAutoFlush() %><br>

#### get/post

method="get/post"

* `get`: 提交的数据最多不超过2KB，安全性较低但效率比post高。适合提交数据量不大。安全性不高的数据。比如：搜索、查询等功能。
* `post`: 将用户提交的信息封装在HTML HEADER内。适合提交数据量大，安全性高的用户信息。比如：注册、修改、上传等功能。

	<form action="dologin.jsp" method="post">
		用户名:<input type="text" name="username"><br>
		密码:<input type="password" name="password"><br>
		<input type="submit" value="登录">
	</form>

#### request/response

![](http://ot4r4qnml.bkt.clouddn.com/request.PNG)

	//login.jsp:
	<form action="request.jsp" method="post">
    	用户名:<input type="text" name="username"><br>
    	爱好:<input type="checkbox" name="favorite" value="read">读书
    	<input type="checkbox" name="favorite" value="music">音乐
    	<input type="checkbox" name="favorite" value="movie">电影
    	<input type="checkbox" name="favorite" value="internet">上网
    	<input type="submit" value="提交">
	</form>
	<a href="request.jsp?username=lisi">测试URL传参数</a>

	//request.jsp:
	<%
	request.setCharacterEncoding("utf-8"); //解决中文乱码问题
	%>
	用户：<%=request.getParameter("username") %><br/>
	爱好：<%
		if(request.getParameterValues("favorite")!=null){
			String[] favorites=request.getParameterValues("favorite");
			for(int i=0;i<favorites.length;i++){
				out.println(favorites[i]+"&nbsp;&nbsp;");
			 }
		}
		%>
	%>

#### session
#### application
#### page对象

### JavaBean

javabean是一种规范，而不是一种技术或工具。

![](http://ot4r4qnml.bkt.clouddn.com/javabean.PNG)

#### 创建JavaBean

1. 新建一个Web Project --命名：JavaBeanDemo1 ；
2. 新建一个package --命名：com.po ；
3. 新建一个class --命名：Users ；
4. ![](http://ot4r4qnml.bkt.clouddn.com/JavaBeanDemo1-1.PNG)
5. 右键 -- `source` -- `Generate getters and setters` -- `Select All` --ok ；
6. 双击index.jsp -- 使用page指令 `<%@ page import="com.po.Users" %>`
7. 
	`<%
	Users user = new Users();
	user.setUsername("admin"); //设置用户名
	user.setPassword("123456"); //设置密码
	%>
	用户名：<%=user.getUsername() %><br>
	密码：<%=user.getPassword() %><br>`

#### JavaBean动作元素
* `<jsp:useBean id="myUsers" class="com.po.Users" scope="page" />`
* `<jsp:setProperty name="myUsers" property=" * "/>`
* `<jsp:getProperty name="myUsers" property=" * "/>`

