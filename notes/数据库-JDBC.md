[toc]
#JDBC 
- Java DataBase Connectivity Java 数据库连接， Java语言操作数据库
- JDBC 本质：sun 公司定义的一套操作关系型数据库的规则接口，各个数据库厂商进行实现对应的接口，包括了一组与数据库交互的api提供驱动的包。
![OBC](./img/sql/sql-jdbc.png)

### JDBC 数据库访问的模型
JDBC支持数据库访问的两层模型和三层模型
- 两层模型，DBMS 和 Java 应用程序之间通过通信协议进行访问。
- 三层模型，增加了与网络端的访问
![sql-model](./img/sql/sql-model.png)

## JDBC 建立连接

### 导入 JDBC 驱动<br>
对应数据库中的实现的类进行添加<br>

### 注册 JDBC 驱动程序<br>
数据库的驱动类文件动态的加载到内存中
以 mysql 为例，两种注册方式  
**方式1--Class.forName()**
```Java
try {
   Class.forName("com.mysql.jdbc.Driver");
}
catch(ClassNotFoundException ex) {
   e.printStackTrace();
}
```
**方法2--DriverManager.registerDriver()**
```Java
   Driver driver = new com.mysql.jdbc.Driver();
   DriverManager.registerDriver(driver);
```
### 数据库 URL 指定<br>
加载了驱动程序，便可以使用 DriverManager.getConnection() 方法连接到数据库了。
```Java
// DriverManager.getConnection() 有三种重载方式
getConnection(String url)
getConnection(String url, Properties prop)
getConnection(String url, String user, String password)
```

数据库的URL是指向数据库地址。下表列出了下来流行的JDBC驱动程序名和数据库的URL。  

|RDBMS|	JDBC驱动程序的名称|	URL |
|--- | --- | ---| 
|Mysql|	com.mysql.jdbc.Driver|	jdbc:mysql://hostname/ databaseName|
|Oracle	|oracle.jdbc.driver.OracleDriver|	jdbc:oracle:thin:@hostname:port Number:databaseName|
|DB2|	COM.ibm.db2.jdbc.net.DB2Driver|	jdbc:db2:hostname:port Number/databaseName|
|Sybase	|com.sybase.jdbc.SybDriver	|jdbc:sybase:Tds:hostname: port Number/databaseName|
### 创建连接对象<br>
用DriverManager.getConnection()方法来创建一个连接对象，以 Mysql 为例。getConnection()最常用形式要求传递一个数据库URL，用户名 username和密码 password。

1、使用数据库URL的用户名和密码

```java
String URL = "jdbc:mysql://localhost/EXAMPLE";
String USER = "username";
String PASS = "password"
Connection conn = DriverManager.getConnection(URL, USER, PASS);
```

2、只使用一个数据库URL
```java
String URL = "jdbc:mysql://localhost/EXAMPLE?user=root&password=0909";
//Mysql URL的参数设置详细可以查阅相关资料
Connection conn = DriverManager.getConnection(URL);
```
3、使用数据库的URL和一个Properties对象
```java
import java.util.*;
String URL = "jdbc:mysql://localhost/EXAMPLE";
Properties pro = new Properties( );
//Properties对象，保存一组关键字-值对
pro.put( "user", "root" );
pro.put( "password", "" );
Connection conn = DriverManager.getConnection(URL, pro);
```
4、关闭JDBC 连接

```java
conn.close();
```

## JDBC 接口
Sun 公司开发了一个标准的 SQL 数据库访问接口———JDBC API，可以使用不同的接口实现能够让 SQL 或 PL/SQL 命令和从数据库接收数据。它们还定义了许多方法，帮助消除Java 和数据库之间数据类型的差异。

|接口|	应用场景|
|---| ---|
|Statement|	当在运行时使用静态 SQL 语句时（Statement接口不能接收参数）|
|PreparedStatement|	当计划多次使用 SQL 语句时（PreparedStatement 接口接收在运行时输入参数）|
|CallableStatement	|当要访问数据库中的存储过程时（CallableStatement对象的接口还可以接收运行时输入参数）|
**使用过程**
```java
Statement stmt = null;
try {
   stmt = conn.createStatement( );
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
    stmt.close();
}

```

## JDBC 事务
启动一个事务，而不是让 JDBC 驱动程序默认使用 auto-commit 模式支持。这个时候我们就要使用 Connection 对象的 setAutoCommit() 方法。我们传递一个布尔值 false 到 setAutoCommit() 中，就可以关闭自动提交。反之我们传入一个 true 便将其重新打开。
```java
try{
    Connection conn = null;
    conn = DriverManager.getConnection(URL);
    //关闭自动提交,也叫事务的开启
    conn.setAutoCommit(false); 
    // ···
    // 事务提交，出现异常将执行不到这步
    conn.commit();
}catch( ){
    //出现异常时在 catch 块中进行异常的抛出，完成事务的回滚，最好用父类异常 Exception
    conn.rollback(); 
}
```


## 数据库连接池
一个容器(集合)，存放数据库连接的容器。当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。
- 好处：节约资源 用户访问高效
```java
1. 标准接口：DataSource   javax.sql包下的
    方法：
    * 获取连接：getConnection()
    * 归还连接：Connection.close()。如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会再关闭连接了。而是归还连接.

2. 一般我们不去实现它，有数据库厂商来实现
    1. C3P0：数据库连接池技术
    2. Druid：数据库连接池实现技术，由阿里巴巴提供的
```
C3P0
```java
public class C3P0 {
    public static void main(String[] args) throws SQLException {
        //1.创建数据库连接池对象
        DataSource ds  = new ComboPooledDataSource();
        //2. 获取连接对象
        Connection conn = ds.getConnection();
        //3. 打印
        System.out.println(conn);

    }
}
```
Druid
```java
public class Druid {
    public static void main(String[] args) throws Exception {
        //1.导入jar包
        //2.定义配置文件
        //3.加载配置文件
        Properties pro = new Properties();
        InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
        pro.load(is);
        //4.获取连接池对象
        DataSource ds = DruidDataSourceFactory.createDataSource(pro);
        //5.获取连接
        Connection conn = ds.getConnection();
        System.out.println(conn);

    }
}
```


## Spring JDBC
Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发
```java
public class JdbcTemplate {
    public static void main(String[] args) {
        //1.导入jar包
        //2.创建JDBCTemplate对象
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
        //3.调用方法
        String sql = "update account set balance = 5000 where id = ?";
        int count = template.update(sql, 3);
        System.out.println(count);
    }
}
```