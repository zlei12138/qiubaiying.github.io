---
layout:     post
title:      apache的DBUtils
subtitle:   例子demo增删改查
date:       2018-04-09
author:     ZL
header-img: img/20180409.jpg
catalog: 	 true
tags:
    - DBUtils
    - 数据库
---


**这是对JDBC的封装操作，主要作用是减少代码，结合连接池使用。**

**查询需要一个Bean类，从数据库中读取到的数据会存放在Bean类里面**

---

1. 首先导包  

   ![导包](http://ovoxjpcrm.bkt.clouddn.com/ccb3c75bdebca1ff2abd6e540311a328.png)  
   第一个：C3P0连接池的包  
   第二个+第四个：DBCP连接池的包  
   第三个：DBUtils的包  
   第五个：连接数据库的包  
   此外还需要单元测试的包junit-4.12t+hamcrest-core-1.3  
   >两个连接池DBCP和C3P0二选一即可    

   ---

2. 利用DBUtils增删改  

    ```java

    import java.sql.SQLException;

    import org.apache.commons.dbutils.QueryRunner;
    import org.junit.Test;

    import cn.itheima.jdbc.utils.C3P0Utils;

    /**
     * 测试DBUtils工具类的增删改操作
     * 
     * @author Never Say Never
     * @date 2016年7月31日
     * @version V1.0
     */
    public class TestDBUtils1 {

    	/**
    	 * 添加所有用户方法
    	 */
    	@Test
    	public void testAddUser() {
    		try {
    			// 1.创建核心类QueryRunner
          //需要传入一个连接池，DBCP或者C3P0都可以
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写SQL语句
    			String sql = "insert into tbl_user values(null,?,?)";
    			// 3.为站位符设置值
    			Object[] params = { "余淮", "耿耿" };
    			// 4.执行添加操作
    			int rows = qr.update(sql, params);
    			if (rows > 0) {
    				System.out.println("添加成功!");
    			} else {
    				System.out.println("添加失败!");
    			}
    		} catch (SQLException e) {
    			// TODO Auto-generated catch block
    			e.printStackTrace();
    		}
    	}
    	
    	/**
    	 * 根据id修改用户方法
    	 * 
    	 */
    	@Test
    	public void testUpdateUserById() {
    		try {
    			// 1.创建核心类QueryRunner
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写SQL语句
    			String sql = "update tbl_user set upassword=? where uid=?";
    			// 3.为站位符设置值
    			Object[] params = { "xxx", 21 };
    			// 4.执行添加操作
    			int rows = qr.update(sql, params);
    			if (rows > 0) {
    				System.out.println("修改成功!");
    			} else {
    				System.out.println("修改失败!");
    			}
    		} catch (SQLException e) {
    			// TODO Auto-generated catch block
    			e.printStackTrace();
    		}
    	}
    	
    	/**
    	 * 根据id删除用户方法
    	 */
    	@Test
    	public void testDeleteUserById() {
    		try {
    			// 1.创建核心类QueryRunner
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写SQL语句
    			String sql = "delete from tbl_user where uid=?";
    			// 3.为站位符设置值
    			Object[] params = {19};
    			// 4.执行添加操作
    			int rows = qr.update(sql, params);
    			if (rows > 0) {
    				System.out.println("删除成功!");
    			} else {
    				System.out.println("删除失败!");
    			}
    		} catch (SQLException e) {
    			// TODO Auto-generated catch block
    			e.printStackTrace();
    		}
    	}
    }

    ```

3. 利用DBUtils查询，需要用到Bean类  

    比如Bean类：查询到的数据都放在了User里面

    ```java
    public class User {
    	private int uid;
    	private String uname;
    	private String upassword;

    	public User() {
    	}

    	public int getUid() {
    		return uid;
    	}

    	public void setUid(int uid) {
    		this.uid = uid;
    	}

    	public String getUname() {
    		return uname;
    	}

    	public void setUname(String uname) {
    		this.uname = uname;
    	}

    	public String getUpassword() {
    		return upassword;
    	}

    	public void setUpassword(String upassword) {
    		this.upassword = upassword;
    	}

    }
    ```

    查询代码：  
    
    ```java
    import java.sql.SQLException;
    import java.util.List;
    import java.util.Map;
    
    import org.apache.commons.dbutils.QueryRunner;
    import org.apache.commons.dbutils.handlers.BeanHandler;
    import org.apache.commons.dbutils.handlers.BeanListHandler;
    import org.apache.commons.dbutils.handlers.ColumnListHandler;
    import org.apache.commons.dbutils.handlers.MapListHandler;
    import org.apache.commons.dbutils.handlers.ScalarHandler;
    import org.junit.Test;
    
    import cn.itheima.domain.User;
    import cn.itheima.jdbc.utils.C3P0Utils;
    
    /**
     * 测试DBUtils查询操作
     * 
     * @author Never Say Never
     * @date 2016年7月31日
     * @version V1.0
     */
    public class TestDBUtils2 {
    
    	/*
    	 * 查询所有用户方法
    	 */
    	@Test
    	public void testQueryAll() {
    		try {
    			// 1.获取核心类queryRunner
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写sql语句
    			String sql = "select * from tbl_user";
    			// 3.执行查询操作
    			List<User> users = qr.query(sql, new BeanListHandler<User>(User.class));
    			// 4.对结果集集合进行遍历
    			for (User user : users) {
    				System.out.println(user.getUname() + " : " + user.getUpassword());
    			}
    		} catch (SQLException e) {
    			throw new RuntimeException(e);
    		}
    	}
    	
    	/*
    	 * 根据id查询用户方法
    	 */
    	@Test
    	public void testQueryUserById() {
    		try {
    			// 1.获取核心类queryRunner
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写sql语句
    			String sql = "select * from tbl_user where uid=?";
    			//3.为占位符设置值
    			Object[] params = {21};
    			// 4.执行查询操作
    			User user = qr.query(sql, new BeanHandler<User>(User.class), params);
    			System.out.println(user.getUname() + " : " + user.getUpassword());
    		} catch (SQLException e) {
    			throw new RuntimeException(e);
    		}
    	}
    	
    	/*
    	 * 根据所有用户的总个数
    	 */
    	@Test
    	public void testQueryCount() {
    		try {
    			// 1.获取核心类queryRunner
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写sql语句
    			String sql = "select count(*) from tbl_user";
    			// 4.执行查询操作
    			Long count = (Long) qr.query(sql, new ScalarHandler());
    			System.out.println(count);
    		} catch (SQLException e) {
    			throw new RuntimeException(e);
    		}
    	}
    	
    	/*
    	 * 查询所有用户方法
    	 */
    	@Test
    	public void testQueryAll1() {
    		try {
    			// 1.获取核心类queryRunner
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写sql语句
    			String sql = "select * from tbl_user";
    			// 3.执行查询操作
    			List<Map<String, Object>> list = qr.query(sql, new MapListHandler());
    			// 4.对结果集集合进行遍历
    			for (Map<String, Object> map : list) {
    				System.out.println(map);
    			}
    		} catch (SQLException e) {
    			throw new RuntimeException(e);
    		}
    	}
    	
    	/*
    	 * 查询所有用户方法
    	 */
    	@Test
    	public void testQueryAll2() {
    		try {
    			// 1.获取核心类queryRunner
    			QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
    			// 2.编写sql语句
    			String sql = "select * from tbl_user";
    			// 3.执行查询操作
    			List<Object> list = qr.query(sql, new ColumnListHandler("uname"));
    			// 4.对结果集集合进行遍历
    			for (Object object : list) {
    				System.out.println(object);
    			}
    		} catch (SQLException e) {
    			throw new RuntimeException(e);
    		}
    	}
    }

    ```
    
