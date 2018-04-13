---
layout:     post
title:      Servlet_HttpServlet_ServletContext
subtitle:   用法
date:       2018-04-13
author:     ZL
header-img: img/20180413.jpg
catalog: 	 true
tags:
    - Servlet
---

# Servlet  
## Servlet简介

Servlet 运行在服务端的Java小程序，是sun公司提供一套规范（接口），用来处理客户端请求、响应给浏览器的动态资源。但servlet的实质就是java代码，通过java的API	动态的向客户端输出内容  

servlet规范：包含三个技术点：  
- servlet技术  
- filter技术---过滤器  
- listener技术---监听器  


## 最简单的例子  

工程目录：主要就是一个java和一个xml文件  
![servlet](http://ovoxjpcrm.bkt.clouddn.com/7e83ee9f205cc523312dc750ed360037.png)

TestServlet.java的代码：  
```java
package aa;
import java.io.IOException;

import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class TestServlet implements Servlet {

	@Override
	public void destroy() {
		System.out.println("destroy");
	}

	@Override
	public ServletConfig getServletConfig() {
		return null;
	}

	@Override
	public String getServletInfo() {
		return null;
	}

	@Override
	public void init(ServletConfig arg0) throws ServletException {
		System.out.println("init");
	}

	@Override
	public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
		System.out.println("service");
		arg1.getWriter().write("xxxxxxxxxxxxxxxxxxxx");
	}

}
```


web.xml的代码：  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>ServletDemo2</display-name>
  //加载欢迎页面的顺序，找到了一个就不会往下找了
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
  
  <servlet>
    //servlet的名字
  	<servlet-name>test</servlet-name>
    //servlet类的全类名
  	<servlet-class>aa.TestServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
    //和所对应的servlet的名字匹配
  	<servlet-name>test</servlet-name>
    //匹配浏览器输入的url
  	<url-pattern>/Testservlet</url-pattern>
  </servlet-mapping>
</web-app>
```


使用：浏览器输入地址：http://localhost:8080/ServletDemo2/Testservlet  
![](http://ovoxjpcrm.bkt.clouddn.com/c60c9a5979d8b478aa09b24929722224.png)


## servlet的配置  
在web.xml里面的就是servlet的配置  

过程：    
1. 根据用户输入的url匹配url-pattern，
2. 然后发现这个url-pattern的名字叫test，找到对应的叫test的servlet
3. 然后发现这个servlet的全类名是aa.TestServlet，然后就可以找到对应的类。  
![](http://ovoxjpcrm.bkt.clouddn.com/787e1d27505cec6146793343c3cb46e6.png)



url-pattern的配置方式：  
- 完全匹配 访问的资源与配置的资源完全相同才能访问到    
```<url-pattern>/Testservlet</url-pattern>```

- 目录匹配 格式：/虚拟的目录../*   *代表任意  
```<url-pattern>/aaa/bbb/*</url-pattern>```

- 扩展名匹配 格式：*.扩展名  
```<url-pattern>*.html</url-pattern>```

  注意：第二种与第三种不要混用 /aaa/bbb/*.html（错误的）



## Servlet的生命周期  

- init(ServletConfig config)  
何时执行：servlet对象创建的时候执行  
ServletConfig ： 代表的是该servlet对象的配置信息

- service（ServletRequest request,ServletResponse response）  
何时执行：每次请求都会执行  
ServletRequest ：代表请求 认为ServletRequest 内部封装的是http请求的信息  
ServletResponse ：代表响应 认为要封装的是响应的信息  

- destroy()  
何时执行：servlet销毁的时候执行  


默认：
*servlet对象创建：第一次访问该serv对象时*  
*servlet对象销毁：服务器关闭的时候*  

eg：对XXXServlet进行了10次访问，init()，destory()，service()	一共执行力多少次？  
----->1,0,10

当在servlet的配置时 加上一个配置 <load-on-startup> servlet对象在服务器启动	时就创建


---


# HttpServlet  
上面是实现servlet的接口，但是实际中不用，而是继承HttpServlet，HttpServlet封装的更好一些。  

```java
package aa;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyHttpServlet extends javax.servlet.http.HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public MyHttpServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```

配置方法并没有改变，还是一样的。



# ServletContext  

## 简介（类似Android中的Context）  

ServletContext代表是一个web应用的环境（上下文）对象，ServletContext对象	内部封装是该web应用的信息，ServletContext对象一个web应用只有一个  

ServletContext对象的生命周期？  
创建：该web应用被加载（服务器启动或发布web应用（前提，服务器启动状		态））
销毁：web应用被卸载（服务器关闭，移除该web应用）  


## 获取ServletContext的对象  
- ServletContext servletContext = config.getServletContext();  
- ServletContext servletContext = this.getServletContext();


## ServletContext的作用  
1. 获得web应用全局的初始化参数  
web.xml中配置初始化参数  
![](http://ovoxjpcrm.bkt.clouddn.com/f2b8e72c2f5e26964dda713322ed9133.png)  
通过context对象获得参数  
![](http://ovoxjpcrm.bkt.clouddn.com/f40d8504927a7faf026556d76e0ea06a.png)  

2. 获得web应用中任何资源的绝对路径  
String path = context.getRealPath(相对于该web应用的相对地址);  

3. ServletContext是一个域对象  
存储数据的区域就是域对象  
ServletContext域对象的作用范围：整个web应（所有的web资源都可以随意向	servletcontext域中存取数据，数据可以共享）  
域对象的通用的方法：  
context.setAtrribute(String name,Object obj);  
context.getAttribute(String name);  
context.removeAttribute(String name);  
数据是所有servlet可以共享的。  
