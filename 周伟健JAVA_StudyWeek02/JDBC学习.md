第一节：JDBC简介
==========
JDBC（java数据库连接）是一种用于执行SQL语句的JavaAPI，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。JDBC提供了一种基准，据此可以构建更高级的工具和接口，使数据库开发人员能够编写数据库应用程序。


----------


----------

第二章：JDBC连接数据库
==========

第一节：JDBC连接数据库步骤
===============

1.加载驱动
------
	//驱动名称
	private static String jdbcName = "com.mysql.jdbc.Driver";
		Class.forName(jdbcName);
			System.out.println("加载驱动成功！");

2.连接数据库
-------

    //数据库地址
	private static String dbUrl ="jdbc:mysql://localhost:3306/book" ;
	//用户名
	private static String userName = "root";
	//密码
	private static String passWord = "123456";

3.使用语句操作数据库
-----------
    //获取数据库连接
    con = DriverManager.getConnection(dbUrl, userName,passWord);
    System.out.println("获取数据库连接成功！");

4.关闭数据库连接，释放资源
--------------
    con.close();


----------

第二节：在项目里配置数据库驱动
===============
方法：右击项目->Build Path->Configure Build Path->Add Exteranl JARs->选择驱动.jar

----------

第三节：加载数据驱动
==========

Mysql的驱动名： 
加载方式：Class.forName(驱动名)；

         Class.forName("com.mysql.jdbc.Driver");

----------

第四节：连接及关闭数据库
============

1，DriverManager 驱动管理类
---------------------

(管理一组JDBC驱动程序的基本服务)，主要负责获取一个数据库的连接；
语法： 

 - 试图建立到给定数据库 URL 的连接。返回到URL的连接

 
 - url：jdbc:subprotocol:subname 形式的数据库 url
 - user:数据库用户，连接是为该用户建立的
 - password:用户的密码

2，MySQL 数据库的连接地址格式
------------------

 - jdbc:mysql://IP 地址:端口号/数据库名称
 
 - jdbc 协议：JDBC URL 中的协议总是 jdbc ；
 
 - 子协议：驱动程序名或数据库连接机制（这种机制可由一个或多个驱动程序支持）的名称，如 mysql；
 
 - 子名称：一种标识数据库的方法。必须遵循“//主机名：端口/子协议”的标准 URL 命名约定， //如：
   //localhost:3306/db_book
   //综合连接地址格式如：jdbc:mysql://localhost:3306/db_book（数据库地址）

----------

3，Connection 接口 与特定数据库的连接（会话）。
------------------------------
void close()
立即释放此 Connection 对象的数据库和 JDBC 资源，而不是等待它们被自动释放


----------

第三章：使用 Statement 接口实现增，删，改操作
============================

第一节：Statement接口的引入
==================

 - 作用：用于执行静态 SQL 语句并返回它所生成结果的对象。
 
 - int executeUpdate(String sql) 执行给定 SQL 语句，该语句可能为 INSERT、UPDATE 或  DELETE 语句，或者不返回任何内容的 SQL 语句（如 SQL DDL 语句）。
 
 - void close() 立即释放此 Statement 对象的数据库和 JDBC 资源，而不是等待该对象自动关闭时发生此操作。



----------

第二节：使用 Statement 接口实现添加数据操作
===========================
        String sql = "insert into book values(1003,'name03','hcy',90)";
        Connection con = util01.getConnect();
        Statement sta = con.createStatement();
        int result = sta.executeUpdate(sql);//操作一条数据返回1，操作十条数据返回10，没有则返回0
		
		System.out.println("操作结果："+result+"条");
		
		//先开的后关
		sta.close();	//关闭Statement
		con.close();	//关闭连接
		


----------

第三节：使用 Statement 接口实现更新数据操作
===========================
    String sql = "update book set id="+book.getId()+",bookName='"+book.getBookName()+"',author='"+book.getAuthor()+"',price="+book.getPrice()+" where id="+book.getId();
    
    Statement sta = con.createStatement();	//获取Statement
	int result = sta.executeUpdate(sql);
	
		Book book = new Book(1003,"name03","lmm",100);
		int result = upDateBook(book);
		if(result !=  0) {
			System.out.println("更新成功！");
		}else {
			System.out.println("更新失败！");
		}
		


----------

第四节：使用 Statement 接口实现删除数据操作
===========================
    String sql = "delete from book where id="+id;
    Statement sta = con.createStatement();	//获取Statement
	int result = sta.executeUpdate(sql);
	int result02 = deleteBook(1005);
		
		if(result02 !=  0) {
			System.out.println("删除成功！");
		}else {
			System.out.println("删除失败！");
		}


----------

第四章 使用 PreparedStatement 接口实现增，删，改操作
====================================

第一节：PreparedStatement 接口引入
==========================
PreparedStatement 是 Statement 的子接口，属于预处理操作，与直接使用 Statement 不同的是，PreparedStatement在操作时，是先在数据表中准备好了一条 SQL 语句，但是此 SQL 语句的具体内容暂时不设置，而是之后再进
行设置。（以后开发一般用 PreparedStatement，不用 Statement）


----------

第二节：使用 PreparedStatement 接口实现添加数据操作
===================================
    String sql = "insert into book values (?,?,?,?)";
		
	PreparedStatement psm = con.prepareStatement(sql);

		psm.setInt(1,book.getId());
		psm.setString(2, book.getBookName());
		psm.setString(3,book.getAuthor());
		psm.setDouble(4,book.getPrice());
		
	int result = psm.executeUpdate();
	


----------

第三节：使用 PreparedStatement 接口实现更新数据操作
===================================
    String sql = "update book set id=?,bookName=?,author=?,price=? where id=?";
    


----------

第四节：使用 PreparedStatement 接口实现删除数据操作
===================================
    String sql = "delete from book where id=?";
 


----------

第五章 ResultSet 结果集
=================

第一节：ResultSet 结果集的引入
====================

当我们查询数据库时，返回的是一个二维的结果集，我们这时候需要使用 ResultSet 接口来遍历结果集，获取每一行的数据。


----------

第二节：使用 ResultSet 接口遍历查询结果
=========================

 - boolean next() 将光标从当前位置向前移一行。
 
 - ResultSet光标最初位于第一行前，第一次调用使第一行成为当前行，第二次调用使第二行成为当前行返回值为布尔类型，新的当前行有效返回true，不存在下一行则返回false
 
 - String getString(int columnIndex) 以 Java 编程语言中 String 的形式获取此 ResultSet 对象的当前行中指定列 的值。
 
 - String getString(String columnLabel) 以 Java 编程语言中 String 的形式获取此 ResultSet 对象的当前行中指定列的值。


----------
     ResultSet rs = psm.executeQuery();	//返回结果集ResultSet
     while(rs.next()) {
				int id = rs.getInt("id");		//获取第一列的值
				String bookName = rs.getString("bookName");   //获取第二列的值
				String author = rs.getString("author");
				double price = rs.getDouble("price");
				
				System.out.println("编号："+id+"\t 书名："+bookName+"\t作者："+author+"\t价格："+price);
			}

----------

第六章 处理大数据对象
===========
大数据对象处理主要有 CLOB（character large object）和 BLOB（binary large object）两种类型的字段；在CLOB中可以存储大字符数据对象，比如长篇小说；在 BLOB中可以存放二进制大数据对象，比如图片，电影，音乐；


----------

第一节：处理 CLOB 数据
==============
    String sql = "insert into book values (?,?)";
    PreparedStatement psm = con.prepareStatement(sql);

		psm.setDouble(1,book.getPrice());
		File context = book.getContext();	//获取大字符数据文件
		InputStream inputStream = new FileInputStream(context);
		psm.setAsciiStream(2, inputStream,(int)context.length());	//给第五个坑设置值
		


----------


		ResultSet rs = psm.executeQuery(sql);
    if(rs.next()) {
			double price = rs.getDouble("price");
			Clob c = rs.getClob("context");
			String context = c.getSubString(1, (int) c.length());
			


----------

第二节：处理 BLOG 数据
==============
    String sql = "insert into book values (?,?,?,?,?,?)";
    File pic = book.getPic();	//获取图片文件
	InputStream inputStream2 = new FileInputStream(pic);
	psm.setBinaryStream(6, inputStream2,(int)pic.length());		//给第六个坑设置值


----------


	Blob b = rs.getBlob("pic");
	FileOutputStream out = new FileOutputStream(new File("D:/pic2.jpg"));
	out.write(b.getBytes(1,(int)b.length())); 


----------

第七章 使用 CallableStatement 接口调用存储过程
=================================

第一节：CallableStatement 接口的引入
===========================
CallableStatement 主要是调用数据库中的存储过程，CallableStatement 也是 Statement 接口的子接口。在使用CallableStatement时可以接收存储过程的返回值。


----------

第二节：使用 CallableStatement 接口调用存储过程
=================================
void registerOutParameter(int parameterIndex, int sqlType)
按顺序位置 parameterIndex 将 OUT 参数注册为 JDBC 类型 sqlType。

    添加存储过程
    DELIMITER &&
    CREATE PROCEDURE pro_getBookNameById(IN bookId INT,OUT bN VARCHAR(20))
    BEGIN
    	SELECT bookName INTO bn FROM book WHERE id = bookId;
    END
    &&
    DELIMITER :

----------


    String sql = "{CALL pro_getBookNameById(?,?) }";
    CallableStatement cas = con.prepareCall(sql);
    cas.setInt(1,id);
	cas.registerOutParameter(2, Types.VARCHAR);	//设置返回类型
	cas.execute();	//执行
	String bookName = cas.getString("bN");	//获取返回值

----------

第八章 使用元数据分析数据库
==============

第一节：使用 DatabaseMetaData 获取数据库基本信息
=================================

 - DatabaseMetaData 可以得到数据库的一些基本信息，包括数据库的名称、版本，以及得到表的信息。
 - String getDatabaseProductName() 获取此数据库产品的名称。
 - int getDriverMajorVersion() 获取此 JDBC 驱动程序的主版本号。
 - int getDriverMinorVersion() 获取此 JDBC 驱动程序的次版本号。

----------
    DatabaseMetaData dmd = con.getMetaData();	//获取元数据
    
	System.out.println("数据库名称："+dmd.getDatabaseProductName());
	System.out.println("数据库版本号："+dmd.getDatabaseMajorVersion()+"."+dmd.getDatabaseMinorVersion());

----------


第二节：使用 ResultSetMetaData 获取 ResultSet 对象中的信息
============================================

 - ResultSetMetaData 可获取关于 ResultSet 对象中列的基本信息；
 
 - int getColumnCount() 返回此 ResultSet 对象中的列数。
 
 - String getColumnName(int column) 获取指定列的名称。
 
 - int getColumnTypeName(int column) 获取指定列的 SQL 类型名称。
 
  


----------


  
    ResultSetMetaData rsm = psm.getMetaData();
		
    int num = rsm.getColumnCount();	//获取元数据列的总数
		
	System.out.println("共有："+num+"列");
		
	for(int i=1;i<=num;i++) {
	System.out.println(rsm.getColumnName(i)+","+rsm.getColumnTypeName(i)); //列名和数据类型
	}


----------

第九章 JDBC 事务处理
=============

第一节：事务的概念
=========

 - 事务处理在数据库开发中有着非常重要的作用，所谓事务就是所有的操作要么一起成功，要么一起失败，事务本身具有原子性（Atomicity）、一致性（Consistency）、隔离性或独立性（Isolation）、持久性（Durability）4
   个特性，这 4 个特性也被称为 ACID 特征。

 - 原子性：原子性是事务最小的单元，是不可再分隔的单元，相当于一个个小的数据库操作，这些操作必须同时成功，如果一个失败了，则一切的操作将全部失败。

 - 一致性：指的是在数据库操作的前后是完全一致的，保证数据的有效性，如果事务正常操作则系统会维持有效性，如果事务出现了错误，则回到最原始状态，也要维持其有效性，这样保证事务开始时和结束时系统处于一致状态。

 - 隔离性：多个事务可以同时进行且彼此之间无法访问，只有当事务完成最终操作时，才可以看到结果；

 - 持久性：事务完成之后，它对于系统的影响是永久性的。该修改即使出现致命的系统故障也将一直保持。
 


----------

第二节：MySQL 对事务的支持
================
序号 命令 描述
1 SET AUTOCOMMIT=0 取消自动提交处理，开启事务处理
2 SET AUTOCOMMIT=1 打开自动提交处理，关闭事务处理
3 START TRANSACTION 启动事务
4 BEGIN 启动事务，相当于执行 START TRANSACTION
5 COMMIT 提交事务
6 ROLLBACK 回滚全部事务
7 SAVEPOINT 事务保存点名称
设置事务保存点
8 ROLLBACK TO
SAVEPOINT 保存点名称
回滚操作到保存点




----------


			

