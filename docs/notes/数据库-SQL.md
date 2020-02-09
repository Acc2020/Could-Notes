[toc]
# SQL


## 基础

(SQL)Structured Query Language：结构化查询语言 <br>
可以操作所有关系型数据库的规则。每一种 DBMS 的方式存在不一样,有自己的实现<br>	
**SQL 通用语法** <br>
-  SQL 语句可以单行或多行书写，以分号结尾
-  MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写
- 3 种注释 
```SQL
# 注释内容(mysql 特有)
SELECT * 
FROM  mytable -- 注释内容   
/* 注释 */
```
**SQL 分类**<br>
- DDL(Data Definition Language)数据定义语言<br>		
- DML(Data Manipulation Language)数据操作语言<br>		
- DQL(Data Query Language)数据查询语言<br>	
- DCL(Data Control Language)数据控制语言<br>
		
## DDL：操作数据库、表
用来定义数据库对象：数据库，表，列等
- CRUD <br>
    C(Create):创建<br>
    R(Retrieve)：查询<br>
    U(Update):修改<br>
    D(Delete):删除<br>
### C(Create):创建
**创建数据库**
```SQL
SHOW DATABASES;
-- 查看当前数据库中存在的数据库
CREATE DATABASE mydb;
USE mydb;
SELECT DATABASE();
-- 查看当前正在使用的数据库
```
**创建表**
```SQL
CREATE TABLE mytable (
    id INT, -- 创建 id int 类型
    name VARCHAR(10), -- 创建 name varchar 类型长度为 10 ，长度可自动增长
    col1 DATE NULL -- 创建 col1 date 类型，可为空
);
-- 最后用 ‘;’ 结尾，最后一列不用 ‘,’;
/*  
常用的数据属性：
每一个属性来自一个域，它的取值必须是域中的值。
1、INT 长整数（4个字节）
2、DOUBLE 浮点型
3、VARCHAR(n) 最大长度为 n 的可变长字符串
4、DATE 日期，年月日：YYYY-MM-DD
5、TIME 时间，时分秒：HH：MM：SS
6、DATETIME 时间日期，年月日时分秒
7、TIMESTAMP 时间戳I
*/
```
### R(Retrieve):查询

```SQL
SHOW　TABLES; -- 查看当前数据库中的表
SHOW CREATE TABLE mytable -- 查看当前创建表的表结构
DESC mytable -- 查看当前表结构，与上一个方式不一样
```
### U(Update):修改
```SQL
-- 修改表名
-- alter table 表名 rename to 新的表名;
ALTER TABLE mytable rename to newtable;
-- 修改表的字符集
-- alter table 表名 character set 字符集名称;
ALTER TABLE mytable CHARARTER SET GBK   
-- 添加一列
-- alter table 表名 add 列名 数据类型;
ALTER TABLE mytable ADD col2 INT;
-- 修改列名称 类型
-- alter table 表名 change 列名 新列别 新数据类型;
ALTER TABLE mytable CHANGE col2 newcol2 VARCHAR(20);
-- alter table 表名 modify 列名 新数据类型;
ALTER TABLE mytable MODIFY col2 DATE;
-- 删除列
-- alter table 表名 drop 列名;
ALTER TABLE mytable DROP col2;
```

### D(Delete):删除
```SQL
-- 删除表;
DROP TABLE IF EXISTS mytable;
```
## DML：增删改表中数据
用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等
### 增加数据
普通增加
```SQL
 INSERT INTO mytable(col1,col2) values(val1, val2);
	/* 注意：
		1. 列名和值要一一对应。
		2. 如果表名后，不定义列名，则默认给所有列添加值
			insert into 表名 values(值1,值2,...值n);
		3. 除了数字类型，其他类型需要使用引号(单双都可以)引起来
        */
```
插入检索后的数据
```SQL
INSERT INTO mytable(col1, col2)
SELECT col1, col2 FROM mytable02;
```
讲一个表数据增加到另一个表中
```SQL
CREATE TABLE newtable AS 
SELECT * FROM mytable;
```
### 删除数据
```SQL
DELETE FROM mytable WHERE id = 1; -- 删除 id = 1 的记录
DELETE FROM mytable; -- 删除表中所有记录
TRUNCATE TABLE mytable; --先删除表，然后再创建一张一样的表
/* 注意：
		1. 如果不加条件，则删除表中所有记录。
		2. 如果要删除所有记录
			1. delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
			2. TRUNCATE TABLE 表名; -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。
            */
```
### 修改数据
```SQL
UPDATE mytable 
SET col = nowcol
WHERE id = 1;
-- 如果不加任何条件，则会将表中所有记录全部修改。
```

## DQL：查询表中的记录
用来查询数据库中表的记录(数据)。关键字：select, where 等
```SQL
 /*语法
	select
		字段列表
	from
		表名列表
	where
		条件列表
	group by
		分组字段
	having
		分组之后的条件
	order by
		排序
	limit
		分页限定
    */
```

### 基础查询
```SQL
SELECT DISTINCT col 
FROM mytable
/*1. 多个字段的查询
    SELECT 字段名1，字段名2... FROM 表名；
    * tips：
        * 如果查询所有字段，则可以使用*来替代字段列表。
        /
2. 去除重复：
    * DISTINCT
3. 起别名：
    * as：as也可以省略
    */
```
### 条件查询
```SQL
WHERE 可跟调节的运算符
* > 、< 、<= 、>= 、= 、<>
* BETWEEN...AND  
* IN( 集合) 
* LIKE：模糊查询
    * 占位符：
        * _:单个任意字符
        * %：多个任意字符
* IS NULL  
* and  或 &&
* or  或 || 
* not  或 !
```
### 排序查询
```SQL
* 语法：order by 子句
    * order by 排序字段1 排序方式1 ，  排序字段2 排序方式2...

* 排序方式：
    * ASC：升序，默认的。
    * DESC：降序。

* tips：
    * 如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。
```

### 聚合函数
```SQL
1. count：计算个数
    1. 一般选择非空的列：主键
    2. count(*)
2. max：计算最大值
3. min：计算最小值
4. sum：计算和
5. avg：计算平均值
* tips：聚合函数的计算，排除null值。
		解决方案：
			1. 选择不包含非空的列进行计算
			2. IFNULL函数
```

### 分组查询
```SQL
1. 语法：group by 分组字段；
2. 注意：
    1. 分组之后查询的字段：分组字段、聚合函数
    2. where 和 having 的区别？
        1. where 在分组之前进行限定，如果不满足条件，则不参与分组。having在分组之后进行限定，如果不满足结果，则不会被查询出来
        2. where 后不可以跟聚合函数，having可以进行聚合函数的判断。
```

### 分页查询
```SQL
1. 语法：limit 开始的索引,每页查询的条数;
2. 公式：开始的索引 = （当前的页码 - 1） * 每页显示的条数
    -- 每页显示3条记录 
    SELECT * FROM student LIMIT 0,3; -- 第1页
    SELECT * FROM student LIMIT 3,3; -- 第2页	
    SELECT * FROM student LIMIT 6,3; -- 第3页
3. limit 是一个MySQL"方言"
```

### 多表查询


## DCL：管理用户，授权
用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等
###用户管理
```SQL
1. 添加用户：
    * 语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
2. 删除用户：
    * 语法：DROP USER '用户名'@'主机名';
3. 修改用户密码： 
    UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
    UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
    
    SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
    SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');

    * mysql中忘记了root用户的密码？
        1. cmd -- > net stop mysql 停止mysql服务
            * 需要管理员运行该cmd

        2. 使用无验证方式启动mysql服务： mysqld --skip-grant-tables
        3. 打开新的cmd窗口,直接输入mysql命令，敲回车。就可以登录成功
        4. use mysql;
        5. update user set password = password('你的新密码') where user = 'root';
        6. 关闭两个窗口
        7. 打开任务管理器，手动结束mysqld.exe 的进程
        8. 启动mysql服务
        9. 使用新密码登录。
4. 查询用户：
    -- 1. 切换到mysql数据库
    USE myql;
    -- 2. 查询user表
    SELECT * FROM USER;
    
    * 通配符： % 表示可以在任意主机使用用户登录数据库
```
### 权限管理
```SQL
1. 查询权限：
    -- 查询权限
    SHOW GRANTS FOR '用户名'@'主机名';
    SHOW GRANTS FOR 'lisi'@'%';

2. 授予权限：
    -- 授予权限
    grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
    -- 给张三用户授予所有权限，在任意数据库任意表上
    
    GRANT ALL ON *.* TO 'zhangsan'@'localhost';
3. 撤销权限：
    -- 撤销权限：
    revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
    REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';
```


## 事务
如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。
操作：
1. 开启事务： start transaction;
2. 回滚：rollback;
3. 提交：commit;
```SQL
事务的四大特征：
	1. 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
	2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
	3. 隔离性：多个事务之间。相互独立。
	4. 一致性：事务操作前后，数据总量不变
3. 事务的隔离级别（了解）
	* 概念：多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。
	* 存在问题：
		1. 脏读：一个事务，读取到另一个事务中没有提交的数据
		2. 不可重复读(虚读)：在同一个事务中，两次读取到的数据不一样。
		3. 幻读：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。
	* 隔离级别：
		1. read uncommitted：读未提交
			* 产生的问题：脏读、不可重复读、幻读
		2. read committed：读已提交 （Oracle）
			* 产生的问题：不可重复读、幻读
		3. repeatable read：可重复读 （MySQL默认）
			* 产生的问题：幻读
		4. serializable：串行化
			* 可以解决所有的问题

		* 注意：隔离级别从小到大安全性越来越高，但是效率越来越低
		* 数据库查询隔离级别：
			* select @@tx_isolation;
		* 数据库设置隔离级别：
			* set global transaction isolation level  级别字符串;
```