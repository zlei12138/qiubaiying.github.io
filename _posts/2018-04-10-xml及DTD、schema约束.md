---
layout:     post
title:      xml及DTD、schema约束
subtitle:   用法
date:       2018-04-10
author:     ZL
header-img: img/20180410.jpg
catalog: 	 true
tags:
    - DTD
    - XML
    - schema
---

xml常用来存放配置文件或者存放数据，Android中的xml的作用我觉得也是存放配置文件的。

# xml语法注意  

1. 开头必须是(放在0行0列的位置)：  
       <?xml version="1.0" encoding="UTF-8"   
      version有1.0和1.1，但是基本都用1.0

2. 因为很多符号己经被XML文档结构所使用，所以在元素体或属性值中想使用这些符号就必须使
用转义字符， 例如： “〈”、“〉”、“ ’ ”、“””、“＆ ”。  

    ![xml转义字符](http://ovoxjpcrm.bkt.clouddn.com/2ea93938a2980c5c4602ae36270f4c37.png)

3. 当大量的转义字符出现在xml文档中时，会使xml文档的可读性大幅度降低。这时如果使用CDATA
段就会好一些。

    ```
    <1[CDATA[
      任意内容
    ]]〉
    ```

# DTD约束

因为xml里面的元素体没有限制，使用DTD约束可以限制xml的约束体，规定XML文档中元素的
名称， 子元素的名称及顺序， 元素的属性等。

## DTD的引入

![DTD引入](http://ovoxjpcrm.bkt.clouddn.com/76c6a562fe13138cc7e9f043e66e0bcc.png)


## DTD实例

通常DTD都不是自己写的

![DTD实例](http://ovoxjpcrm.bkt.clouddn.com/df7c96425c9055e8a08c8af18dbeb0e4.png)


这样在xml中就必须按照DTD要求的写了，不然会报错。


# schema约束

和DTD功能一样，但是比DTD更强大  
schema约束本身是xml文件，但是扩展名是xsd

实例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 
	模拟servlet2.5规范，如果开发人员需要在xml使用当前Schema约束，必须包括指定命名空间。
	格式如下：
	<web-app xmlns="http://www.example.org/web-app_2_5" 
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xsi:schemaLocation="http://www.example.org/web-app_2_5 web-app_2_5.xsd"
			version="2.5">
-->
<xsd:schema xmlns="http://www.w3.org/2001/XMLSchema" 
	targetNamespace="http://www.example.org/web-app_2_5"
	xmlns:xsd="http://www.w3.org/2001/XMLSchema"
	xmlns:tns="http://www.example.org/web-app_2_5" 
	elementFormDefault="qualified">
	
	<xsd:element name="web-app">
		<xsd:complexType>
			<xsd:choice minOccurs="0" maxOccurs="unbounded">
				<xsd:element name="servlet">
					<xsd:complexType>
						<xsd:sequence>
							<xsd:element name="servlet-name"></xsd:element>
							<xsd:element name="servlet-class"></xsd:element>
						</xsd:sequence>
					</xsd:complexType>
				</xsd:element>
				<xsd:element name="servlet-mapping">
					<xsd:complexType>
						<xsd:sequence>
							<xsd:element name="servlet-name"></xsd:element>
							<xsd:element name="url-pattern" maxOccurs="unbounded"></xsd:element>
						</xsd:sequence>
					</xsd:complexType>
				</xsd:element>
				<xsd:element name="welcome-file-list">
					<xsd:complexType>
						<xsd:sequence>
							<xsd:element name="welcome-file" maxOccurs="unbounded"></xsd:element>
						</xsd:sequence>
					</xsd:complexType>
				</xsd:element>
			</xsd:choice>
			<xsd:attribute name="version" type="double" use="optional"></xsd:attribute>
		</xsd:complexType>
	</xsd:element>
</xsd:schema>
```

看懂即可  
在Android中的xml约束也是用的这种。
