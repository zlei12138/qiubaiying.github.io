---
layout:     post
title:      JavaEE学习日记_JQuery
subtitle:   JQuery
date:       2018-01-21
author:     ZL
header-img: img/20180121.jpg
catalog: 	 true
tags:
    - JQuery
    - JavaEE
---

# JQuery简介
Jquery它是javascript的一个轻量级框架，对javascript进行封装，它提供了很多方便的选择器。供你快速定位到需要操作的元素上面去。还提供了很多便捷的方法。

# JQ与JS的区别

```javascript
			window.onload= function(){
				alert("张三");
			}
			
			//整个文档加载完毕后(包括图片)执行
			window.onload= function(){
				alert("老王");
			}
			
			//dom树绘制完毕后执行,可能DOM元素关联的东西并没有加载完
			jQuery(document).ready(function(){
				alert("李四");
			});
			
			//jquery的简写方法(页面加载)
			$(function(){
				alert("王五");
			});
			
```

# JQ的获取

- 传统方式：

```javascript
//getElementById("btn")
$(function () {
   window.document.getElementById("btn").onclick = function () {
       alert("111");
   }
});
```

- JQ

```javascript
//$("#btn")
jQuery(function () {
    jQuery("#btn").click(function () {
        alert("222");
    });
})
```

# JQ无法操作JS的属性和方法，相互转换

例子：innerHTML属于JS，JQ对象无法操作

```javascript
//点击button，改变span的内容---传统方式
$(function () {
    $("#btn").click(function () {
        window.document.getElementById("span").innerHTML="67890";
    })
})


//Jq方式
$(function () {
    $("#btn").click(function () {
      
      //这里无法使用innerHTML
      //  $("span").innerHTML="67890";

      //html()方法可以替代
       $("#span").html("67890");
    })
})




ps：DOM对象也无法使用JQ的属性和方法
//无法使用
window.document.getElementById("span").html("67890");
```

*JQ非要使用JS的属性和方法*
方式一：

```javascript
 $("span").get(0).innerHTML="99999";
 ```
 
方式二：

```javascript
 $("span")[0].innerHTML="988888";
 ```



*DOM对象转换位JQ对象*

将DOM对象用$()包裹一下就好了

```javascript
var spanEle = window.document.getElementById("span");

//转换
$(spanEle).html("77777");
```


# jQuery的效果

![Jquery的效果](http://ovoxjpcrm.bkt.clouddn.com/d3b7158d14d264cc2769b17fd2984fbf.png)

ps：toggle函数
>显示或者隐藏某个控件


# Jquery 选择器
id选择器：$("#id名称");
元素选择器：$("元素名称");
类选择器：$(".类名");
通配符：$("*");

例子：

```javascript
<html>
	<head>
		<meta charset="UTF-8">
		<title>基本选择器</title>
		<link rel="stylesheet" href="../../css/style.css" type="text/css"/>
		<script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
		<script>
			$(function(){
				$("#btn1").click(function(){
					$("#one").css("background-color","pink");
				});
				$("#btn2").click(function(){
					$(".mini").css("background-color","pink");
				});
				$("#btn3").click(function(){
					$("div").css("background-color","pink");
				});
				$("#btn4").click(function(){
					$("*").css("background-color","pink");
				});
				$("#btn5").click(function(){
					$("#two .mini").css("background-color","pink");
				});
			});
		</script>		
	</head>
	<body>
		<input type="button" id="btn1" value="选择为one的元素"/>
		<input type="button" id="btn2" value="选择样式为mini的元素"/>
		<input type="button" id="btn3" value="选择所有的div元素"/>
		<input type="button" id="btn4" value="选择所有元素"/>
		<input type="button" id="btn5" value="选择id为two并且样式为mini的元素"/>
		<hr/>
		<div id="one">
			<div class="mini">
				111
			</div>
		</div>
		
		<div id="two">
			<div class="mini">
				222
			</div>
			<div class="mini">
				333
			</div>
		</div>
		
		<div id="three">
			<div class="mini">
				444
			</div>
			<div class="mini">
				555
			</div>
			<div class="mini">
				666
			</div>
		</div>
		
		<span id="four">
			
		</span>
	</body>
</html>
```

# 层级选择器

![层级选择器](http://ovoxjpcrm.bkt.clouddn.com/a27992a6ca2dde24f956754106af3606.png)

ancestor descendant: 在给定的祖先元素下匹配所有的后代元素(儿子、孙子、重孙子)  
parent > child : 在给定的父元素下匹配所有的子元素(儿子)  
prev + next: 匹配所有紧接在 prev 元素后的 next 元素(紧挨着的，同桌)  
prev ~ siblings: 匹配 prev 元素之后的所有 siblings 元素(兄弟)

例子

```javascript
<html>
	<head>
		<meta charset="UTF-8">
		<title>层级选择器</title>
		<link rel="stylesheet" href="../../css/style.css" />
		<script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
		<script>
			$(function(){
				$("#btn1").click(function(){
					$("body div").css("background-color","pink");
				});
				$("#btn2").click(function(){
					$("body>div").css("background-color","pink");
				});
				$("#btn3").click(function(){
					$("#two+div").css("background-color","pink");
				});
				$("#btn4").click(function(){
					$("#one~div").css("background-color","pink");
				});
			});
			
		</script>
		
		
	</head>
	<body>
		<input type="button" id="btn1" value="选择body中的所有的div元素"/>
		<input type="button" id="btn2" value="选择body中的第一级的孩子"/>
		<input type="button" id="btn3" value="选择id为two的元素的下一个元素"/>
		<input type="button" id="btn4" value="选择id为one的所有的兄弟元素"/>
		
		<hr/>
		<div id="one">
			<div class="mini">
				111
			</div>
		</div>
		
		<div id="two">
			<div class="mini">
				222
			</div>
			<div class="mini">
				333
			</div>
		</div>
		
		<div id="three">
			<div class="mini">
				444
			</div>
			<div class="mini">
				555
			</div>
			<div class="mini">
				666
			</div>
		</div>
		
		<span id="four">
			
		</span>
	</body>
</html>
```

# 基本过滤选择器

    $('li').first() 等价于：$(“li:first”)

![](http://ovoxjpcrm.bkt.clouddn.com/1dc9603d55e3c5fdd2e68251dfbad6d6.png)

例子：

```javascript
<html>
	<head>
		<meta charset="UTF-8">
		<title>基本过滤选择器</title>
		<link rel="stylesheet" href="../../css/style.css" type="text/css"/>
		<script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
		<script>
			$(function(){
				$("#btn1").click(function(){
					$("div:first").css("background-color","pink");
				});
				$("#btn2").click(function(){
					$("div:last").css("background-color","pink");
				});
				$("#btn3").click(function(){
					$("div:odd").css("background-color","pink");
				});
				$("#btn4").click(function(){
					$("div:even").css("background-color","pink");
				});
			});
		</script>
		
	</head>
	<body>
		<input type="button" id="btn1" value="body中的第一个div元素"/>
		<input type="button" id="btn2" value="body中的最后一个div元素"/>
		<input type="button" id="btn3" value="选择body中的奇数的div"/>
		<input type="button" id="btn4" value="选择body中的偶数的div"/>
		
		<hr/>
		<div id="one">
			<div class="mini">
				111
			</div>
		</div>
		
		<div id="two">
			<div class="mini">
				222
			</div>
			<div class="mini">
				333
			</div>
		</div>
		
		<div id="three">
			<div class="mini">
				444
			</div>
			<div class="mini">
				555
			</div>
			<div class="mini">
				666
			</div>
		</div>
		
		<span id="four">
			
		</span>
	</body>
</html>
```


# 属性选择器

![属性选择器](http://ovoxjpcrm.bkt.clouddn.com/234b34b6e9741a58915c19a51d6d3620.png)

```javascript
<html>
	<head>
		<meta charset="UTF-8">
		<title>层级选择器</title>
		<link rel="stylesheet" href="../../css/style.css"  type="text/css"/>
		<script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
		<script>
			$(function(){
				$("#btn1").click(function(){
					$("div[id]").css("background-color","pink");
				});
				$("#btn2").click(function(){
					$("div[id='two']").css("background-color","pink");
				});
			});
			
		</script>
	</head>
	<body>
		<input type="button" id="btn1" value="选择有id属性的div"/>
		<input type="button" id="btn2" value="选择有id属性的值为two的div"/>
		
		<hr/>
		<div id="one">
			<div class="mini">
				111
			</div>
		</div>
		
		<div id="two">
			<div class="mini">
				222
			</div>
			<div class="mini">
				333
			</div>
		</div>
		
		<div id="three">
			<div class="mini">
				444
			</div>
			<div class="mini">
				555
			</div>
			<div class="mini">
				666
			</div>
		</div>
		
		<span id="four">
			
		</span>
	</body>
</html>
```

# 表单选择器

![表单选择器](http://ovoxjpcrm.bkt.clouddn.com/ed030dcbe1b29b2bb7ab92a637ccbe95.png)

```javascript
<html>
	<head>
		<meta charset="UTF-8">
		<title>表单选择器</title>
		<link rel="stylesheet" type="text/css" href="../../css/style.css"/>
		<script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
		<script>
			$(function(){
				$("#btn1").click(function(){
					$(":input").css("background-color","pink");
				});
				$("#btn2").click(function(){
					$(":text").css("background-color","pink");
				});
			});
		</script>
	</head>
	<body>
		<input type="button" id="btn1" value="选择所有input元素" />
		<input type="button" id="btn2" value="选择文本框" />
		<br/>
		<form>
			<input type="text" /><br />
			<input type="checkbox" /><br />
			<input type="radio" /><br />
			<input type="image" /><br />
			<input type="file" /><br />
			<input type="submit" />
			<input type="reset" /><br />
			<input type="password" /><br />
			<input type="button" /><br />
			<select><option/></select><br />
			<textarea></textarea><br />
			<button></button>
		</form>
	</body>
</html>
```
