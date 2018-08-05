---
layout:     post
title:      hibernate_基本
subtitle:   用法
date:       2018-05-23
author:     ZL
header-img: img/20180523.jpg
catalog: 	 true
tags:
    - hibernate
---

# 介绍
hibernate是一个关于数据库的框架。
## 好处
完全面向对象操作数据库
操作数据库的时候,可以以面向对象的方式来完成.不需要书写SQL语句

---

# 搭建demo
## 步骤
1. 下载hibernate
https://sourceforge.net/projects/hibernate/files/hibernate-orm/5.0.7.Final/  
下载解压后有lib文件夹，下面有required文件夹，里面有必须的jar包。

2. 创建一个数据库
![](http://ovoxjpcrm.bkt.clouddn.com/2fda42850de38562f0cc1078d5f70a82.png)

3. 新建javaweb项目并导包
![](http://ovoxjpcrm.bkt.clouddn.com/8d8d3ca9e11e67d78e8e45e9ae9db86a.png)
![](http://ovoxjpcrm.bkt.clouddn.com/ad9dace1d5c74dd331007affed2c19a4.png)

4. 创建一个bean类，用于保存数据

    数据库有什么字段，就创建对应的变量，然后添加set，get方法。

    ```java
    package domain;

    public class Customer {

     private Long cust_id;

     private String cust_name;
     private String cust_source;
     private String cust_industry;
     private String cust_level;
     private String cust_linkman;
     private String cust_phone;
     private String cust_mobile;
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

5. 在bean类customer的统计目录下，创建Customer.hbm.xml

    >这个文件的作用是一个映射的作用，将数据库中的内容和bean类customer对应起来。

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
        <!-- 这部分是约束内容 -->



       <!-- 配置表与实体对象的关系 -->
       <!-- package属性:填写一个包名.在元素内部凡是需要书写完整类名的属性,可以直接写简答类名了. -->
    <hibernate-mapping package="domain" >


    	<!--
    		class元素: 配置实体与表的对应关系的
    			name: 完整类名
    			table:数据库表名
    	 -->
    	<class name="Customer" table="cst_customer" >



    		<!-- id元素:配置主键映射的属性
    				name: 填写主键对应属性名
    				column(可选): 填写表中的主键列名.默认值:列名会默认使用属性名
    				type(可选):填写列(属性)的类型.hibernate会自动检测实体的属性类型.
    						每个类型有三种填法: java类型|hibernate类型|数据库类型
    				not-null(可选):配置该属性(列)是否不能为空. 默认值:false
    				length(可选):配置数据库中列的长度. 默认值:使用数据库类型的最大长度
    		 -->
    		<id name="cust_id"  >
    			<!-- generator:主键生成策略
                后面介绍生成策略
         -->
    			<generator class="native"></generator>
    		</id>




    		<!-- property元素:除id之外的普通属性映射
    				name: 填写属性名
    				column(可选): 填写列名
    				type(可选):填写列(属性)的类型.hibernate会自动检测实体的属性类型.
    						每个类型有三种填法: java类型|hibernate类型|数据库类型
    				not-null(可选):配置该属性(列)是否不能为空. 默认值:false
    				length(可选):配置数据库中列的长度. 默认值:使用数据库类型的最大长度
    		 -->
    		<property name="cust_name" column="cust_name" >
    			<!--  <column name="cust_name" sql-type="varchar" ></column> -->
    		</property>
    		<property name="cust_source" column="cust_source" ></property>
    		<property name="cust_industry" column="cust_industry" ></property>
    		<property name="cust_level" column="cust_level" ></property>
    		<property name="cust_linkman" column="cust_linkman" ></property>
    		<property name="cust_phone" column="cust_phone" ></property>
    		<property name="cust_mobile" column="cust_mobile" ></property>
    	</class>
    </hibernate-mapping>
    ```

6. 书写主配置文件

    > 在src目录下创建hibernate.cfg.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE hibernate-configuration PUBLIC
    	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
    <hibernate-configuration>

    	<session-factory>

    		 <!-- 数据库驱动 -->
    		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
    		 <!-- 数据库url -->
    		<property name="hibernate.connection.url">jdbc:mysql:///test01</property>
    		 <!-- 数据库连接用户名 -->
    		<property name="hibernate.connection.username">root</property>
    		 <!-- 数据库连接密码 -->
    		<property name="hibernate.connection.password">123456</property>



    		<!-- 数据库方言
    			不同的数据库中,sql语法略有区别. 指定方言可以让hibernate框架在生成sql语句时.针对数据库的方言生成.
    			sql99标准: DDL 定义语言  库表的增删改查
    					  DCL 控制语言  事务 权限
    					  DML 操纵语言  增删改查
    			注意: MYSQL在选择方言时,请选择最短的方言.
    		 -->

    		<!--
    		#hibernate.dialect org.hibernate.dialect.MySQLDialect
    		#hibernate.dialect org.hibernate.dialect.MySQLInnoDBDialect
    		#hibernate.dialect org.hibernate.dialect.MySQLMyISAMDialect
    		 -->
    		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>




    		<!-- 将hibernate生成的sql语句打印到控制台 -->
    		<property name="hibernate.show_sql">true</property>
    		<!-- 将hibernate生成的sql语句格式化(语法缩进) -->
    		<property name="hibernate.format_sql">true</property>



    		<!--
    		## auto schema export  自动导出表结构. 自动建表
    		#hibernate.hbm2ddl.auto create		自动建表.每次框架运行都会创建新的表.以前表将会被覆盖,表数据会丢失.(开发环境中测试使用)
    		#hibernate.hbm2ddl.auto create-drop 自动建表.每次框架运行结束都会将所有表删除.(开发环境中测试使用)
    		#hibernate.hbm2ddl.auto update(推荐使用) 自动生成表.如果已经存在不会再生成.如果表有变动.自动更新表(不会删除任何数据).
    		#hibernate.hbm2ddl.auto validate	校验.不自动生成表.每次启动会校验数据库中表是否正确.校验失败.
    		 -->
    		<property name="hibernate.hbm2ddl.auto">update</property>



    		<!-- 引入orm元数据
    			路径书写: 填写src下的路径
    		 -->
    		<mapping resource="domain/Customer.hbm.xml" />

    	</session-factory>
    </hibernate-configuration>
    ```


7. 代码测试（往数据库中插入一条数据）
    ```java
    package domain;

    import org.hibernate.Session;
    import org.hibernate.SessionFactory;
    import org.hibernate.Transaction;
    import org.hibernate.cfg.Configuration;
    import org.junit.Test;

    import domain.Customer;

    public class Demo {

    	@Test
    	public void fun1(){
    		Configuration conf = new Configuration().configure();

    		SessionFactory sessionFactory = conf.buildSessionFactory();

    		Session session = sessionFactory.openSession();

    		Transaction tx = session.beginTransaction();

    		//----------------------------------------------
    		Customer c = new Customer();
    		c.setCust_name("google");

    		session.save(c);

    		//----------------------------------------------
    		tx.commit();
    		session.close();
    		sessionFactory.close();
    	}
    }

    ```

## 注意
如果本地配置的是java9，那么需要在额外导入几个包。这几个包新版本java9没有。
![](http://ovoxjpcrm.bkt.clouddn.com/a68266982ec5ecaa549bb39b29cc619c.png)

---

# hibernate的API介绍
## 示例：
```java
//事务操作
public void fun1(){
  //1 创建,调用空参构造
  Configuration conf = new Configuration().configure();
  //2 根据配置信息,创建 SessionFactory对象
  SessionFactory sf = conf.buildSessionFactory();
  //3 获得session
  Session session = sf.openSession();
  //4 session获得操作事务的Transaction对象
  //开启事务并获得操作事务的tx对象(建议使用)
  Transaction tx2 = session.beginTransaction();
  //----------------------------------------------
  增删改查操作
  //----------------------------------------------
  tx2.commit();//提交事务
  tx2.rollback();//回滚事务
  session.close();//释放资源
  sf.close();//释放资源
}
```

## Configuration类
两个作用：
- 加载主配置src下的hibernate.cfg.xml文件
    ```
    Configuration conf = new Configuration();
		conf.configure();
    ```
- 创建 SessionFactory对象  
    ```SessionFactory sf = conf.buildSessionFactory();```


## SessionFactory类
作用：  
创建session对象  
```Session session = sf.openSession();```

注意：
- sessionfactory 负责保存和使用所有配置信息.消耗内存资源非常大.
- sessionFactory属于线程安全的对象设计.

>所以保证在web项目中,只创建一个sessionFactory.


## Session类
>最核心的东西，增删改查都靠它。

作用：
- 获取事物：
  - 方式一：
    ```
    Transaction tx = session.getTransaction();
    tx.begin();
    ```
  - 方式二：  
    ```Transaction tx2 = session.beginTransaction();```


- 增删改查(这里是最简单的的增删改查)
1. 增  
![](http://ovoxjpcrm.bkt.clouddn.com/8037abb28dedbba591c4305ad2d8526e.png)
2. 删（删除id为1的数据，第二个参数是id，类型是long） 
![](http://ovoxjpcrm.bkt.clouddn.com/984c6a54c9ae707f60088486c42d7095.png)
3. 改  
![](http://ovoxjpcrm.bkt.clouddn.com/01033b918fa6427b3605fc44f2d5d20d.png)
4. 查  
![](http://ovoxjpcrm.bkt.clouddn.com/35e4f614a8f9937a5249a0913ff9b8da.png)


## Transaction类
作用：事物

获取方式：
- 方式一：
  ```
  Transaction tx = session.getTransaction();
  tx.begin();
  ```
- 方式二：  
  ```Transaction tx2 = session.beginTransaction();```

提交：  
```tx2.commit();//提交事务```

回滚：  
```tx2.rollback();//回滚事务```


---


# HibernateUtils
增删改查有一些重复代码，按照Java的一贯作风，肯定是要封装的。  

这里提供一个封装的代码：  
```java
package cn.itheima.utils;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtils {
	private static SessionFactory sf;
	
	static{
		//1 创建,调用空参构造
		Configuration conf = new Configuration().configure();
		//2 根据配置信息,创建 SessionFactory对象
		 sf = conf.buildSessionFactory();
	}
	
	//获得session => 获得全新session
	public static Session openSession(){
				//3 获得session
				Session session = sf.openSession();
				
				return session;
		
	}
	//获得session => 获得与线程绑定的session
	public static Session getCurrentSession(){
		//3 获得session
		Session session = sf.getCurrentSession();
		
		return session;
	}
	public static void main(String[] args) {
		System.out.println(HibernateUtils.openSession());
	}
	
}

```
使用：  
```java
public void save(Customer c) {
		//1 获得session
		Session session = HibernateUtils.openSession();
		//2 打开事务
		Transaction tx = session.beginTransaction();
		//3 执行保存
		session.save(c);
		//4 提交事务
		tx.commit();
		//5 关闭资源
		session.close();
		
	}
```

---

# hibernate中的实体规则（就是上面的bean类customer的注意事项）  
1. 持久化类提供无参数构造（默认不写就是）
2. 成员变量私有,提供共有get/set方法访问（bean类都这样子的呀）
3. 持久化类中的属性,应尽量使用包装类型（就是说用Double，Long，Integer而不是double，long，int）
4. 持久化类需要提供oid.与数据库中的主键列对应（就是说数据库中的主键在bean类中一定要有属性对应，没有主键的数据库不能使用hibernate）
5. 不要用final修饰class（就是说Customer不要用final修饰）

---

# 关于主键  
> 在 Customer.hbm.xml中有配置主键生成策略。

```xml
<id name="cust_id"  >
  <!-- generator:主键生成策略-->
  <generator class="native"></generator>
</id>
```

> 这里为了介绍主键生成策略就把主键也全部介绍一下。

## 主键要求
主键要求一个不能为空，必须要有，二个主键不能重复。


## 主键分类
- 如果一个表里面有一个列，他的值就满足不为空且不重复，比如身份证号码，然后就以这个列为主键的话，那么这种主键就叫做**自然主键**
- 多数时候会在表里面创建一个没有意义的列，这个列满足主键的条件，以这个列为主键，这种主键就叫做**代理主键**

## 主键生成策略（对代理主键而言）  

自然主键：assigned
> 这个是表中的一个列，具体什么值就录入什么值。

代理主键：（并不需要录入，自动生成，所以会有生成策略）
1. identity : 
    > 主键自增.由数据库来维护主键值.录入时不需要指定主键.
2. sequence: 
    > Oracle中的主键生成策略.
3. increment(了解): 
    > 主键自增.由hibernate来维护.每次插入前会先查询表中id最大值.+1作为新主键值.			
4. hilo(了解): 
    > 高低位算法.主键自增.由hibernate来维护.开发时不使用.
5. native:
    > hilo+sequence+identity 自动三选一策略.
6. uuid: 
    > 产生随机字符串作为主键. 主键类型必须为string 类型.
7. assigned:
    > 自然主键生成策略（就是没有策略）,hibernate不会管理主键，由开发人员自己录入。


---


# hibernate中的对象状态  
## 三种状态  
> 顺势状态  

*没有id,没有在session缓存中*，在内存中是孤立存在的，与数据库中的数据没有任何关联。

> 持久化状态

*有id,在session缓存中*

> 游离|托管状态

*有id,没有在session缓存中*

*这里的有id是指有与数据中对应的id，当调用save方法以后就在数据库中有了id*

## 例子
```java
public void fun1(){
		Configuration conf = new Configuration().configure();
		
		SessionFactory sessionFactory = conf.buildSessionFactory();
		
		Session session = sessionFactory.openSession();
	
		Transaction tx = session.beginTransaction();
		//----------------------------------------------
		Customer c = new Customer();//瞬时状态
		c.setCust_name("google公司");
		
		session.save(c);//持久化状态，有id,在session缓存中
		
		//----------------------------------------------
		tx.commit();//游离|托管状态
		session.close();
		sessionFactory.close();
	}
```

## 三种状态的转换  
![](http://ovoxjpcrm.bkt.clouddn.com/8805891c7f1fdc983d43a33997b64baa.png) 



---


# hibernate缓存  

> hibernate会有缓存的，不过这个理解一下就好，因为在写代码的时候并没有什么不一样。  

> hibernate的一级缓存的作用就是减少对数据库的访问次数。  

hibernate的一级缓存有如下特点：  
1. 当应用程序调用Session接口的save()、update()、saveOrUpdate()时、如果Session缓存中没有相应的对象，hibernate就会自动的把从数据库中查询到的相应对象信息加入到一级缓存中去。  
2. 当调用Session接口的load()、get()方法以及Query接口的list()、iterator()方法时，会判断缓存中是否存在该对象，有则返回，不会查询数据库，如果缓存中没有要查询对象，再去数据库中查询相应对象，并添加到一级缓存中。
3. 当调用session的close()方法时，session缓存会被清空。

![](http://ovoxjpcrm.bkt.clouddn.com/506c79307891348349718fd41d5e2af4.png) 

![](http://ovoxjpcrm.bkt.clouddn.com/865b08b096acb7641a089c9a3964c548.png) 


比如：  
```
Customer c1 = session.get(Customer,1l);
Customer c2 = session.get(Customer,1l);
```

这两句代码实际上只是查询了一次数据库。

---

# hibernate指定事务的隔离级别

> 在数据库的事务的那篇博客中有关于数据库的事务的特性和隔离级别的介绍，这里就不再多加介绍了，就介绍一下如何在hibernate中指定数据库的隔离级别  

- read uncommitted : 读取尚未提交的数据 ：哪个问题都不能解决  
- read committed：读取已经提交的数据 ：可以解决脏读 ---- oracle默认的  
- repeatable read：重复读取：可以解决脏读 和 不可重复读 ---**mysql默认的**  
- serializable：串行化：可以解决 脏读 不可重复读 和 虚读---相当于锁表  

在hibernate.cfg.xml中  
![](http://ovoxjpcrm.bkt.clouddn.com/4ffab2df65dea3cecfa8bda45760dd09.png) 

---

# 在项目中确保使用同一个Session的例子：  

> 在hibernate中,确保使用同一个session的问题,hibernate已经帮我们解决了. 我们开发人员只需要调用sf.getCurrentSession()方法即可获得与当前线程绑定的session对象  

注意：调用getCurrentSession方法必须配合主配置中的一段配置  
![](http://ovoxjpcrm.bkt.clouddn.com/2f799dcb400550fe2aceac9e2cc57e01.png) 

注意2:通过getCurrentSession方法获得的session对象.当事务提交时,session会自动关闭.不要手动调用close关闭.

service层：  
![](http://ovoxjpcrm.bkt.clouddn.com/072990d1034a73996ba2a535e57c5de5.png) 


dao层：  
![](http://ovoxjpcrm.bkt.clouddn.com/23cce90147676048bb437a325062e382.png) 

---

# hibernate中的批量查询(概述)

> HQL： 多表查询，但不复杂时使用  
criteria：单表条件查询  
原生SQL：复杂业务查询  

## HQL查询  
> Hibernate独家查询语言,属于面向对象的查询语言  

1. 基本查询（查询所有记录）  
    ```java
    //基本查询
    public void fun1(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1> 书写HQL语句
    //		String hql = " from cn.itheima.domain.Customer ";
      String hql = " from Customer "; // 查询所有Customer对象
      //2> 根据HQL语句创建查询对象
      Query query = session.createQuery(hql);
      //3> 根据查询对象获得查询结果
      List<Customer> list = query.list();	// 返回list结果
      //query.uniqueResult();//接收唯一的查询结果
      
      System.out.println(list);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

    > 如果查询的结果只有一个：query.uniqueResult();  
    如果查询的结果是一个集合：query.list();  


2. 条件查询1.0  
    ```java
    public void fun2(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1> 书写HQL语句
      String hql = " from Customer where cust_id = 1 ";
      //2> 根据HQL语句创建查询对象
      Query query = session.createQuery(hql);
      //3> 根据查询对象获得查询结果
      Customer c = (Customer) query.uniqueResult();
      
      System.out.println(c);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

    > from Customer where cust_id = 1中的cust_id是Customer类中的属性的名字，而不是数据库中cust_id字段的名字。HQL语句中,不可能出现任何数据库相关的信息的  

3. 条件查询2.0（问号占位符）

    ```java
    public void fun3(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1> 书写HQL语句
      String hql = " from Customer where cust_id = ? "; 
      //2> 根据HQL语句创建查询对象
      Query query = session.createQuery(hql);
      //设置参数
      //query.setLong(0, 1l);
      query.setParameter(0, 1l);
      //3> 根据查询对象获得查询结果
      Customer c = (Customer) query.uniqueResult();
      
      System.out.println(c);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

    > 设置参数的两种写法：  
    
    - query.setLong、setboolean、setDouble等等，根据问号占位符的数据类型而选择，参数1：第几个问号，从0开始计数；参数二：该问号处的值。  
    - query.setParameter(0, 1l);这种不用区分问号占位符的数据类型了，参数1：第几个问号，从0开始计数；参数二：该问号处的值。  


4. 条件查询3.0（命令占位符）
    ```java
    public void fun4(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1> 书写HQL语句
      String hql = " from Customer where cust_id = :aaa and cust_name = :bbb"; 
      //2> 根据HQL语句创建查询对象
      Query query = session.createQuery(hql);
      //设置参数
      query.setParameter("aaa", 1l);
      query.setParameter("bbb", "张三");
      //3> 根据查询对象获得查询结果
      Customer c = (Customer) query.uniqueResult();
      
      System.out.println(c);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

    > 用aaa，bbb取代了问号占位符，这样在设置参数时就不用去数第几个问号了。

5. 分页查询  
    ```java
    public void fun5(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1> 书写HQL语句
      String hql = " from Customer  ";
      //2> 根据HQL语句创建查询对象
      Query query = session.createQuery(hql);
      //设置分页信息 limit ?,?
      query.setFirstResult(1);
      query.setMaxResults(1);
      //3> 根据查询对象获得查询结果
      List<Customer> list =  query.list();
      
      System.out.println(list);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```
    > query.setFirstResult(1);从第几条记录开始查询，默认0；  
    query.setMaxResults(1);查询几条数据。  
    这两个方法对应了sql语句中limit ?,?的两个问号。  


## Hibernate无语句面向对象查询

1. 基本查询（查询所有）  
    ```java
    public void fun1(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      
      //查询所有的Customer对象
      Criteria criteria = session.createCriteria(Customer.class);
      
      List<Customer> list = criteria.list();
      
      System.out.println(list);
      
    //Customer c = (Customer) criteria.uniqueResult();
      
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

2. 条件查询  
    ```java
    public void fun2(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //创建criteria查询对象
      Criteria criteria = session.createCriteria(Customer.class);
      //添加查询参数 => 查询cust_id为1的Customer对象
      criteria.add(Restrictions.eq("cust_id", 1l));
      //执行查询获得结果
      Customer c = (Customer) criteria.uniqueResult();
      System.out.println(c);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

    | sql条件     | Restrictions.xx |
    | ----------- | :---------------: |
    | >           | gt                |
    | >=          | ge                |
    | <           | lt                |
    | <=          | le                |
    | ==          | eq                |
    | !=          | ne                |
    | in          | in                |
    | between and | between           |
    | like        | like              |
    | is not null | isNotNull         |
    | is null     | isNull            |
    | or          | or                |
    | and         | and               |
	

3. 分页查询  
    ```java
    public void fun3(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //创建criteria查询对象
      Criteria criteria = session.createCriteria(Customer.class);
      //设置分页信息 limit ?,?
      criteria.setFirstResult(1);
      criteria.setMaxResults(2);
      //执行查询
      List<Customer> list = criteria.list();
      
      System.out.println(list);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

4. 查询总记录数  
    ```java
    public void fun4(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //创建criteria查询对象
      Criteria criteria = session.createCriteria(Customer.class);
      //设置查询的聚合函数 => 总行数
      criteria.setProjection(Projections.rowCount());
      //执行查询
      Long count = (Long) criteria.uniqueResult();
      
      System.out.println(count);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```
    

## 原生SQL查询  

1. 基本查询（查询所有）
    ```java
    public void fun2(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1 书写sql语句
      String sql = "select * from cst_customer";
      
      //2 创建sql查询对象
      SQLQuery query = session.createSQLQuery(sql);
      //指定将结果集封装到哪个对象中
      query.addEntity(Customer.class);
      
      //3 调用方法查询结果
      List<Customer> list = query.list();
      
      System.out.println(list);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

2. 条件查询
    ```java
    public void fun3(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1 书写sql语句
      String sql = "select * from cst_customer where cust_id = ? ";
      
      //2 创建sql查询对象
      SQLQuery query = session.createSQLQuery(sql);
      
      query.setParameter(0, 1l);
      //指定将结果集封装到哪个对象中
      query.addEntity(Customer.class);
      
      //3 调用方法查询结果
      List<Customer> list = query.list();
      
      System.out.println(list);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```

3. 分页查询
    ```java
    public void fun4(){
      //1 获得session
      Session session = HibernateUtils.openSession();
      //2 控制事务
      Transaction tx = session.beginTransaction();
      //3执行操作
      //-------------------------------------------
      //1 书写sql语句
      String sql = "select * from cst_customer  limit ?,? ";
      
      //2 创建sql查询对象
      SQLQuery query = session.createSQLQuery(sql);
      
      query.setParameter(0, 0);
      query.setParameter(1, 1);
      //指定将结果集封装到哪个对象中
      query.addEntity(Customer.class);
      
      //3 调用方法查询结果
      List<Customer> list = query.list();
      
      System.out.println(list);
      //-------------------------------------------
      //4提交事务.关闭资源
      tx.commit();
      session.close();// 游离|托管 状态, 有id , 没有关联
    }
    ```
