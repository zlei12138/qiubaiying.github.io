---
layout:     post
title:      hibernate_HQL、criteria更详细的操作+查询优化
subtitle:   HQL、criteria更详细的操作+查询优化
date:       2018-05-27
author:     ZL
header-img: img/20180527.jpg
catalog: 	 true
tags:
    - hibernate
    - HQL
    - Criteria
    - 查询优化
---

# HQL  

## 基本语法  

```java
//基本语法
@Test
public void fun1(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql = " from  cn.itcast.domain.Customer ";//完整写法
  String hql2 = " from  Customer "; //简单写法
  String hql3 = " from java.lang.Object "; //扩展
  
  Query query = session.createQuery(hql2);
  
  List list = query.list();
  
  System.out.println(list);
  //----------------------------------------------------
  tx.commit();
  session.close();  
}  
```
> String hql3 = " from java.lang.Object ";  
这种是扩展写法是按映射类的类型查，没什么用，就是把所有的表都查一下，因为所有的表的映射类都是Object  

## 排序  

```java
@Test
//排序
public void fun2(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql1 = " from  cn.itcast.domain.Customer order by cust_id asc ";
  String hql2 = " from  cn.itcast.domain.Customer order by cust_id desc ";
  
  Query query = session.createQuery(hql2);
  
  List list = query.list();
  
  System.out.println(list);
  //----------------------------------------------------
  tx.commit();
  session.close();

}
```
>这里的cust_id是Customer类的属性，不是数据库表的字段，HQL的原则就是不会出现数据库中的东西。

## 条件查询  

```java
@Test
//条件查询
public void fun3(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql1 = " from  cn.itcast.domain.Customer where cust_id =? ";
  String hql2 = " from  cn.itcast.domain.Customer where cust_id = :id ";
  
  Query query = session.createQuery(hql2);
  
//		query.setParameter(0, 2l);
  query.setParameter("id", 2l);
  
  
  List list = query.list();
  
  System.out.println(list);
  //----------------------------------------------------
  tx.commit();
  session.close();
  
}
```
> 如果不知道":id"是什么，可以去看hibernate一的博客里面关于hql查询的内容。


## 分页查询  

```java
@Test
//分页查询
public void fun4(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql1 = " from  cn.itcast.domain.Customer  ";//完整写法
  
  Query query = session.createQuery(hql1);
  
  //limit ?,?
  // (当前页数-1)*每页条数
  query.setFirstResult(2);
  query.setMaxResults(2);
  
  List list = query.list();
  
  System.out.println(list);
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```

## 统计查询  
  
```java
@Test
//统计查询
//count	计数
//sum 	求和
//avg	平均数
//max
//min
public void fun5(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql1 = " select count(*) from  cn.itcast.domain.Customer  ";//完整写法
  String hql2 = " select sum(cust_id) from  cn.itcast.domain.Customer  ";//完整写法
  String hql3 = " select avg(cust_id) from  cn.itcast.domain.Customer  ";//完整写法
  String hql4 = " select max(cust_id) from  cn.itcast.domain.Customer  ";//完整写法
  String hql5 = " select min(cust_id) from  cn.itcast.domain.Customer  ";//完整写法
  
  Query query = session.createQuery(hql5);
  
  //Number是一个java类是Float、Integer、Double等的父类
  Number number  = (Number) query.uniqueResult();
  
  System.out.println(number);
  //----------------------------------------------------
  tx.commit();
  session.close();  
}
```

## 投影查询  

```java
@Test
//投影查询
public void fun6(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql1 = " select cust_name from  cn.itcast.domain.Customer  ";
  String hql2 = " select cust_name,cust_id from  cn.itcast.domain.Customer  ";
  String hql3 = " select new Customer(cust_id,cust_name) from  cn.itcast.domain.Customer  ";
  
  Query query = session.createQuery(hql3);
  
  List list = query.list();
  
  System.out.println(list);
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```
> 投影查询就是一条数据有多个字段，只查某些个字段。比如只查id字段或者只查id和name字段。hql2和hql3的不同地方在于，hql2返回的东西是List< Object[]>,hql3返回的是List< Customer>对象。


## HQL多表（不常用）

### 原生SQL的语法复习： 

![](http://ovoxjpcrm.bkt.clouddn.com/09c16240642bfae537f0ef334609dd2a.png)

1. 交叉连接-笛卡尔积(避免使用)  
```select * from A,B```  
2. 内连接  
    1. 隐式内连接  
      ```select * from A,B  where b.aid = a.id```
    2. 显式内连接
      ```select * from A inner join B on b.aid = a.id```
3. 外连接  
    1. 左外  
      ```select * from A left [outer] join B on b.aid = a.id```  
    2. 右外  
      ```select * from A right [outer] join B on b.aid = a.id```  

### HQL多表  

		内连接(普通内连接+迫切内连接)
		外连接
			|-左外(左外+左外迫切)
			|-右外(右外+右外迫切)

表内容展示：  
![](http://ovoxjpcrm.bkt.clouddn.com/e78995695e3eb69fd8fe795e408dfac5.png)  
> cst_customer是“一”，cst_linkman是“多”。customer1和customer6分别有三个linkman，customer2,3,4,5没有linkman。

### 内连接（customer2,3,4,5没有linkman所以不在查询结果之内）  
```java
@Test
//HQL 内连接 => 将连接的两端对象分别返回.放到数组中.
public void fun1(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql = " from Customer c inner join c.linkMens ";
  
  Query query = session.createQuery(hql);
  
  List<Object[]> list = query.list();
  
  for(Object[] arr : list){
    System.out.println(Arrays.toString(arr));
  }
  //----------------------------------------------------
  tx.commit();
  session.close();
}

```
查询结果：是一个Object数组，里面包含Customer类和Linkman类  
```
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@1e7f19b4]
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@235b4cb8]
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@4346808]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@712ac7e6]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@86d6bf7]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@2fbe26da]
```

### 迫切内连接  
```java
@Test
//HQL 迫切内连接 => 帮我们进行封装.返回值就是一个对象
public void fun2(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql = " from Customer c inner join fetch c.linkMens ";
  
  Query query = session.createQuery(hql);
  
  List<Customer> list = query.list();
  
  System.out.println(list);
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```
查询结果：一个Customer数组，只有Customer数组，Linkman被封装到Customer对象内部去了，可以通过customer.getLinkman得到。    
```java
[Customer [cust_id=1, cust_name=customer1], 
Customer [cust_id=1, cust_name=customer1],
Customer [cust_id=1, cust_name=customer1], 
Customer [cust_id=6, cust_name=customer6],
Customer [cust_id=6, cust_name=customer6], 
Customer [cust_id=6, cust_name=customer6]]
```

### 左外连接  
> 所有的“左”都会显示出来，这里“customer”是“左”，如果customer有对应的linkman，就把linkman也返回回来。没有就null。  

```java
@Test
//HQL 左外连接 => 将连接的两端对象分别返回.放到数组中.
public void fun3(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql = " from Customer c left join c.linkMens ";
  
  Query query = session.createQuery(hql);
  
  List<Object[]> list = query.list();
  
  for(Object[] arr : list){
    System.out.println(Arrays.toString(arr));
  }
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```  

查询结果：返回Object数组  

```
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@1e7f19b4]
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@235b4cb8]
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@4346808]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@712ac7e6]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@86d6bf7]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@2fbe26da]
[Customer [cust_id=2, cust_name=customer2], null]
[Customer [cust_id=3, cust_name=customer3], null]
[Customer [cust_id=4, cust_name=customer4], null]
[Customer [cust_id=5, cust_name=customer5], null]
```

### 右外连接

>所有的“右”都会显示出来，这里“linkman”是“右”，如果linkman有对应的customer，就把customer也返回回来。没有就null。

```java
@Test
//HQL 右外连接 => 将连接的两端对象分别返回.放到数组中.
public void fun4(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  String hql = " from Customer c right join c.linkMens ";
  
  Query query = session.createQuery(hql);
  
  List<Object[]> list = query.list();
  
  for(Object[] arr : list){
    System.out.println(Arrays.toString(arr));
  }
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```
查询结果：本例中所有的linkman都有对应的customer，所以没有null值。  
```
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@1e7f19b4]
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@235b4cb8]
[Customer [cust_id=1, cust_name=customer1], cn.itcast.domain.LinkMan@4346808]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@712ac7e6]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@86d6bf7]
[Customer [cust_id=6, cust_name=customer6], cn.itcast.domain.LinkMan@2fbe26da]
```


#  Criteria

## 基本查询  
```java
@Test
//基本语法
public void fun1(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  
  Criteria c = session.createCriteria(Customer.class);
  
  List<Customer> list = c.list();
  
  System.out.println(list);
  
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```

## 条件查询  
```java
@Test
//条件语法
public void fun2(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  
  Criteria c = session.createCriteria(Customer.class);
  
//		c.add(Restrictions.idEq(2l));
  c.add(Restrictions.eq("cust_id",2l));
  
  List<Customer> list = c.list();
  
  System.out.println(list);
  
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```

![](http://ovoxjpcrm.bkt.clouddn.com/951a1844ed30de8f3f3d0f2e3d9ca00d.png)
>Restrictions的其他规则内容也可以在hibernate第一篇文章中查看。

## 分页查询  
```java
@Test
//分页语法 - 与HQL一样
public void fun3(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  
  Criteria c = session.createCriteria(Customer.class);
  //limit ?,? 
  c.setFirstResult(0);
  c.setMaxResults(2);
  
  List<Customer> list = c.list();
  
  System.out.println(list);
  
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```

## 排序查询  
```java
@Test
//排序语法 
public void fun4(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  
  Criteria c = session.createCriteria(Customer.class);
  
  c.addOrder(Order.asc("cust_id"));
  //c.addOrder(Order.desc("cust_id"));
  
  List<Customer> list = c.list();
  
  System.out.println(list);
  
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```

## 统计查询  
```java
@Test
//统计语法 
public void fun5(){
  Session session = HibernateUtils.openSession();
  Transaction tx = session.beginTransaction();
  //----------------------------------------------------
  
  Criteria c = session.createCriteria(Customer.class);
  
  //设置查询目标
  c.setProjection(Projections.rowCount());
  
  List list = c.list();
  
  System.out.println(list);
  
  //----------------------------------------------------
  tx.commit();
  session.close();
}
```

## 离线Criteria查询  

> 首先解释一下。

> 正常的Criteria查询需要先获取session对象，然后通过session.createCriteria(Customer.class)获取到Criteria对象。  
离线查询就是不需要先获取session对象，可以先获取一个离线Criteria对象DetachedCriteria，然后再获取session对象，把session对象当作参数传进去就可以获取到Criteria对象了。  
说白了就是session对象和Criteria对象的获取先后顺序的问题。

> 这种的作用应该是在复杂的项目中，会把各种操作分离，设想一种情况，关于session对象的获取在一个模块中，但是在另一个模块里面又需要用到Criteria对象（这时暂时没办法或者很麻烦获取到session对象），那么这种离线Criteria就可以起到作用了。  

```java
@Test
	public void fun1(){
		//Service/web层
		DetachedCriteria dc  = DetachedCriteria.forClass(Customer.class);
		
		dc.add(Restrictions.idEq(6l));//拼装条件(全部与普通Criteria一致)
		
		//----------------------------------------------------
		Session session = HibernateUtils.openSession();
		Transaction tx = session.beginTransaction();
		//----------------------------------------------------
		Criteria c = dc.getExecutableCriteria(session);
		
		List list = c.list();
		
		System.out.println(list);
		//----------------------------------------------------
		tx.commit();
		session.close();
	}
```

# 查询优化  

## 类级别（load延时查询）

>Customer c = session.get(Customer.class, 2l);  
这里使用get就开始查询数据库了。  
>Customer c = session.load(Customer.class, 2l);  
这里其实并没有查询数据库，只有后面使用c的时候才真正开始查询数据库。

使用方式：  
1. 首先在Customer.hbm.xml中配置lazy。  
![](http://ovoxjpcrm.bkt.clouddn.com/b3fa12b295834a0ff260600724d56087.png)  
true（默认）：开启延时查询  
false：不开启，立刻查询(这时load方法和get方法就没有区别了)  

注意：注意:使用懒加载时要确保,调用属性加载数据时,session还是打开的.不然会抛出异常  

## 关联级别（没啥用）  
>关联级别是什么？
```java
Customer c = session.get(Customer.class, 2l);
Set<LinkMan> linkMens = c.getLinkMens();
```
上面的代码c是从数据库从查的，linkMens是通过c对象获取的，因为c和linkMens是关联的，这就是关联级别。  

>关联级别的影响属性  

在Customer.hbm.xml中  
![](http://ovoxjpcrm.bkt.clouddn.com/b1536561f6e980888d6f9d443197b68e.png)  
lazy：延时加载。  
  - true：默认，延迟加载  
  - false：立即加载  
  - extra：及其懒惰  

fetch：决定加载策略。  
  - select(默认值): 单表查询加载
  - join: 使用多表查询加载集合
  - subselect:使用子查询加载集合  

这两个属性的排列组合会产生不同的影响。  
结论:为了提高效率.fetch的选择上应选择select. lazy的取值应选择 true. 全部使用默认值.  


## 批量抓取（没啥用）  
![](http://ovoxjpcrm.bkt.clouddn.com/93b8251de0022fdc0767e362347952e7.png)
