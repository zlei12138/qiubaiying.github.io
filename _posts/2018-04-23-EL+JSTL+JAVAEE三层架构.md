---
layout:     post
title:      EL+JSTL+JAVAEE三层架构
subtitle:   用法
date:       2018-04-23
author:     ZL
header-img: img/20180423.jpg
catalog: 	 true
tags:
    - EL
    - JSTL
    - JAVAEE三层架构
    - MVC
---

# EL 
## 介绍：  
在jsp中可以使用java代码，但是还是比较麻烦的，EL就是简化了一些Java代码的编写。  
EL（Express Lanuage）表达式可以嵌入在jsp页面内部，减少jsp脚本的编写，EL	出现的目的是要替代jsp页面中脚本的编写。

## 使用EL获取域中的数据：  
jsp脚本：<%=request.getAttribute(name)%>  
EL表达式替代上面的脚本：${requestScope.name}  

EL最主要的作用是获得四大域中的数据，格式${EL表达式}  
- EL获得pageContext域中的值：${pageScope.key};  
- EL获得request域中的值：${requestScope.key};  
- EL获得session域中的值：${sessionScope.key};  
- EL获得application域中的值：${applicationScope.key};  
- EL从四个域中获得某个值${key};  
同样是依次从pageContext域，request域，session域，application域中	获取属性，在某个域中获取后将不在向后寻找

比如在域中有一些数据，用jsp脚本代码获取如下： 
```jsp
<!-- 脚本方式取出域中的值 -->
<%=request.getAttribute("company") %>
<%
  User sessionUser = (User)session.getAttribute("user");
  out.write(sessionUser.getName());
%>
```

用EL表达式获取域中数据如下：  
```jsp
<!-- 使用EL表达式获得域中的值 -->
${requestScope.company }
${sessionScope.user.name }
${applicationScope.list[1].name}//获取一个list<User>

<!-- 使用el表达式 全域查找 ,依次从pageContext域，request域，session域，application域中	获取属性，在某个域中获取后将不在向后寻找-->
${company }
${user.name }
${list[1].name}
```

还可以使用el获取表单数据，用法还是一样。  
比如：  
```jsp
<%
  request.getParameter("username");
%>

<!-- 使用el获得参数 -->
${param.username }
```

## EL内置对象  
 
- pageScope,requestScope,sessionScope,applicationScope   获取JSP中域中的数据  
- param,paramValues 	- 接收参数.  
相当于request.getParameter()  rrquest.getParameterValues()
- header,headerValues	 - 获取请求头信息  
相当于request.getHeader(name)
- initParam				- 获取全局初始化参数  
相当于this.getServletContext().getInitParameter(name)
- cookie	 				- WEB开发中cookie  
相当于request.getCookies()---cookie.getName()---cookie.getValue()
- pageContext     - WEB开发中的pageContext  
${pageContext.request.contextPath}获取web应用的名称。  

比如： 
```jsp
<!-- 使用el获得参数 -->
${param.username }
${header["User-Agent"] }
${initParam.aaa }
${cookie.name.value }

<!-- 通过el表达式获得request对象 -->
${pageContext.request }

```

## EL执行表达式  
```jsp

<!-- el可以执行表达式运算 -->
${1+1 }
${1==1?true:false }
<!-- empty 判定某个对象是否是null  是null返回true -->
${empty list}
```

---

# JSTL  
## 说明  
JSTL配合EL使用可以更加简化。  
JSTL（JSP Standard Tag Library)，JSP标准标签库，可以嵌入在jsp页面中使用标签的形式完成业务逻辑等功能。jstl出现的目的同el一样也是要代替jsp页面中的脚本代码。JSTL标准标准标签库有5个子库，但随着发展，目前常使用的是他的核心库

| 标签库    | 标签库的URI                            | 前缀 |
| --------- | -------------------------------------- | ---- |
| *Core*    | *http://java.sun.com/jsp/jstl/core*    | *c*  |
| I18N      | http://java.sun.com/jsp/jstl/fmt       | fmt  |
| SQL       | http://java.sun.com/jsp/jstl/sql       | sql  |
| XML       | http://java.sun.com/jsp/jstl/xml       | x    |
| Functions | http://java.sun.com/jsp/jstl/functions | fn   |
基本上只有表中第一个用的比较多。  

## 导入
- 导包  
![](http://ovoxjpcrm.bkt.clouddn.com/84e3610e64aef72811779c39613a67a9.png)  
新版本的可能只要导入一个包  

- 导入  
  ```jsp
  <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
  ```

## 使用  
- <c:if test=””>标签
其中test是返回boolean的条件
  ```jsp
  <%
    if (count == 9){
      out.write("xxxx");
    }
  &>

  ==========等价于=====================

  <!-- jstl标签经常会和el配合使用 -->
  <!-- test代表的返回boolean的表达式 -->
  <c:if test="${count==9 }">
    xxxx
  </c:if>
  ```
- <c:forEach>标签
  ```jsp
  for(int i=0;i<=5;i++){
  			syso(i)
  }
  

	<c:forEach begin="0" end="5" var="i">
		${i }<br/>
	</c:forEach>
  
  
  
  <!-- 模拟增强for    productList---List<Product>
  for(Product product : productList){
    syso(product.getPname());
  }
  
  <!-- items:一个集合或数组   var:代表集合中的某一个元素-->
  <c:forEach items="${productList }" var="pro">
  ${pro.pname }
  </c:forEach>
  ```


## JSTL配合EL取更复制的数据  
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.*" %>
<%@ page import="com.itheima.domain.*" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
		//模拟List<String> strList
		List<String> strList = new ArrayList<String>();
		strList.add("itcast");
		strList.add("itheima");
		strList.add("boxuegu");
		strList.add("shandingyu");
		request.setAttribute("strList", strList);
		
		//遍历List<User>的值
		List<User> userList = new ArrayList<User>();
		User user1 = new User();
		user1.setId(2);
		user1.setName("lisi");
		user1.setPassword("123");
		userList.add(user1);
		User user2 = new User();
		user2.setId(3);
		user2.setName("wangwu");
		user2.setPassword("123");
		userList.add(user2);
		application.setAttribute("userList", userList);
		
		//遍历Map<String,String>的值
		Map<String,String> strMap = new HashMap<String,String>();
		strMap.put("name", "lucy");
		strMap.put("age", "18");
		strMap.put("addr", "西三旗");
		strMap.put("email", "licy@itcast.cn");
		session.setAttribute("strMap", strMap);
		
		//遍历Map<String,User>的值
		Map<String,User> userMap = new HashMap<String,User>();
		userMap.put("user1", user1);
		userMap.put("user2", user2);
		request.setAttribute("userMap", userMap);
	%>
	
	
	
	<h1>取出strList的数据</h1>
	<c:forEach items="${strList }" var="str">
		${str }<br/>
	</c:forEach>
	
	<h1>取出userList的数据</h1>
	<c:forEach items="${userList}" var="user">
		user的name：${user.name }------user的password：${user.password }<br/>
	</c:forEach>
	
	<h1>取出strMap的数据</h1>
	<c:forEach items="${strMap }" var="entry">
		${entry.key }====${entry.value }<br/>
	</c:forEach>
	
	<h1>取出userMap的数据</h1>
	<c:forEach items="${userMap }" var="entry">
		${entry.key }:${entry.value.name }--${entry.value.password }<br/>
	</c:forEach>
	
</body>
</html>
```



# JAVAEE开发模式  
## MVC：---- web开发的设计模式  
M：Model---模型 javaBean：封装数据  
V：View-----视图 jsp：单纯进行页面的显示  
C：Controller----控制器 Servelt：获取数据--对数据进行封装--传递数据--	指派显示的jsp页面  

## javaEE的三层架构
服务器开发时 分为三层  
web层：与客户端交互  
service层：复杂业务处理  
dao层：与数据库进行交互  
开发实践时 三层架构通过包结构体现  

![](http://ovoxjpcrm.bkt.clouddn.com/b8e47c27944aa61f7b95515da829c02e.png)  
![](http://ovoxjpcrm.bkt.clouddn.com/788a1aff5bf373a7ed0998bc2c277deb.png)  
