---
layout:     post
title:      JavaEE学习日记_JQuery下
subtitle:   JQuery
date:       2018-01-22
author:     ZL
header-img: img/20180122.jpg
catalog: 	 true
tags:
    - JQuery
    - JavaEE
---

# 二级联动的例子

代码：

```javascript
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>省市联动2</title>
    <meta charset="utf-8">
    <script type="text/javascript" src="jquery-1.8.3.js"></script>
    <script>
        //1.创建一个二维数组用于存储省份和城市
        var cities = new Array(3);
        cities[0] = new Array("武汉市","黄冈市","襄阳市","荆州市");
        cities[1] = new Array("长沙市","郴州市","株洲市","岳阳市");
        cities[2] = new Array("石家庄市","邯郸市","廊坊市","保定市");
        cities[3] = new Array("郑州市","洛阳市","开封市","安阳市");


        $(function () {
            $("#province").change(function () {
                //获取到省的value的值
                var value = this.value;

                //将之前添加的option删除
                $("#city option").detach();
                //$("#city option").remove();也可以
                //  $("#city").empty();也可以

                //遍历
                $.each(cities,function (i,n) {
                    if (i == value){
                        $.each(cities[i],function (j, m) {
                            var option = window.document.createElement("option");
                            var optionnode = window.document.createTextNode(m);
                            //option.appendChild(optionnode);
                            $(option).append(optionnode);
                            $("#city").append(option);
                        })
                    }
                })

            });

        })

    </script>
</head>
<body>
<select id="province">
    <option value=0>0</option>
    <option value=1>1</option>
    <option value=2>2</option>
    <option value=3>3</option>
    <option value=4>4</option>
</select>

<select id="city">

</select>
</body>
</html>
```

*知识点：*
## 删除函数（一个删除标签的内容，一个连标签都删除了）
![JQuery的删除函数](http://ovoxjpcrm.bkt.clouddn.com/7c6353e8d73e456f863b1004bbe71953.png)

empty():删除匹配的元素集合中所有的*子节点。*
![empty函数例子](http://ovoxjpcrm.bkt.clouddn.com/02bd170cb17210d78d55bce86f0226e1.png)

remove([expr]):从DOM中删除所有匹配的*元素。*
![remove例子](http://ovoxjpcrm.bkt.clouddn.com/df475a617dd6668184dbca59995b2936.png)

detach和remove相同

## 遍历函数
jQuery.each(object, [callback])

![](http://ovoxjpcrm.bkt.clouddn.com/43e9452598c09f22db0e65ab172267ae.png)

each(callback)
![](http://ovoxjpcrm.bkt.clouddn.com/fa30b890948fd60cea40fc695922d2e8.png)


## 文档处理操作
apend:  A.append(B)  将B追加到A的内容的末尾处  
appendTo: A.appendTo(B)  将A加到B内容的末尾处

# 常见事件
![常见事件](http://ovoxjpcrm.bkt.clouddn.com/e600adad52a619b9d9b0c31082dc9557.png)

例子：
事件绑定：

```javascript
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
```
鼠标滑过

```javascript
$(function() {
  $(".head").mouseover(function() {
    $(this).next().show();
  }).mouseout(function() {
    $(this).next().hide();
  })
})
```

事件移除

```javascript
$(function() {
  $('#btn').bind("click", function() {
    $('#test').append("<p>我的绑定函数1</p>");
  });
  $('#delAll').click(function() {
    $('#btn').unbind("click");
  });
})
```

事件合成

```javascript
$(function() {
  $(".head").hover(function() {
    $(this).next().show();
  }, function() {
    $(this).next().hide();
  })
})

$(function() {
  $(".head").toggle(function() {
    $(this).next().hide();
  }, function() {
    $(this).next().show();
  })
})


hover把鼠标放上去就行，toggle需要点击才行
```

# Jquery validation表单校验

![](http://ovoxjpcrm.bkt.clouddn.com/b9458883341ce572df0c50339449d1f5.png)

![](http://ovoxjpcrm.bkt.clouddn.com/c22f455ba5daf5a8bc12882d5ab89833.png)
![](http://ovoxjpcrm.bkt.clouddn.com/d621ba65f9f616cad6b25f3ce8a8cc54.png)

例子：

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>validate入门案例</title>
		<script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
		<!--validate.js是建立在jquery之上的，所以得先导入jquery的类库-->
		<script type="text/javascript" src="../../js/jquery.validate.min.js" ></script>
		<!--引入国际化js文件-->
		<script type="text/javascript" src="../../js/messages_zh.js" ></script>
		<script>
			$(function(){
				$("#checkForm").validate({
					rules:{
						username:{
							required:true,
							minlength:6
						},
						password:{
							required:true,
							digits:true,
							minlength:6
						}
					},
					messages:{
						username:{
							required:"用户名不能为空!",
							minlength:"用户名不得少于6位！"
						},
						password:{
							required:"密码不能为空!",
							digits:"密码必须是整数!",
							minlength:"密码不得少于6位!"
						}
					}
				});
			});
		</script>
		
	</head>
	<body>
		<form action="#" id="checkForm">
			用户名：<input type="text" name="username" /><br />
			密码：<input type="password" name="password"/><br />
			<input type="submit"/>
		</form>
	</body>
</html>
```
