# JDBC基础

## JDBC数据库连接

JDBC(JAVA DataBase Connection) JAVA数据库连接，用JAVA语言操作数据库。

Java连接数据库：
* 导jar包：驱动；
* 加载驱动：Class.forName("类名")；
* 给出url(背会),username,password；
* 使用DriverManager类得到Connection对象.

JAVA连接数据库：
```java
public class Jdbc {
	@Test
	public void fun1() throws ClassNotFoundException, SQLException {
		/*
		 * jdbc四大配置参数：
		 * > driveClassname:com.mysql.jdbc.Driver
		 * > (MySQL 8.0 以上: com.mysql.cj.jdbc.Driver)
		 * > url:jdbc:mysql://localhost:3306/mydb1
		 * > (MySQL 8.0 以上: jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC)
		 * > username:root
		 * > password:123
		 */
		Class.forName("com.mysql.cj.jdbc.Driver"); //加载驱动类，注册驱动
		//com.mysql.cj.jdbc.Driver driver = new com.mysql.cj.jdbc.Driver();
		//DriverManager.registerDriver(driver);
		
		String url = "jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC";
		String username = "root";
		String password = "123";
		
		Connection con = DriverManager.getConnection(url, username, password);
		System.out.println(con);
	}
}
```

所有java.sql.Driver实现类都提供了static块，块内代码就是把自己注册到DriverManager中。在jdbc4.0之后，每个驱动jar包中，在 META-INF/services 目录下提供名为 java.sql.Driver 的文件，文件内容为该接口的实现类名称。

为了兼容4.0之前的mysql版本，Class.forName 语句不要省略。



## JDBC实现增删改查

```java
package cancy.jdbc.demo;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import org.junit.jupiter.api.Test;

public class Jdbc {
	// 增删改实现
	@Test
	public void fun1() throws ClassNotFoundException, SQLException {
		/* 一、得到Connection
		 * jdbc四大配置参数：
		 * > driveClassname:com.mysql.jdbc.Driver
		 * > (MySQL 8.0 以上: com.mysql.cj.jdbc.Driver)
		 * > url:jdbc:mysql://localhost:3306/mydb1
		 * > (MySQL 8.0 以上: jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC)
		 * > username:root
		 * > password:123
		 */
		
		// 准备四大参数
		String driverClassName = "com.mysql.cj.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC";
		String username = "root";
		String password = "123";//123
		// 加载驱动类
		Class.forName(driverClassName); //加载驱动类，注册驱动
		// 使用DriverManager得到Connection
		Connection con = DriverManager.getConnection(url, username, password);
		System.out.println(con);
		
		/* 二、对数据库做增删改
		 * 1.通过Connection对象创建Statement(向数据库发送SQL语句)
		 * 2.调用int executeUpdate(String sql)，可发送DML,DDL
		 */
		
		// 通过Connection对象创建Statement
		Statement stmt = con.createStatement();
		// 使用Statement发送SQL语句
		//String sql = "INSERT INTO student VALUES(NULL,'刘备')";
		//String sql = "UPDATE student SET sname='诸葛亮' WHERE sname='庞统'";
		String sql = "DELETE FROM stu_tea";
		int r = stmt.executeUpdate(sql);
		System.out.println(r);
	}
	// 查询实现
	@Test
	public void fun2() throws ClassNotFoundException, SQLException {
		/* 一、得到Connection
		 * 二、得到Statement,发送select语句
		 * 三、对查询返回的表格进行解析
		 */
		
		// 一、得到Connection
		// 1.准备四大参数
		String driverClassName = "com.mysql.cj.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC";
		String username = "root";
		String password = "123";
		// 加载驱动类
		Class.forName(driverClassName); //加载驱动类，注册驱动
		// 使用DriverManager得到Connection
		Connection con = DriverManager.getConnection(url, username, password);
		System.out.println(con);
		
		// 二、得到Statement,发送select语句
		// 1.得到Statement对象：Connection的createStatement()方法
		Statement stmt = con.createStatement();
		// 2.调用Statement的ResultSet rs=executeQuerry(String querySql)
		ResultSet rs = stmt.executeQuery("SELECT * FROM student");
		
		// 三、解析ResultSet
		// 1. 把行光标移动到第一行，可以调用next()方法完成（光标起始位置在BeforeFirst）
		while(rs.next()) {//光标移到下一行并判断是否存在
			int sid = rs.getInt(1); //通过列编号获取该列值
			String sname = rs.getString("sname"); //通过列名称获取该列值			
			System.out.println(sid + "," + sname);
			
		// 四、关闭资源（倒关）
		rs.close();
		stmt.close();
		con.close(); //必须关闭
		
		}
	}
}
```

## JDBC代码规范化

```java
package cancy.jdbc.demo;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import org.junit.jupiter.api.Test;

public class Jdbc {
	// 增删改实现
	@Test
	public void fun1() throws ClassNotFoundException, SQLException {
		/* 一、得到Connection
		 * jdbc四大配置参数：
		 * > driveClassname:com.mysql.jdbc.Driver
		 * > (MySQL 8.0 以上: com.mysql.cj.jdbc.Driver)
		 * > url:jdbc:mysql://localhost:3306/mydb1
		 * > (MySQL 8.0 以上: jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC)
		 * > username:root
		 * > password:123
		 */
		
		// 准备四大参数
		String driverClassName = "com.mysql.cj.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC";
		String username = "root";
		String password = "123";
		// 加载驱动类
		Class.forName(driverClassName); //加载驱动类，注册驱动
		// 使用DriverManager得到Connection
		Connection con = DriverManager.getConnection(url, username, password);
		System.out.println(con);
		
		/* 二、对数据库做增删改
		 * 1.通过Connection对象创建Statement(向数据库发送SQL语句)
		 * 2.调用int executeUpdate(String sql)，可发送DML,DDL
		 */
		
		// 通过Connection对象创建Statement
		Statement stmt = con.createStatement();
		// 使用Statement发送SQL语句
		//String sql = "INSERT INTO student VALUES(NULL,'刘备')";
		//String sql = "UPDATE student SET sname='诸葛亮' WHERE sname='庞统'";
		String sql = "DELETE FROM stu_tea";
		int r = stmt.executeUpdate(sql);
		System.out.println(r);
	}
	// 查询实现
	@Test
	public void fun2() throws ClassNotFoundException, SQLException {
		/* 一、得到Connection
		 * 二、得到Statement,发送select语句
		 * 三、对查询返回的表格进行解析
		 */
		
		// 一、得到Connection
		// 1.准备四大参数
		String driverClassName = "com.mysql.cj.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC";
		String username = "root";
		String password = "123";
		// 加载驱动类
		Class.forName(driverClassName); //加载驱动类，注册驱动
		// 使用DriverManager得到Connection
		Connection con = DriverManager.getConnection(url, username, password);
		System.out.println(con);
		
		// 二、得到Statement,发送select语句
		// 1.得到Statement对象：Connection的createStatement()方法
		Statement stmt = con.createStatement();
		// 2.调用Statement的ResultSet rs=executeQuerry(String querySql)
		ResultSet rs = stmt.executeQuery("SELECT * FROM student");
		
		// 三、解析ResultSet
		// 1. 把行光标移动到第一行，可以调用next()方法完成（光标起始位置在BeforeFirst）
		while(rs.next()) {//光标移到下一行并判断是否存在
			int sid = rs.getInt(1); //通过列编号获取该列值
			String sname = rs.getString("sname"); //通过列名称获取该列值			
			System.out.println(sid + "," + sname);
		}	
		// 四、关闭资源（倒关）
		rs.close();
		stmt.close();
		con.close(); //必须关闭
	}
	//规范化
	public void fun3() throws Exception {
		
		Connection con = null;//定义引用
		Statement stmt = null;
		ResultSet rs = null;
		try {
			// 一、得到连接
			String driverClassName = "com.mysql.jbdc.Driver";
			String url = "jcdc:mysal://localhost:3306/mydb1?useSSL=false&serverTimezone=UTC";
			String username = "root";
			String password = "123";			
			Class.forName(driverClassName);
			con = DriverManager.getConnection(url);
			
			//二、创建statement
			stmt = con.createStatement();
			String sql = "select * from student";
			rs = stmt.executeQuery(sql);
			
			//三、循环遍历rs，打印其中数据
			while(rs.next()) {
				System.out.println(rs.getObject(1) + "," + rs.getObject(2));
			}
			
		} catch (Exception e) {
			throw new RuntimeException(e);
		} finally {
			// 关闭
			if(rs != null) rs.close();
			if(stmt != null) stmt.close();
			if(con != null) con.close();	
		}
	}
}
```
