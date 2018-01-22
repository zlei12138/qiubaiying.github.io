---
layout:     post
title:      JavaEE学习日记_JQuery片面总结
subtitle:   JQuery
date:       2018-01-22
author:     ZL
header-img: img/20180122.jpg
catalog: 	 true
tags:
    - JQuery
    - JavaEE
---



*强势总结操作*

# JQery属性操作

## 属性操作attr

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>属性操作_attr</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js" ></script>
		<script>
			/**
			 * 需求：
			 * 	1.获取图片的路径
			 * 	2.设置图片的高度属性
			 *  3.给图片设置多个属性(宽度和高度)
			 *  4.移出图片的高度属性
			 */
			
			/**
			 * 方法分析：
			 * 	1.attr(name):读(获)取属性的值
			 *  2.attr(key,value)：设置属性值
			 *  3.attr(properties)：同时设置多个属性
			 *  4.removeAttr(name)：删除某个属性
			 * 
			 * 所有方法注意查看运行显示结果，查看其中的某个方法时，请注释其它方法，后面的案例不再说明！
			 */
			
			/*
			 * 代码实现
			 */
			$(function(){
				//1.attr(name):该方法用于获取属性的值(根据属性的名称)
				var srcEle = $("img").attr("src");
				alert(srcEle);//输出结果为该图片的路径：../../img/1.jpg
				
				//2.attr(key,value):该方法用于设置属性和值
				$("img").attr("height","800px");//设置该图片的高度属性，注意观察图片的高度变化
				
				//3.attr(properties)：同时设置多个属性值
				//$("img").attr({"width":"1300px","height":"300px"});//注意观察图片发生的变化(宽高都改变)
				
				//4.removeAttr(name)：删除某个属性和值
				$("img").removeAttr("width");//清除width属性后，图片变回其自身原有的大小
				
			});
		</script>
	</head>
	<body>
		<img src="../../img/1.jpg"  width="800px" />
	</body>
</html>
```

## css类操作

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>属性操作_CSS类</title>
		<style type="text/css">
			.divclass {
				color: red;
			}
			
			.div1 {
				background-color: yellow;
			}
			
			#div1 {
				border: 1px solid black;
				width: 400px;
				height: 250px;
				margin: auto;
			}
			
			#father {
				border: 1px solid white;
				width: 450px;
				height: 300px;
				margin: auto;
				text-align: center;
			}
		</style>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js"></script>
		<script>
			/**
			 * 需求：
			 * 1.点击button，使一个div的背景颜色变为 黄色
			 * 2.点击button，清空之前设置的背景颜色
			 * 3.通过toggleClass(class) 实现点击字体变为紅色，再点击样式还原
			 */

			/**
			 * 方法分析：
			 *  1.addClass(class) 添加一个class属性
			 *  2.removeClass([class]) 移除一个class属性
			 *  3.toggleClass(class) 如果存在(不存在)就删除(添加)一个类
			 */

			/**
			 *代码实现 
			 */
			$(function() {
				// 1.点击button，使一个div的背景颜色变为黄色
				$("#button1").click(function() {
					$("#div1").addClass("div1");
				});

				// 2.点击button，清空之前设置的背景颜色
				$("#button2").click(function() {
					$("#div1").removeClass("div1");
				});

				// 3.通过toggleClass(class) 实现点击字体变为紅色，再点击样式还原
				$("#button3").click(function() {
					$("#div1").toggleClass("divclass");
				});
			});
		</script>
	</head>

	<body>
		<div id="father">
			<div id="div1">AAAAAA</div><br />
			<input type="button" value="背景颜色变为黄色" id="button1" />
			<input type="button" value="背景颜色清空" id="button2" />
			<input type="button" value="字体颜色开关" id="button3" />
		</div>
	</body>

</html>
```

## 属性操作_html代码

```javascript
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>属性操作_html代码</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js"></script>
		<script>
			/**
			 * 需求：
			 * 	1.点击按钮获取div中 html
			 * 	2.点击按钮设置div中html
			 */

			/**
			 * 方法分析
			 * 	1.html()方法用于读取innerHTML
			 * 	2.html(val)方法用于设置innerHTML
			 */
			
			/**
			 *代码实现 
			 */
			
			$(function(){
				//1.点击按钮获取div中 html
				$("#btn1").click(function(){
					var divEle = $("div").html();
					alert(divEle);//<p>传智播客</p>
				});
				
				//2.点击按钮设置div中html
				$("#btn2").click(function(){
					$("div").html("Java学院");//覆盖之前此位置的内容
				});
				
			})
		</script>
	</head>

	<body>
		<div><p>传智播客</p></div>
		<input type="button" value="获取html" id="btn1" />
		<input type="button" value="设置html" id="btn2" />
	</body>

</html>
```

## 属性操作_文本text

```javascript
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>属性操作_文本text</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js"></script>
		<script>
			/**
			 * 需求：
			 * 	1.点击按钮获取div中text
			 * 	2.点击按钮设置div中text
			 */

			/**
			 * 方法分析
			 * 	1.text()方法用于读取文本内容
			 * 	2.text(val)方法用于设置文本内容
			 */
			
			/**
			 *代码实现 
			 */
			
			$(function(){
				//1.点击按钮获取div中 text
				$("#btn1").click(function(){
					var divEle = $("div").text();
					alert(divEle);//传智播客
				});
				
				//2.点击按钮设置div中text
				$("#btn2").click(function(){
					$("div").text("Java学院");//覆盖之前此位置的内容
				});
				
			})
		</script>
	</head>

	<body>
		<div><p>传智播客</p></div>
		<input type="button" value="获取text" id="btn1" />
		<input type="button" value="设置text" id="btn2" />
	</body>

</html>
```


## 属性操作_值val

```javascript
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>属性操作_值val</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js"></script>
		<script>
			/**
			 * 需求：
			 *  1.点击按钮获得文本框、下拉框、单选框选中的value
			 *  2.点击按钮设置用户名的默认值为“老王”
			 */

			/**
			 * 方法分析
			 * 	1.val()方法用于读取元素value属性
			 * 	2.val(val)方法用于设置元素value属性
			 */
			
			/**
			 *代码实现 
			 */
			
			$(function(){
				//1.点击按钮获得文本框、下拉框、单选框选中的value
				$("#btn1").click(function(){
					alert($("#username").val());
					alert($("#city").val());
					alert($("input[type='radio']:checked").val());
				});
					
				//2.点击按钮设置用户名的默认值为“老王”
				$("#btn2").click(function(){
					$("#username").val("老王");
				});
			})
		</script>
	</head>

	<body>
		用户名 <input type="text" id="username" /> <br/> 
		性别 <input type="radio" name="gender" value="男" />男
			<input type="radio" name="gender" value="女" checked="checked"/> 女<br/> 
		城市
			<select id="city">
				<option value="北京">北京</option>
				<option value="上海">上海</option>
				<option value="广州">广州</option>
			</select> <br/>
			<input type="button" value="获取val" id="btn1" />
			<input type="button" value="设置val" id="btn2" />
	</body>

</html>
```

# JQuery文档处理

## 复制操作

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>文档处理_复制操作</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js" ></script>
		<script>
			/**
			 * 需求
			 * 	1.单击苹果、橘子或者菠萝列表项，复制点击的内容并追加到ul的末尾
			 *  2.单击苹果、橘子或者菠萝列表项，复制点击的内容并追加到ul的末尾，要求复制后的内容也具有复制的能力
			 */
			
			/**
			 * 方法分析：
			 * 	1.clone()：克隆匹配的DOM元素并且选中这些克隆的副本。
			 *  2.clone()：元素以及其所有的事件处理并且选中这些克隆的副本(简言之，副本具有与真身一样的事件处理能力)
			 */
		
			 $(function(){
			 	//点击li列表项，将当选点击的li内容追加到ul末尾
				$("ul li").click(function(){
				    //$(this).clone().appendTo("ul"); // 复制当前点击的节点，并将它追加到<ul>元素
				    $(this).clone(true).appendTo("ul");//设置参数true后，复制后的内容也具备单击事件
				})   
			  });
		</script>
	</head>
	<body>
		<p title="选择你最喜欢的水果." >你最喜欢的水果是?</p>
		<ul>
		  <li title='苹果'>苹果</li>
		  <li title='橘子'>橘子</li>
		  <li title='菠萝'>菠萝</li>
		</ul>
	</body>
</html>
```

## 插入操作

```javascript
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>文档处理_插入操作</title>
		<script src="../../js/jquery-1.11.0.min.js" type="text/javascript" charset="utf-8"></script>
		<script type="text/javascript">
			/**
			 * 需求 
			 *  在id=edu下增加<option value="大专">大专</option> 
			 */

			/**
			 * 方法分析：
			 *  内部插入
			 * 		1.append(content):内部结尾处，将B追加到A里面去
			 *  	2.appendTo(content):内部结尾处，将A追加到B里面去
			 *  	3.prepend(content):内部开始处，将B追加到A里面去
			 *  	4.prependTo(content):内部开始处，将B追加到A里面去
			 * 	外部插入
			 *  	1.after(content):外部，将B追加到A后面
			 * 		2.before(content):外部，将A追加到B前面
			 * 		3.insertAfter(content):外部，将A追加到B后面
			 * 		4.insertBefore(content)::外部，将A追加到B前面
			 */

			$(function() {
				// 追加 option 内容大专
				// 创建元素
				var $newNode = $("<option value='大专'>大专</option>");
				
				//内部插入
				//$("#edu").append($newNode);   // 内部结尾，将B追加到A里面去
				//$("#edu").prepend($newNode);  // 内部开始，将B追加到A里面去

				//外部插入
				//$("option[value='本科']").after($newNode);
				$newNode.insertBefore($("option:contains('硕士')"));
			});
		</script>

	</head>

	<body>
		<select id="edu">
			<option value="博士">博士</option>
			<option value="硕士">硕士</option>
			<option value="本科">本科</option>
			<option value="高中">高中</option>
		</select>

	</body>

</html>
```

## 替换操作

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>文档处理_替换操作</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js" ></script>
		<script>
			$(function(){
				//将B的内容替换掉A处的内容
			 	$("p").replaceWith("<strong>你最不喜欢的水果是?</strong>"); 
			 	// 同样的实现： $("<strong>你最不喜欢的水果是?</strong>").replaceAll("p"); 
			});
		</script>
	</head>
	<body>
		<p title="选择你最喜欢的水果." >你最喜欢的水果是?</p>
		<ul>
		  <li title='苹果'>苹果</li>
		  <li title='橘子'>橘子</li>
		  <li title='菠萝'>菠萝</li>
		</ul>
	</body>
</html>
```

## 删除操作

```javascript
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>文档处理_删除操作</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js"></script>
		<script type="text/javascript">
			/**
			 * 需求
			 * 	分别使用detach和remove 删除带有click事件的p标签，删除后再将p 重新加入body 查看事件是否存在
			 */
			
			/**
			 * 方法分析
			 * 	1.remove()：删除节点后，事件也会删除
			 *  2.detach()：删除节点后，事件会保留 从1.4新API 
			 *  3.empty():清空元素中的所有后代节点。(这个案例是要删除而不是清空，需要注意)
			 */
			
			/**
			 * 代码实现 
			 */
			$(function() {
				$("p").click(function() {
					alert($(this).text());
				});
				// 使用remove方法删除 p元素，连同事件一起删除
				//var $p = $("p").remove();
				// 使用detach删除，事件会保留
				var $p = $("p").detach();

				$("div").append($p);
			});
		</script>

	</head>

	<body>
		<p>AAA</p>
		
		<div>BBB</div>
	</body>

</html>
```

例子2

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>删除相关操作的区别</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js" ></script>
		<script>
			$(function(){
				//清空第二个li元素节点的所有后代节点(此处是文本节点橘子)，通过firebug查看html源码验证
				//$("ul li:eq(1)").empty();
				
				//删除第一个li元素节点
				$("ul li:eq(0)").remove();
			});
		</script>
	</head>
	<body>
		<p title="选择你最喜欢的水果.">你最喜欢的水果是?</p>
		<ul>
			<li title='苹果'>苹果</li>
			<li title='橘子'>橘子</li>
			<li title='菠萝'>菠萝</li>
		</ul>
	</body>
</html>
```


# JQuery文档处理遍历操作

## each(callback)

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>使用对象访问方式遍历</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js" ></script>
		<script>
			$(function(){
				// 全选/ 全不选
				$("#checkallbox").click(function(){
					var isChecked = this.checked;
					//使用对象访问的方式进行遍历，语法：$().each(function(){})
					$("input[name='hobby']").each(function(){
						this.checked = isChecked;
					});
				});
			});
		</script>
	</head>
	<body>
		请选择您的爱好<br/>
		<input type="checkbox" id="checkallbox" /> 全选/全不选 <br/>
		<input type="checkbox" name="hobby" value="足球" /> 足球
		<input type="checkbox" name="hobby" value="篮球" /> 篮球
		<input type="checkbox" name="hobby" value="游泳" /> 游泳
		<input type="checkbox" name="hobby" value="唱歌" /> 唱歌 <br/>
	</body>
</html>
```

## $.each()

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>使用工具类遍历方式</title>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js" ></script>
		<script>
			/**
			 * 说明：两种遍历方式，掌握其中一种即可，个人觉得此种方式较容易理解!
			 */
		
			$(function(){
				// 全选/ 全不选
				$("#checkallbox").click(function(){
					var isChecked = this.checked;
					//使用工具类遍历方式，语法：$.each(array，function(i,j){})  其中array代表被遍历的对象，i代表角标，j代表遍历后的内容。
					$.each($("input[name='hobby']"), function(i,j) {
						j.checked = isChecked;
					});
				});
			});
		</script>
	</head>
	<body>
		请选择您的爱好<br/>
		<input type="checkbox" id="checkallbox" /> 全选/全不选 <br/>
		<input type="checkbox" name="hobby" value="足球" /> 足球
		<input type="checkbox" name="hobby" value="篮球" /> 篮球
		<input type="checkbox" name="hobby" value="游泳" /> 游泳
		<input type="checkbox" name="hobby" value="唱歌" /> 唱歌 <br/>
	</body>
</html>
```

# JQuery的css操作

```javascript
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>属性操作_CSS类</title>
		<style type="text/css">
			.divclass {
				color: red;
			}
			
			.div1 {
				background-color: yellow;
			}
			
			#div1 {
				border: 1px solid black;
				width: 400px;
				height: 250px;
				margin: auto;
			}
			
			#father {
				border: 1px solid white;
				width: 450px;
				height: 300px;
				margin: auto;
				text-align: center;
			}
		</style>
		<script type="text/javascript" src="../../js/jquery-1.11.0.min.js"></script>
		<script>
			/**
			 * 需求：
			 * 1.点击button，使一个div的背景颜色变为绿色
			 * 2.点击button，使一个div的背景颜色变为绿色以及里面内容的字体颜色变成红色
			 */

			/**
			 * 方法分析：
			 *  1.css(name, value) 设置一个CSS样式属性
			 *  2.css(properties) 传递key - value对象， 设置多个CSS样式属性
			 */

			/**
			 * CSS操作方法汇总：
			 * 1.通过attr属性设置 / 获取 style属性
			 * 	attr('style', 'color:red'); // 添加style属性
			 * 2.设置CSS样式属性
			 * 	css(name, value) 设置一个CSS样式属性
			 * 	css(properties) 传递key - value对象， 设置多个CSS样式属性		
			 * 3.设置class属性
			 * 	addClass(class) 添加一个class属性
			 * 	removeClass([class]) 移除一个class属性
			 * 	toggleClass(class) 如果存在(不存在)就删除(添加) 一个类
			 */
	
			/**
			 *代码实现 
			 */
			$(function() {
				// 1.点击button，使一个div的背景颜色变为绿色
				//方式一
				/*$("#button1").click(function() {
					$("#div1").css("background-color","green");
				});*/
				
				//方式二：
				$("#button1").click(function() {
					$("#div1").attr("style","background-color:green");
				});

				// 2.点击button，使一个div的背景颜色变为绿色，内容字体颜色变成红色
				$("#button2").click(function() {
					$("#div1").css({"background-color":"green","color":"red"});
				});
			});
		</script>
	</head>

	<body>
		<div id="father">
			<div id="div1">AAAAAA</div><br />
			<input type="button" value="背景颜色变为绿色" id="button1" />
			<input type="button" value="背景颜色变为绿色内容字体变成红色" id="button2" />
		</div>
	</body>

</html>
```

# JQuery事件

## 事件绑定

### 点击展开

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">

	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
		<title>点击展开</title>
		<link rel="stylesheet" type="text/css" href="../../../css/style.css" />
		<script src="../../../js/jquery-1.8.3.min.js" type="text/javascript"></script>
		<script type="text/javascript">
			$(function() {
				$("#panel h5.head").bind("click", function() {
					var $content = $(this).next();
					if($content.is(":visible")) {
						$content.hide();
					} else {
						$content.show();
					}
				})
			})
		</script>
	</head>

	<body>
		<div id="panel">
			<h5 class="head">什么是jQuery?</h5>
			<div class="content">
				jQuery是继Prototype之后又一个优秀的JavaScript库，它是一个由 John Resig 创建于2006年1月的开源项目。jQuery凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发Ajax。它独特而又优雅的代码风格改变了JavaScript程序员的设计思路和编写程序的方式。
			</div>
		</div>
	</body>

</html>
```

### 鼠标滑过

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">

	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
		<title>鼠标滑过</title>
		<link rel="stylesheet" type="text/css" href="../../../css/style.css" />
		<script src="../../../js/jquery-1.8.3.min.js" type="text/javascript"></script>
		<script type="text/javascript">
			$(function() {
				$(".head").mouseover(function() {
					$(this).next().show();
				}).mouseout(function() {
					$(this).next().hide();
				})
			})
		</script>
	</head>

	<body>
		<div id="panel">
			<h5 class="head">什么是jQuery?</h5>
			<div class="content">
				jQuery是继Prototype之后又一个优秀的JavaScript库，它是一个由 John Resig 创建于2006年1月的开源项目。jQuery凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发Ajax。它独特而又优雅的代码风格改变了JavaScript程序员的设计思路和编写程序的方式。
			</div>
		</div>
	</body>

</html>
```

## 移除事件

### 事件移除

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">

	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
		<title>事件移除</title>
		<style type="text/css">
			* {
				margin: 0;
				padding: 0;
			}
			
			body {
				font-size: 13px;
				line-height: 130%;
				padding: 60px;
			}
			
			p {
				width: 200px;
				background: #888;
				color: white;
				height: 16px;
			}
		</style>
		<script src="../../../js/jquery-1.8.3.min.js" type="text/javascript"></script>
		<script type="text/javascript">
			$(function() {
				$('#btn').bind("click", function() {
					$('#test').append("<p>我的绑定函数1</p>");
				});
				$('#delAll').click(function() {
					$('#btn').unbind("click");
				});
			})
		</script>
	</head>

	<body>
		<button id="btn">点击我</button>
		<div id="test"></div>
		<button id="delAll">删除所有事件</button>
	</body>

</html>
```

## 事件合成

### 合成事件hover

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">

	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
		<title>合成事件hover</title>
		<link rel="stylesheet" type="text/css" href="../../../css/style.css" />
		<script src="../../../js/jquery-1.8.3.min.js" type="text/javascript"></script>
		<script type="text/javascript">
			$(function() {
				$(".head").hover(function() {
					$(this).next().show();
				}, function() {
					$(this).next().hide();
				})
			})
		</script>
	</head>

	<body>
		<div id="panel">
			<h5 class="head">什么是jQuery?</h5>
			<div class="content">
				jQuery是继Prototype之后又一个优秀的JavaScript库，它是一个由 John Resig 创建于2006年1月的开源项目。jQuery凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发Ajax。它独特而又优雅的代码风格改变了JavaScript程序员的设计思路和编写程序的方式。
			</div>
		</div>
	</body>

</html>
```

### 合成事件toggle

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">

	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
		<title>合成事件toggle</title>
		<link rel="stylesheet" type="text/css" href="../../../css/style.css" />
		<script src="../../../js/jquery-1.8.3.min.js" type="text/javascript"></script>
		<script type="text/javascript">
			$(function() {
				$(".head").toggle(function() {
					$(this).next().hide();
				}, function() {
					$(this).next().show();
				})
			})
		</script>
	</head>

	<body>
		<div id="panel">
			<h5 class="head">什么是jQuery?</h5>
			<div class="content">
				jQuery是继Prototype之后又一个优秀的JavaScript库，它是一个由 John Resig 创建于2006年1月的开源项目。jQuery凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发Ajax。它独特而又优雅的代码风格改变了JavaScript程序员的设计思路和编写程序的方式。
			</div>
		</div>
	</body>

</html>
```
