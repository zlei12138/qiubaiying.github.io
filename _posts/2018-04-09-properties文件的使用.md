---
layout:     post
title:      properties文件的使用
subtitle:   存放参数的文件
date:       2018-04-09
author:     ZL
header-img: img/20180409.jpg
catalog: 	 true
tags:
    - Properties
---


**如果有一些参数需要用到，但是又不想写死在代码里面，比如一些url，username，password等。这时候可以使用Properties，将参数放在这里面，需要用到的时候从这里获取即可。**


1. 比如新建一个xx.properties文件  

    >driver=com.mysql.jdbc.Driver  
    url=jdbc:mysql://localhost:3306/test01?useUnicode=true&characterEncoding=utf   
    username=root  
    password=123456  

2. 比如在代码中获取这些参数

    ```java
    /**
     * 静态代码块加载配置文件信息
     */
    static {
      try {
        // 1.通过当前类获取类加载器
        ClassLoader classLoader = JDBCUtils_V3.class.getClassLoader();
        // 2.通过类加载器的方法获得一个输入流
        InputStream is = classLoader.getResourceAsStream("db.properties");
        // 3.创建一个properties对象
        Properties props = new Properties();
        // 4.加载输入流
        props.load(is);
        // 5.获取相关参数的值
        driver = props.getProperty("driver");
        url = props.getProperty("url");
        username = props.getProperty("username");
        password = props.getProperty("password");
      } catch (IOException e) {
        e.printStackTrace();
      }

    }
    ```
