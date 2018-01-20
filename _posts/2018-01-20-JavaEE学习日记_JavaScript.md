---
layout:     post
title:      JavaEE学习日记_JavaScript
subtitle:   JavaScript
date:       2018-01-20
author:     ZL
header-img: img/20180120.jpg
catalog: 	 true
tags:
    - JavaScript
    - JavaEE
---


# 关于thead和tbody
- 这个是放在table里面的，把表格的标题行和内容行分开

```html
<table border="1" width="500" height="50" align="center" id="tbl">
    <thead>
        <tr>
            <th>编号</th>
            <th>姓名</th>
            <th>年龄</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>张三</td>
            <td>22</td>
        </tr>
        <tr>
            <td>2</td>
            <td>李四</td>
            <td>25</td>
        </tr>
        <tr>
            <td>3</td>
            <td>王五</td>
            <td>27</td>
        </tr>
        <tr>
            <td>4</td>
            <td>赵六</td>
            <td>29</td>
        </tr>
        <tr>
            <td>5</td>
            <td>田七</td>
            <td>30</td>
        </tr>
        <tr>
            <td align="center">6</td>
            <td>汾酒</td>
            <td>20</td>
        </tr>
    </tbody>
</table>
```

>其中thead里面用tr和th
>
>tbody里面用tr和td

tr和td的差别不大，一般tr用在标题的单元格里面，字体一般会加粗，td用在表格内容的单元格里面



- 用JavaScript获取table里面的内容

  拿到表格
  >var tbid = window.document.getElementById("tbl");

  拿到第i个tbody
  >tbid.tBodies[0]

  拿到第i个tbody的第j行
  >tbid.tBodies[0].rows[i]

  拿到第i个tbody的行数
  >tbid.tBodies[i].rows.length;

  拿到table里面tbody的数量
  >tbid.tBodies.length

  设置tbody行的参数
  >tbid.tBodies[0].rows[i].style.xxxxx = "xxxx";
  >比如：
  >tbid.tBodies[0].rows[i].style.backgroundColor = "pink";


# 事件
之前学了onClick、onSubmit、onblur等
现在介绍2个**onmouseover**和**onMouseout**
指的是鼠标滑动上去和鼠标离开
![js事件](http://ovoxjpcrm.bkt.clouddn.com/da0506d212d628ccb8900385321185c0.png)


    onfocus/onblur:聚焦离焦事件，用于表单校验的时候比较合适。
    onclick/ondblclick:鼠标单击和双击事件
    onkeydown/onkeypress：搜索引擎使用较多
    onload：页面加载事件，所有的其它操作(匿名方式)都可以放到这个绑定的函数里面去。如果是有名称，那么在html页面中只能写一个。
    onmouseover/onmouseout/onmousemove：购物网站商品详情页。
    onsubmit：表单提交事件 ，有返回值，控制表单是否提交。
    onchange:当用户改变内容的时候使用这个事件(二级联动)





# getElementsByName("xxxx");
如果设置了name = "xxxx"属性，
通过这name得到该元素
返回值是一个数组，所有name叫xxxx的都会得到


# checkBox有一个属性是checked，可以用来判断是否选中，也可以直接设置它。


# JavaScript的DOM操作
![](http://ovoxjpcrm.bkt.clouddn.com/e90a27e4e3af191dfb584a1a5a7ae337.png)


    Document:整个html文件都成为一个document文档
    Element:所有的标签都是Element元素
    Attribute：标签里面的属性
    Text：标签中间夹着的内容为text文本
    Node:document、element、attribute、text统称为节点node.


## Document对象
每个载入浏览器的 HTML 文档都会成为 Document 对象。
![document对象的方法](http://ovoxjpcrm.bkt.clouddn.com/d9be82baadc70cf534b28e6ebfcb5c8d.png)

后面两个方法获取之后需要遍历！

以下两个方法很重要，但是在手册中查不到！
创建文本节点：document.createTextNode()
创建元素节点：document.createElement()

## Element对象
我们所认知的html页面中所有的标签都是element元素

element.appendChild()向元素添加新的子节点，作为最后一个子节点。  
element.firstChild返回元素的首个子节点。  
element.getAttribute()返回元素节点的指定属性值。  
element.innerHTML设置或返回元素的内容。  
element.insertBefore()在指定的已有的子节点之前插入新节点。  
element.lastChild返回元素的最后一个子元素。  
element.setAttribute()把指定属性设置或更改为指定值。  
element.removeChild()从元素中移除子节点。  
element.replaceChild()替换元素中的子节点。


## Attribute对象  
我们所认知的html页面中所有标签里面的属性都是attribute
![attribute方法](http://ovoxjpcrm.bkt.clouddn.com/8426e9708487c8aaae8a61d7213d45c3.png)


例子：  
在页面中使用列表显示一些城市


```html
<ul>
	<li>北京</li>
	<li>上海</li>
	<li>广州</li>
</ul>
```


我们希望点击一个按钮实现动态添加城市。  

分析：  
	事件(onclick)  
	获取ul元素节点  
	创建一个城市的文本节点  
	创建一个li元素节点  
	将文本节点添加到li元素节点中去。  
	使用element里面的方法appendChild()来添加子节点  
	
代码：  

```html
<html>
<head>
    <title>Title</title>
    <meta charset="utf-8">
    <script>
        function addCity() {
            var cityEle = window.document.getElementById("city");
            var node = window.document.createTextNode("shanghai");
            var element = window.document.createElement("li");
            element.appendChild(node);
            cityEle.appendChild(element);
        }
    </script>
</head>
<body>
    <ul id="city">
        <li>北京</li>
        <li>北京</li>
        <li>北京</li>
    </ul>
    <input type="button" onclick="addCity()" value="添加">
</body>
</html>
```

# 创建数组的方式
new Array();  
new Array(size);  
new Array(element0,element1,element2......)  


# 省市联动的例子

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>省市联动2</title>
    <meta charset="utf-8">
    <script>
        //1.创建一个二维数组用于存储省份和城市
        var cities = new Array(3);
        cities[0] = new Array("武汉市","黄冈市","襄阳市","荆州市");
        cities[1] = new Array("长沙市","郴州市","株洲市","岳阳市");
        cities[2] = new Array("石家庄市","邯郸市","廊坊市","保定市");
        cities[3] = new Array("郑州市","洛阳市","开封市","安阳市");

        function chooseCity(value) {
            var cityEle = window.document.getElementById("city");
            cityEle.options.length=0;
            switch (value-1){
                case 0:
                    for (var i=0;i<cities[0].length;i++){
                        var optionElement = window.document.createElement("option");
                        var textNode = window.document.createTextNode(cities[0][i]);
                        optionElement.appendChild(textNode);
                        cityEle.appendChild(optionElement);
                    }
                    break;
                case 1:
                    for (var i=0;i<cities[1].length;i++){
                        var optionElement = window.document.createElement("option");
                        var textNode = window.document.createTextNode(cities[1][i]);
                        optionElement.appendChild(textNode);
                        cityEle.appendChild(optionElement);
                    }
                    break;
                case 2:
                    for (var i=0;i<cities[2].length;i++){
                        var optionElement = window.document.createElement("option");
                        var textNode = window.document.createTextNode(cities[2][i]);
                        optionElement.appendChild(textNode);
                        cityEle.appendChild(optionElement);
                    }
                    break;
                case 3:
                    for (var i=0;i<cities[3].length;i++){
                        var optionElement = window.document.createElement("option");
                        var textNode = window.document.createTextNode(cities[3][i]);
                        optionElement.appendChild(textNode);
                        cityEle.appendChild(optionElement);
                    }
                    break;
            }
        }
    </script>
</head>
<body>
    <select onclick="chooseCity(this.value)">
        <option value=0>just choose</option>
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

# JavaScript内置对象
![js内置对象](http://ovoxjpcrm.bkt.clouddn.com/085e4d42df587d5e22ada03760ff8573.png)

## Array对象
![数组的创建](http://ovoxjpcrm.bkt.clouddn.com/a13a47c64918217b22831815579dfd5f.png)

数组的特点：  
	长度可变！数组的长度=最大角标+1  
  **Java的数组长度不可变**

## Boolean对象
![Boolean对象创建](http://ovoxjpcrm.bkt.clouddn.com/55aca1e82a597e7714fd0d169338d1e4.png)

如果value 不写，那么默认创建的结果为false


## Date对象
getTime()
返回 1970 年 1 月 1 日至今的毫秒数。
解决浏览器缓存问题

## Math和Number对象
与java里面的基本一致。

## String对象
match()找到一个或多个正则表达式的匹配。  
substr()从起始索引号提取字符串中指定数目的字符。  
substring()提取字符串中两个指定的索引号之间的字符。  

例子  

```javascript
<script>
	var str = "-a-b-c-d-e-f-";
  
  //从第二位开始取四个
	var str1 = str.substr(2,4);//-b-c
	//alert(str1);
	
  //取第二到第四位的
	var str2 = str.substring(2,4);//-b
	alert(str2);
</script>
```

## RegExp对象
正则表达式对象  
test检索字符串中指定的值。返回 true 或 false。


# 全局对象
全局属性和函数可用于所有内建的 JavaScript 对象  
![js全局函数](http://ovoxjpcrm.bkt.clouddn.com/a8bca5a1afff5c32e585d18c0036491f.png)

例子：  

```html
<script>
	var str = "张三";
	//alert(encodeURI(str));//%E5%BC%A0%E4%B8%89
	//alert(encodeURIComponent(str));//%E5%BC%A0%E4%B8%89
	
	//alert(decodeURI(encodeURI(str)));//张三
	//alert(decodeURIComponent(encodeURIComponent(str)));//张三
	
	var str1 = "http://www.itheima.cn";
	//alert(encodeURI(str1));//http://www.itheima.cn
	//alert(encodeURIComponent(str1));//http%3A%2F%2Fwww.itheima.cn
	
	//alert(decodeURI(encodeURI(str1)));//http://www.itheima.cn
	//alert(decodeURIComponent(encodeURIComponent(str1)));//http://www.itheima.cn
	
	var str2 = "alert('abc')";
	//alert(str2);
	eval(str2);
	
</script>
```
