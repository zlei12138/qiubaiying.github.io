---
layout:     post
title:      HttpServletRequest
subtitle:   用法
date:       2018-04-18
author:     ZL
header-img: img/20180418.jpg
catalog: 	 true
tags:
    - HttpServletRequest
---

# HttpServletRequest

每一个doget或者dopost方法里面都会有的参数，其实是浏览器访问时，tomcat将一些信息封装成了一个request参数。  

# request包含的信息  

![](http://ovoxjpcrm.bkt.clouddn.com/2bd563edcb240a8aa935ab93880db4ef.png)

因为request代表请求，所以我们可以通过该对象分别获得Http请求的请求行，请	求头和请求体  

# request的一些api  

## 获取请求行  
- 获得客户端（浏览器）的请求方式：String getMethod()
- 获得请求的资源：
  ```java
  String getRequestURI() 
  StringBuffer getRequestURL() 
  String getContextPath() ---web应用的名称
  String getQueryString() ---- get提交url地址后的参数字符串
  比如username=zhangsan&password=123
  ```

- 获得客户端的一些信息：request.getRemoteAddr() --- 获得访问的客户端IP地址  


## 获取请求头  

```java
long getDateHeader(String name)
String getHeader(String name)
Enumeration getHeaderNames()
Enumeration getHeaders(String name)
int getIntHeader(String name)
```

## 获得请求体  

```java
String getParameter(String name) 
String[] getParameterValues(String name)
Enumeration getParameterNames()
Map<String,String[]> getParameterMap()(结合BeanUtils使用)
```

*在获取参数时中文的乱码问题解决办法：*  
解决post提交方式的乱码：request.setCharacterEncoding("UTF-8");  
解决get提交的方式的乱码：parameter = new String(parameter.getbytes("iso8859-1"),"utf-8");   

## request域  

request对象也是一个存储数据的区域对象，所以也具有如下方法：  
```java
setAttribute(String name, Object o)
getAttribute(String name)
removeAttribute(String name)
```

**注意：request域的作用范围：一次请求中**

ServletContext域与Request域:  
ServletContext：  
	创建：服务器启动  
	销毁：服务器关闭  
	域的作用范围：整个web应用  
request：  
	创建：访问时创建request  
	销毁：响应结束request销毁  
	域的作用范围：一次请求中  


## request完成请求转发  
这是一种类似重定向的功能  
![](http://ovoxjpcrm.bkt.clouddn.com/06c9ad579559cb6e041a0c9f71313dc8.png)  
```java
获得请求转发器----path是转发的地址
RequestDispatcher getRequestDispatcher(String path)
通过转发器对象转发
requestDispathcer.forward(ServletRequest request, ServletResponse response)
```

转发与重定向的区别？  
1. 重定向两次请求，转发一次请求  
2. 重定向地址栏的地址变化，转发地址不变  
3. 重新定向可以访问外部网站 转发只能访问内部资源  
4. 转发的性能要优于重定向  





# 总结（重点内容）

- request获得行的内容  
  - request.getMethod()  
  - request.getRequestURI()  
  - request.getRequestURL()  
  -	request.getContextPath()  
  -	request.getRemoteAddr()  
- request获得头的内容  
	- request.getHeader(name)  
- request获得体（请求参数）  
	- String request.getParameter(name)  
	- Map<String,String[]> request.getParameterMap();(结合BeanUtils使用)  
	- String[] request.getParameterValues(name);  
	注意：客户端发送的参数 到服务器端都是字符串  

- 获得中文乱码的解决：  
	-	post:request.setCharacterEncoding(“UTF-8”);  
	-	get:parameter = new String(parameter.getBytes(“iso8859-1”),”UTF-8”);  

- request转发和域  
	- request.getRequestDispatcher(转发的地址).forward(req,resp);  
	- request.setAttribute(name,value)  
	- request.getAttribute(name)  


# 其他  
- referer：request的一个头，表示执行该此访问的的来源，意思是说，是从哪个页面上跳转到本servlet上来的。  




# 部分代码实例  

- 获得指定的头  
```String header = request.getHeader("User-Agent");```

- 获得所有的头的名称
  ```java
  Enumeration<String> headerNames = request.getHeaderNames();
  while(headerNames.hasMoreElements()){
    String headerName = headerNames.nextElement();
    String headerValue = request.getHeader(headerName);
    System.out.println(headerName+":"+headerValue);
  }
  ```


- referer头判断来源  
  ```java
  //对该新闻的来源的进行判断
  String header = request.getHeader("referer");
  if(header!=null&&header.startsWith("http://localhost")){
    //是从我自己的网站跳转过来的 可以看新闻
    response.setContentType("text/html;charset=UTF-8");
    response.getWriter().write("中国确实已经拿到100块金牌....");
  }else{
    response.getWriter().write("你是盗链者，可耻！！");
  }
  ```

- 获取请求方式、uri、url、ip地址、web应用名称  
  ```java
  //1、获得请求方式
  String method = request.getMethod();
  System.out.println("method:"+method);
  //2、获得请求的资源相关的内容
  String requestURI = request.getRequestURI();
  StringBuffer requestURL = request.getRequestURL();
  System.out.println("uri:"+requestURI);
  System.out.println("url:"+requestURL);
  //获得web应用的名称
  String contextPath = request.getContextPath();
  System.out.println("web应用："+contextPath);
  //地址后的参数的字符串
  String queryString = request.getQueryString();
  System.out.println(queryString);
  //3、获得客户机的信息---获得访问者IP地址
  String remoteAddr = request.getRemoteAddr();
  System.out.println("IP:"+remoteAddr);
  ```

- request.getParameterMap()结合BeanUtils使用  
  ```java
    //使用BeanUtils进行自动映射封装
		//BeanUtils工作原理：将map中的数据 根据key与实体的属性的对应关系封装
		//只要key的名字与实体的属性 的名字一样 就自动封装到实体中
		Map<String, String[]> properties = request.getParameterMap();
		User user = new User();
		try {
			BeanUtils.populate(user, properties);
		} catch (IllegalAccessException | InvocationTargetException e) {
			e.printStackTrace();
		}
    
    
    user是一个bean类，存放数据并且有get，set方法
    ```

- request.getParameterMap()单独使用  
  ```java
  Map<String, String[]> parameterMap = request.getParameterMap();
  for(Map.Entry<String, String[]> entry:parameterMap.entrySet()){
    System.out.println(entry.getKey());
    for(String str:entry.getValue()){
      System.out.println(str);
    }
    System.out.println("---------------------------");
  }
  ```


- 请求转发  
  ```java
  //servlet1 将请求转发给servlet2
  RequestDispatcher dispatcher = request.getRequestDispatcher("/servlet2");
  //执行转发的方法
  dispatcher.forward(request, response);
  ```
