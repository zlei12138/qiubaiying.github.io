---
layout:     post
title:      hibernate_多表
subtitle:   多表操作
date:       2018-05-26
author:     ZL
header-img: img/20180526.jpg
catalog: 	 true
tags:
    - hibernate
    - 多表
---

# 一对多多对一

## 数据库中的一对多，多对一
> 指的是两个数据库表之间的关系，比如一个数据下面的例子中，一个表是存放客户的表，另一个表存放联系人。然后一个客户会有多个联系人，即一个客户和多个联系人绑定，那么对于客户的表就是一对多，对于联系人表就是多对一。 

> 如何将两个表的关系表达出来，在数据库中有一个叫**外键**的东西。在一对多中处在“多”位置的那个表中要表明外键是“一”的表的某个字段。

## hibernate中处理一对多多对一的操作  

1. 首先看一下两个表的内容。 
 
    ```sql
    --客户表的内容
    CREATE TABLE `cst_customer` (
    	  `cust_id` BIGINT(32) NOT NULL AUTO_INCREMENT COMMENT '客户编号(主键)',
    	  `cust_name` VARCHAR(32) NOT NULL COMMENT '客户名称(公司名称)',
    	  `cust_source` VARCHAR(32) DEFAULT NULL COMMENT '客户信息来源',
    	  `cust_industry` VARCHAR(32) DEFAULT NULL COMMENT '客户所属行业',
    	  `cust_level` VARCHAR(32) DEFAULT NULL COMMENT '客户级别',
    	  `cust_linkman` VARCHAR(64) DEFAULT NULL COMMENT '联系人',
    	  `cust_phone` VARCHAR(64) DEFAULT NULL COMMENT '固定电话',
    	  `cust_mobile` VARCHAR(16) DEFAULT NULL COMMENT '移动电话',
    	  PRIMARY KEY (`cust_id`)
    	) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
    ```  
    ```sql
    -- 联系人表的内容
    -- 在这里表明了外键叫“lkm_cust_id”，它指向了客户表的“cust_id”字段。
    CREATE TABLE `cst_linkman` (
		  `lkm_id` bigint(32) NOT NULL AUTO_INCREMENT COMMENT '联系人编号(主键)',
		  `lkm_name` varchar(16) DEFAULT NULL COMMENT '联系人姓名',
		  `lkm_cust_id` bigint(32) NOT NULL COMMENT '客户id',
		  `lkm_gender` char(1) DEFAULT NULL COMMENT '联系人性别',
		  `lkm_phone` varchar(16) DEFAULT NULL COMMENT '联系人办公电话',
		  `lkm_mobile` varchar(16) DEFAULT NULL COMMENT '联系人手机',
		  `lkm_email` varchar(64) DEFAULT NULL COMMENT '联系人邮箱',
		  `lkm_qq` varchar(16) DEFAULT NULL COMMENT '联系人qq',
		  `lkm_position` varchar(16) DEFAULT NULL COMMENT '联系人职位',
		  `lkm_memo` varchar(512) DEFAULT NULL COMMENT '联系人备注',
		  PRIMARY KEY (`lkm_id`),
		  KEY `FK_cst_linkman_lkm_cust_id` (`lkm_cust_id`),
		  CONSTRAINT `FK_cst_linkman_lkm_cust_id` FOREIGN KEY (`lkm_cust_id`) REFERENCES `cst_customer` (`cust_id`) ON DELETE NO ACTION ON UPDATE NO ACTION
		) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
    ``` 


2. 创建关于两个表的Bean类  
客户的bean类：  
    ```java
    package cn.itheima.domain;

    import java.util.HashSet;
    import java.util.Set;

    public class Customer {

    	private Long cust_id;
    	
    	private String cust_name;
    	private String cust_source;
    	private String cust_industry;
    	private String cust_level;
    	private String cust_linkman;
    	private String cust_phone;
    	private String cust_mobile;
    	
    	private Set<LinkMan> linkMens = new HashSet<LinkMan>();
    	
    	public Long getCust_id() {
    		return cust_id;
    	}
    	public void setCust_id(Long cust_id) {
    		this.cust_id = cust_id;
    	}
    	public String getCust_name() {
    		return cust_name;
    	}
    	public void setCust_name(String cust_name) {
    		this.cust_name = cust_name;
    	}
    	public String getCust_source() {
    		return cust_source;
    	}
    	public void setCust_source(String cust_source) {
    		this.cust_source = cust_source;
    	}
    	public String getCust_industry() {
    		return cust_industry;
    	}
    	public void setCust_industry(String cust_industry) {
    		this.cust_industry = cust_industry;
    	}
    	public String getCust_level() {
    		return cust_level;
    	}
    	public void setCust_level(String cust_level) {
    		this.cust_level = cust_level;
    	}
    	public String getCust_linkman() {
    		return cust_linkman;
    	}
    	public void setCust_linkman(String cust_linkman) {
    		this.cust_linkman = cust_linkman;
    	}
    	public String getCust_phone() {
    		return cust_phone;
    	}
    	
    	public Set<LinkMan> getLinkMens() {
    		return linkMens;
    	}
    	public void setLinkMens(Set<LinkMan> linkMens) {
    		this.linkMens = linkMens;
    	}
    	public void setCust_phone(String cust_phone) {
    		this.cust_phone = cust_phone;
    	}
    	public String getCust_mobile() {
    		return cust_mobile;
    	}
    	public void setCust_mobile(String cust_mobile) {
    		this.cust_mobile = cust_mobile;
    	}
    	@Override
    	public String toString() {
    		return "Customer [cust_id=" + cust_id + ", cust_name=" + cust_name + "]";
    	}
    }
    ```

    >**linkMens字段是客户表中没有的，这里写出来linkMens并给他设置set，get是因为一个客户对应多个联系人，在后面的代码中有类似将联系人和客户绑定的操作，需要用到linkMens字段。**  


联系人的bean类：  
  ```java
  package cn.itheima.domain;
  //联系人实体
  public class LinkMan {

  	private Long lkm_id;
  	private Character lkm_gender;
  	private String lkm_name;
  	private String lkm_phone;
  	private String lkm_email;
  	private String lkm_qq;
  	private String lkm_mobile;
  	private String lkm_memo;
  	private String lkm_position;
  	
  	//表达多对一关系
  	private Customer customer ;

  	public Customer getCustomer() {
  		return customer;
  	}
  	public void setCustomer(Customer customer) {
  		this.customer = customer;
  	}
  	public Long getLkm_id() {
  		return lkm_id;
  	}
  	public void setLkm_id(Long lkm_id) {
  		this.lkm_id = lkm_id;
  	}
  	public Character getLkm_gender() {
  		return lkm_gender;
  	}
  	public void setLkm_gender(Character lkm_gender) {
  		this.lkm_gender = lkm_gender;
  	}
  	public String getLkm_name() {
  		return lkm_name;
  	}
  	public void setLkm_name(String lkm_name) {
  		this.lkm_name = lkm_name;
  	}
  	public String getLkm_phone() {
  		return lkm_phone;
  	}
  	public void setLkm_phone(String lkm_phone) {
  		this.lkm_phone = lkm_phone;
  	}
  	public String getLkm_email() {
  		return lkm_email;
  	}
  	public void setLkm_email(String lkm_email) {
  		this.lkm_email = lkm_email;
  	}
  	public String getLkm_qq() {
  		return lkm_qq;
  	}
  	public void setLkm_qq(String lkm_qq) {
  		this.lkm_qq = lkm_qq;
  	}
  	public String getLkm_mobile() {
  		return lkm_mobile;
  	}
  	public void setLkm_mobile(String lkm_mobile) {
  		this.lkm_mobile = lkm_mobile;
  	}
  	public String getLkm_memo() {
  		return lkm_memo;
  	}
  	public void setLkm_memo(String lkm_memo) {
  		this.lkm_memo = lkm_memo;
  	}
  	public String getLkm_position() {
  		return lkm_position;
  	}
  	public void setLkm_position(String lkm_position) {
  		this.lkm_position = lkm_position;
  	}
  }
  ```

> 这里也多了一个属性叫做customer，是为了后面的绑定操作。和上面的linkMens作用一样。


3. 创建hbm.xml  

客户表的Bean类的hbm.xml：Customer.hbm.xml  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping package="cn.itheima.domain" >
	<class name="Customer" table="cst_customer" >
		<id name="cust_id"  >
			<generator class="native"></generator>
		</id>
		<property name="cust_name" column="cust_name" ></property>
		<property name="cust_source" column="cust_source" ></property>
		<property name="cust_industry" column="cust_industry" ></property>
		<property name="cust_level" column="cust_level" ></property>
		<property name="cust_linkman" column="cust_linkman" ></property>
		<property name="cust_phone" column="cust_phone" ></property>
		<property name="cust_mobile" column="cust_mobile" ></property>
	
		<!-- 集合,一对多关系,在配置文件中配置 -->
		<!-- 
			name属性:集合属性名
			column属性: 外键列名
			class属性: 与我关联的对象完整类名
		 -->
		<set name="linkMens">
			<key column="lkm_cust_id" ></key>
			<one-to-many class="LinkMan" />
		</set>
	</class>
</hibernate-mapping>
```
**这里指出set以及其内部的key和one-to-many*


联系人表的Bean类的hbm.xml：LinkMan.hbm.xml  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping package="cn.itheima.domain" >
	<class name="LinkMan" table="cst_linkman" >
		<id name="lkm_id"  >
			<generator class="native"></generator>
		</id>
		<property name="lkm_gender"  ></property>
		<property name="lkm_name"  ></property>
		<property name="lkm_phone"  ></property>
		<property name="lkm_email"  ></property>
		<property name="lkm_qq"  ></property>
		<property name="lkm_mobile"  ></property>
		<property name="lkm_memo"  ></property>
		<property name="lkm_position"  ></property>
		
		<!-- 多对一 -->
		<!-- 
			name属性:引用属性名
			column属性: 外键列名
			class属性: 与我关联的对象完整类名
		 -->
		<many-to-one name="customer" column="lkm_cust_id" class="Customer"  >
		</many-to-one>
	</class>
</hibernate-mapping>
```
**在这里指出many-to-one就可以了**

**这里的联系人表property只有name没有指明column，不知道为什么，不知道是一定不能指明还是指明不指明都可以**

4. 在hibernate.cfg.xml中把两个hbm.xml的位置指出来。  
```xml
<mapping resource="cn/itheima/domain/Customer.hbm.xml" />
<mapping resource="cn/itheima/domain/LinkMan.hbm.xml"/>
```

5. 代码操作  


```java
@Test
//保存客户 以及客户 下的联系人
public void fun1(){
  Session session = HibernateUtils.openSession();
  Transaction transaction = session.beginTransaction();
  
  //1.客户创建对象(一个)
  Customer c= new Customer();
  c.setCust_name("xxx");
  //2.创建联系人对象（多个）
  LinkMan linkMan1 = new LinkMan();
  linkMan1.setLkm_name("111");
  LinkMan linkMan2 = new LinkMan();
  linkMan2.setLkm_name("222");
  LinkMan linkMan3 = new LinkMan();
  linkMan3.setLkm_name("333");
  //相互绑定
  c.getLinkMens().add(linkMan1);
  c.getLinkMens().add(linkMan2);
  c.getLinkMens().add(linkMan3);
  //相互绑定
  linkMan1.setCustomer(c);
  linkMan2.setCustomer(c);
  linkMan3.setCustomer(c);
  
  
  session.save(c);
  session.save(linkMan1);
  session.save(linkMan2);
  session.save(linkMan3);
  
  transaction.commit();
  session.close();
}
```

```java
@Test
//为客户增加联系人
public void fun2(){
  //1 获得session
  Session session = HibernateUtils.openSession();
  //2 开启事务
  Transaction tx = session.beginTransaction();
  //-------------------------------------------------
  //3操作
  //1> 获得要操作的客户对象
  Customer c = session.get(Customer.class,1l);
  //2> 创建联系人
  LinkMan lm1 = new LinkMan();
  lm1.setLkm_name("郝强勇");
  //3> 将联系人添加到客户,将客户设置到联系人中
  c.getLinkMens().add(lm1);
  lm1.setCustomer(c);
  //4> 执行保存
  session.save(lm1);
  //-------------------------------------------------
  //4提交事务
  tx.commit();
  //5关闭资源
  session.close();
}
```

```java
@Test
//为客户删除联系人
public void fun3(){
  //1 获得session
  Session session = HibernateUtils.openSession();
  //2 开启事务
  Transaction tx = session.beginTransaction();
  //-------------------------------------------------
  //3操作
  //1> 获得要操作的客户对象
  Customer c = session.get(Customer.class,1l);
  //2> 获得要移除的联系人
  LinkMan lm = session.get(LinkMan.class, 3l);
  //3> 将联系人从客户集合中移除
  c.getLinkMens().remove(lm);
  lm.setCustomer(null);
  //-------------------------------------------------
  //4提交事务
  tx.commit();
  //5关闭资源
  session.close();
}
```


6. 小总结一波  
这种一对多和单个表的不同地方在于：
- bean类中需要加入对方类型的属性，不然后面没法绑定。
- hbm.xml中要指出一些一对多，多对一的信息。
- hibernate.cfg.xml要把所有的hbm.xml指明出来
- 代码中也要相互绑定。

## 级联操作
这可以简化操作。  
比如原来保存是这样的。客户和联系人需要单独保存。

    session.save(c);  
    session.save(linkMan1);  
    session.save(linkMan2);  
    session.save(linkMan3);  

有了级联操作以后，就可以只保存客户，和客户挂钩的三个联系人会自动保存。
    
    session.save(c);//一句就可以
    
在hbm.xml中配置cascade就可以了。  
比如：  
```xml
<set name="linkMens" cascade="delete"  >
  <key column="lkm_cust_id" ></key>
  <one-to-many class="LinkMan" />
</set>
```
cascade有四个属性值：
- delete：级联删除，删除一个就删除另一个
- save-update: 级联保存更新，保存一个也保存另一个
- all:save-update+delete
- none：默认值，不级联

> 两个hbm.xml都可以配置，也可以只配置一个。比如配置了客户表的save-update，那么保存客户时，客户的联系人也保存了，配置联系人的hbm.xml时，保存联系人时，客户也保存了，两个都配置时，保存任意一个另一个也保存了。

> **级联操作的delete有一个逻辑问题，最好不要用**，*比如一个客户有三个联系人，我想删除联系人a，但是因为delete的配置效果，会把a对应的客户也删除了，这是不符合预期的，因为联系人b和c还在呢。如果客户也配置了delete的话，客户删除的时候还会把b和c也删除了，这就太不符合预期了。*

## 外键关系维护  
在默认情况下，一对多中，两个表都会维护这个外键关系。而这个关系其实只要一方维护就可以。在“一”的那一方可以放弃维护，“多”的那一方是不能放弃的。  

“一”的那一方放弃的方法就是在hbm.xml中配置inverse="true"。true 表示放弃维护，默认是false。这个配置的位置在：  
```xml
<set name="linkMens" inverse="true">
  <key column="lkm_cust_id" ></key>
  <one-to-many class="LinkMan" />
</set>
```

>减少一方维护关系可以优化性能

>要么两个都维护关系，要么只“多”的一方维护关系（因为外键字段就在“多”的一方）。  


# 多对多
多对多大同小异，两个表都有外键，多对多会引入一个中间表。

注意点：  
- 在两个表的bean类中都要加入set集合，以及set集合的get，set方法。  

  比如在类A里面有：  
  ```private Set<B> users = new HashSet<B>();```  
  在类B里面有：  
    ```private Set<A> users = new HashSet<A>();```

- 两个表的hbm.xml配置：  
比如User：  
```xml
<set name="roles" table="sys_user_role" cascade="save-update" >
  <key column="user_id" ></key>
  <many-to-many class="Role" column="role_id" ></many-to-many>
</set>
```
>name:bean类中的set集合属性的名称  
table：中间表的名字，自己取名，两个xml一致即可。  
column：我的外键名  
class：和我多对多对应的那个类   
column：和我多对多的那个外键  

代码使用：  
```java
//多对多关系操作
public class Demo {
	@Test
	//保存员工以及角色
	public void fun1(){
		//1 获得session
		Session session = HibernateUtils.openSession();
		//2 开启事务
		Transaction tx = session.beginTransaction();
		//-------------------------------------------------
		//3操作
		//1> 创建两个 User
		User u1 = new User();
		u1.setUser_name("郝强勇");
		
		User u2 = new User();
		u2.setUser_name("金家德");
		
		//2> 创建两个 Role
		Role r1 = new Role();
		r1.setRole_name("保洁");
		
		Role r2 = new Role();
		r2.setRole_name("保安");
		//3> 用户表达关系
		u1.getRoles().add(r1);
		u1.getRoles().add(r2);
		
		u2.getRoles().add(r1);
		u2.getRoles().add(r2);
		
		//4> 角色表达关系
		r1.getUsers().add(u1);
		r1.getUsers().add(u2);
		
		r2.getUsers().add(u1);
		r2.getUsers().add(u2);
		
		//5> 调用Save方法一次保存
		session.save(u1);
		session.save(u2);
		session.save(r1);
		session.save(r2);
		//-------------------------------------------------
		//4提交事务
		tx.commit();
		//5关闭资源
		session.close();
	}
	
```
>**注意：**
这里如果两方都表达关系，则需要在配置hbm.xml中有一方放弃维护，或者是两方都维护关系，但是上述代码只有一方表达关系，也就是上面的代码第三步和第四步只留其一。

>**结论：**
以后直接放弃一方维护关系，具体放弃哪一方要根据逻辑自己判断。


```java
@Test
	//为郝强勇新增一个角色
	public void fun3(){
		//1 获得session
		Session session = HibernateUtils.openSession();
		//2 开启事务
		Transaction tx = session.beginTransaction();
		//-------------------------------------------------
		//3操作
		//1> 获得郝强勇用户
		User user = session.get(User.class, 1l);
		//2> 创建公关角色
		Role r = new Role();
		r.setRole_name("男公关");
		//3> 将角色添加到用户中
		user.getRoles().add(r);
		//4> 将角色转换为持久化
		session.save(r);//user类如果使用了级联保存这句代码可以省略。
		//-------------------------------------------------
		//4提交事务
		tx.commit();
		//5关闭资源
		session.close();
	}
	
```

```java

@Test
//为郝强勇解除一个角色
public void fun4(){
  //1 获得session
  Session session = HibernateUtils.openSession();
  //2 开启事务
  Transaction tx = session.beginTransaction();
  //-------------------------------------------------
  //3操作
  //1> 获得郝强勇用户
  User user = session.get(User.class, 1l);
  //2> 获得要操作的角色对象(保洁,保安)
  Role r1 = session.get(Role.class, 1l);
  Role r2 = session.get(Role.class, 2l);
  //3> 将角色从用户的角色集合中移除
  user.getRoles().remove(r1);
  user.getRoles().remove(r2);
  
  //-------------------------------------------------
  //4提交事务
  tx.commit();
  //5关闭资源
  session.close();
}
}
```

> 多对多也有cascade级联操作，这里更不能用delete。
