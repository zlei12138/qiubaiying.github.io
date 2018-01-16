---
layout:     post
title:      JavaEE学习日记_JavaScript
subtitle:   JavaScript
date:       2018-01-16
author:     ZL
header-img: img/20180116JavaScript.jpg
catalog: 	 true
tags:
    - JavaEE
    - JavaScript
---



# Javascript的作用：
让整个页面具有动态效果

----

# Javascript的组成
![JavaScript的组成](http://ovoxjpcrm.bkt.clouddn.com/c8f0f81013d2ba8c83b0a37c0e7712cd.png)
- ECMAScript：它是整个javascript的核心，包含(基本语法、变量、关键字、保留字、数据类型、语句、函数等等)
- DOM:文档对象模型，包含(整个html页面的内容)
- BOM：浏览器对象模型,包含(整个浏览器相关内容)

# ECMAScript语法
1. 区分大小写
2. 弱类型的

    使用var定义变量，变量声明不是必须的
    如果在函数的内容使用var定义，那么它是一个局部变量，如果没有使用var它是一个全局的。

## 数据类型
原始类型：Undefined、Null、Boolean、Number、String

>对变量或值调用 typeof 运算符将返回下列值之一：
>
>undefined - 如果变量是 Undefined 类型的
>
>boolean - 如果变量是 Boolean 类型的
>
>number - 如果变量是 Number 类型的
>
>string - 如果变量是 String 类型的
>
>object - 如果变量是一种引用类型或 Null 类型的

    undefined 是声明了变量但未对其初始化时赋予该变量的值，null 则用于表示尚未存在的对象。如果函数或方法要返回的是对象，那么找不到该对象时，返回的通常是 null。

## 运算符

其它运算符与java大体一致，需要注意其等性运算符。

== 它在做比较的时候会进行自动转换。

=== 它在做比较的时候不会进行自动转换。

    var sNum = "66";
    var iNum = 66;
    alert(sNum != iNum);	//输出 "false"
    alert(sNum !== iNum);	//输出 "true"


## 获取元素内容
document.getElementById(“id名称”); 获取元素里面的值
document.getElementById(“id名称”).value;

## JavaScript的引入方式
1. 内部引入

    直接将javascript代码写到<script type=”text/javascript”></script>

2. 外部引入

    需要创建一个.js文件，在里面书写javascript代码，然后在html文件中通过script 标签的src属性引入该外部的js文件
![JavaScript外部引入](http://ovoxjpcrm.bkt.clouddn.com/841b0a735f9c63456fea1dce14ef2a2c.png)


# BOM 对象
## window对象
Window 对象表示浏览器中打开的窗口

![window对象的方法](http://ovoxjpcrm.bkt.clouddn.com/36e1d9fa610b4e1238d7f739edcb4fbe.png)

setInterval():它有一个返回值，主要是提供给clearInterval使用。
setTimeout():它有一个返回值，主要是提供给clearTimeout使用。
clearInterval():该方法只能清除由setInterval设置的定时操作
clearTimeout():该方法只能清除由setTimeout设置的定时操作



//确认按钮
//confirm("您确认删除吗？");
![confirm](http://ovoxjpcrm.bkt.clouddn.com/9fd29c92857241643a4178653622aca8.png)

//提示输入框
prompt("请输入价格：");
![prompt](http://ovoxjpcrm.bkt.clouddn.com/1e7a4147ef4f66fc5a58ffc4257863d3.png)

//警告框
//alert("aaa");
![alert](http://ovoxjpcrm.bkt.clouddn.com/d41322d12dc55ceab7ba11dbc850c6ba.png)


## History对象
History 对象包含用户（在浏览器窗口中）访问过的 URL。
History 对象是 window 对象的一部分，可通过 window.history 属性对其进行访问。


![History属性和方法](http://ovoxjpcrm.bkt.clouddn.com/6ff065a64424150cb8db8da33439ad50.png)


使用：
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>History对象</title>
		<script>
			function fanhui(){
				history.go(1);
				//history.back();
			}
		</script>
	</head>
	<body>
		<input type="button" value="返回上一页" onclick="fanhui()"/><br />
		<a href="03_Location对象.html">下一页</a>
	</body>
</html>
```



## Location 对象
Location 对象包含有关当前 URL 的信息。
Location 对象是 Window 对象的一个部分，可通过 window.location 属性来访问。
![Location对象的属性和方法](http://ovoxjpcrm.bkt.clouddn.com/9ce63c1dbd35ef1928b2abd5623cb709.png)

转到新的页面：
![](http://ovoxjpcrm.bkt.clouddn.com/3950f6894af3f208d98326a29d9b5085.png)


# 事件
onclick点击
onSubmit提交（在form标签中，return xxx()）
onFocus焦点
onBlur焦点离开


# 向页面输出
- 弹窗：alert，pormpt，confirm
- 向浏览器中写入内容：document.write("xxx");
- 向页面指定位置写入内容：innerHTML
