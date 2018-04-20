---
layout:     post
title:      Cookie$Session
subtitle:   用法
date:       2018-04-20
author:     ZL
header-img: img/20180420.jpg
catalog: 	 true
tags:
    - Cookie
    - Session
---

# 会话技术  
就是存储客服端的状态，会话技术是帮助服务器	记住客户端状态（区分客户端）  
从打开一个浏览器访问某个站点，到关闭这个浏览器的整个过程，成为一次会话。  
会话技术分为Cookie和Session：  
Cookie：数据存储在客户端本地，减少服务器端的存储的压力，安全性不好，客户端	可以清除cookie。  
Session：将数据存储到服务器端，安全性相对好，增加服务器的压力。  

# 二、Cookie技术  
Cookie技术是将用户的数据存储到客户端的技术  

## 服务器端向客户端发送一个Cookie  
1. 创建Cookie：  
Cookie cookie = new Cookie(String cookieName,String cookieValue);  
示例：
Cookie cookie = new Cookie("username"，"zhangsan");  
那么该cookie会以响应头的形式发送给客户端：  
注意：Cookie中不能存储中文

2. 设置Cookie在客户端的持久化时间：  
cookie.setMaxAge(int seconds); ---时间秒  
注意：如果不设置持久化时间，cookie会存储在浏览器的内存中，浏览器关闭	cookie信息销毁（会话级别的cookie），如果设置持久化时间，cookie信息会	被持久化到浏览器的磁盘文件里  
示例：  
cookie.setMaxAge(10*60);  
设置cookie信息在浏览器的磁盘文件中存储的时间是10分钟，过期浏览器	自动删除该cookie信息  

3. 设置Cookie的携带路径：  
cookie.setPath(String path);  
注意：如果不设置携带路径，那么该cookie信息会在访问产生该cookie的	web资源所在的路径都携带cookie信息  
示例：  
cookie.setPath("/WEB16");  
代表访问WEB16应用中的任何资源都携带cookie  
cookie.setPath("/WEB16/cookieServlet");  
代表访问WEB16中的cookieServlet时才携带cookie信息  

4. 向客户端发送cookie：  
response.addCookie(Cookie cookie);  

5. 删除客户端的cookie：  
如果想删除客户端的已经存储的cookie信息，那么就使用同名同路径的持久化时	间为0的cookie进行覆盖即可  


## 服务器端怎么接受客户端携带的Cookie  
cookie信息是以请求头的方式发送到服务器端的：  
![](http://ovoxjpcrm.bkt.clouddn.com/deadaff067cd191414795c2bc84a4574.png)  

1. 通过request获得所有的Cookie：  
Cookie[] cookies = request.getCookies();  
2. 遍历Cookie数组，通过Cookie的名称获得我们想要的Cookie  
    ```java
    for(Cookie cookie : cookies){
      if(cookie.getName().equal(cookieName)){
        String cookieValue = cookie.getValue();
      }
    }
    ```


# Session技术  
Session技术是将数据存储在服务器端的技术，会为每个客户端都创建一块内存空间	存储客户的数据，但客户端需要每次都携带一个标识ID去服务器中寻找属于自己的内	存空间。所以说Session的实现是基于Cookie，Session需要借助于Cookie存储客	户的唯一性标识JSESSIONID  


## 获得Session对象  
HttpSession session = request.getSession();  
此方法会获得专属于当前会话的Session对象，如果服务器端没有该会话的Session	对象会创建一个新的Session返回，如果已经有了属于该会话的Session直接将已有	的Session返回（实质就是根据JSESSIONID判断该客户端是否在服务器上已经存在	session了）  

## 向session中存取数据（session也是一个域对象）
Session也是存储数据的区域对象，所以session对象也具有如下三个方法：  
session.setAttribute(String name,Object obj);  
session.getAttribute(String name);  
session.removeAttribute(String name);  

## Session对象的生命周期  
创建：第一次执行request.getSession()时创建  
销毁：  
- 服务器（非正常）关闭时
- session过期/失效（默认30分钟）

  问题：时间的起算点 从何时开始计算30分钟？
  从不操作服务器端的资源开始计时

  可以在工程的web.xml中进行配置
  ```xml
  <session-config>
          <session-timeout>30</session-timeout>
  </session-config>
  ```
- 手动销毁session  
session.invalidate();


# 重点  
Cookie技术：存到客户端  
发送cookie  
Cookie cookie = new Cookie(name,value)  
cookie.setMaxAge(秒)  
cookie.setPath()  
response.addCookie(cookie)  
获得cookie  
Cookie[] cookies = request.getCookies();  
cookie.getName();  
cookie.getValue();  


Session技术：存到服务器端 借助cookie存储JSESSIONID  
HttpSession session = request.getSession();  

setAttribute(name,value);  
getAttribute(name);  

session生命周期  
创建：第一次指定request.getSession();  
销毁：服务器关闭、session失效/过期、手动session.invalidate();  
session作用范围：默认一会话中  


# 代码示例  

- 发出cookie  
  ```java
  //1、创建cookie对象
  Cookie cookie = new Cookie("name","zhangsan");
  
  //1.1 为cookie设置持久化时间 ---- cookie信息在硬盘上保存的时间
  cookie.setMaxAge(10*60);//10分钟 ---- 时间设置为0代表删除该cookie
  //1.2 为cookie设置携带的路径
  //cookie.setPath("/WEB16/sendCookie");//访问sendCookie资源时才携带这个cookie
  cookie.setPath("/WEB16");//访问WEB16下的任何资源时都携带这个cookie
  //cookie.setPath("/");//访问服务器下的所有的资源都携带这个cookie
  
  //2、将cookie中存储的信息发送到客户端---头
  response.addCookie(cookie);
  ```

- 获取cookie  
  ```java
    //获得客户端携带的cookie的数据
  Cookie[] cookies = request.getCookies();
  //Cookie cookie = new Cookie("name","zhangsan");
  //通过cookie名称获得想要的cookie
  if(cookies!=null){
    for(Cookie cookie : cookies){
      //获得cookie的名称
      String cookieName = cookie.getName();
      if(cookieName.equals("name")){
        //获得该cookie的值
        String cookieValue = cookie.getValue();
        System.out.println(cookieValue);
      }
    }
  }
  ```

- 删除cookie  
  ```java
  //删除客户端保存 name=zhangsan的cookie信息
  Cookie cookie = new Cookie("name","");
  //将path设置成与要删除cookie的path一致
  cookie.setPath("/WEB16");
  //设置时间是0
  cookie.setMaxAge(0);

  response.addCookie(cookie);
  ```

- 设置session  
     
     request.getSession()方法内部会判断 该客户端是否在服务器端已经存在session  
     如果该客户端在此服务器不存在session 那么就会创建一个新的session对象  
     如果该客户端在此服务器已经存在session 获得已经存在的该session返回  
     
    ```java
    //创建属于该客户端(会话)的私有的session区域
    HttpSession session = request.getSession();
    
    session.setAttribute("name", "jerry");
    
    String id = session.getId();//该session对象的编号id
    
    ```

- 从session中获取数据  
  ```java
  //从session中获得存储的数据
  HttpSession session = request.getSession();

  Object attribute =  session.getAttribute("name");

  response.getWriter().write(attribute+"");
  ```
