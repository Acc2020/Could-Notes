# SQL
[toc]
## DQL（数据查询语言）
基本格式   
```SQL
    SELECT fied1, fied2...
    FROM table
    [WHERE ...]
    [GROUP BY ...]
    [HAVING ...]
    [ORDER BY ...] 
```

### 基本查询
**DISTINCT 唯一性**
```SQL
    SELECT  DISTINCT field FROM table
    # 例子  
    # 查询部门信息表中姓名唯一的记录
    SELECT DISTINCT name FROM deptname;
    # 查询部门信息中有多少部门名称
    SELECT DISTINCT dept FROM tablename
```

**GROUP BY 排序条件默认升序**
```SQL
    SELECT filelist FROM tablename
    WHERE selectcriteria 
    ORDER BY field ASC;

    # 例子 查找学生成绩升序和降序
    SELECT * FROM tablename WHERE grade=88 ORDER BY  ASC/DESC;
```

**GROUP BY 分组显示**
```SQL
    SELECT fieldlist 
    FROM tablename
    WHERE critieria
    GROUP BY groupfieldlist
    # 例子
    # 查询参加考试的班级
    SELECT classname from classtables GROUP BY classname;
```

**HAVING (分组过滤条件，必须与 GROUP BY)**

```SQL
    SELECT field1 
    FROM tablename
    WHERE   selectcriteria
    GROUP BY  groupfieldlist 
    HAVING groupcriteria
```


**BETWEEN .. AND ... 之间上下包含**
```SQL
SELECT * FROM tablename WHERE id BETWEEN 10 AND 20;
```
**IS 判断**
```SQL
SELECT * FROM tablename WHERE age IS NULL
```

### 复合查询
**连接查询**  
 根据等值连接的方法，能够查询到被查询的所有列
 ```SQL
    SELECT a.*, b.* FROM table A a, table B b WHERE a.field = b.field
 ```

**联合查询**  
把多个 SELECT （两个或者两个以上）语句的查询结果集合并成一个结果集显示，即联合查询，UNION 语法格式为

```SQL
    SELECT filed FROM tableA UNION [ALL] SELECT field FROM tableB UNION [ALL]
```
### 集合查询
- COUNT() 统计字段数
- AVG()  平均值
- SUM() 求和
- MAX() 最大值
- MIN() 最小值



## DML（数据操作语言）

### INSERT 
插入数据到表中
```SQL
    # 插入多个值到表中
    INSERT INTO 
    tablename(field1, field2, field3...)
    VALUES(field1, field2, filed3..)
    FROM tablename
    #例子
    Insert into  score select * from chengji where dept=‘北京分公司实施部’ 
    # 插入单个值到表中
    INSERT INTO tablename(filed1, field2, field3 ...) VALUES (values, values..);
```
注：
- 字符需要使用单引号 ' '
- 如果是字段中包含引号，需要替换成单引号
- 字符串类型的字段超过定义的长度会出错，再插入前进行长度校验
- 日期可以使用数据库中系统时间 SYSDATE 精确到秒。

### DELETE
```SQL
    #格式
    DELETE FROM tablename WHERE 条件；
    DELETE FROM tablename WHERE critieria = '';
    # 例子：删除score表中的name字段为张星的记录
    DELETE FROM score WHERE name = '张三';
```

### UPDATE
通过条件来写修改特定的数据
```SQL
    UPDATE tablename 字段= '值' WHERE 条件
    UPDATE table SET WHERE criteria = '';
```
## DDL (数据定义语言)

### CREATE 
```SQL
    CREATE TABLE tablename (
        field1 type(size) PRIMARY KEY NOT NULL,
        ...
    )
```
注：
- 较小的不为空的放在前面，可能为空的放在后面
- 可以给字段加上默认值，例如 DEFAULT SYSDATE 
每次插入和修改时，不用操作程序都能得到动作的实践

**表索引**
索引是一种数据库对象，对于在表或聚集索引上的每一值将包含一项，为行提供直接的快速索取。

```SQL
CREATE  INDEX index ON table(field1[ASC|DESC]，
field2[ASC|DESC]，...) 
[WITH {PRIMARY|DISALLOWNULL|IGNORENULL}]
```

**视图**  
```SQL
CREATE VIEW  视图名 AS SELECT  表1.字段1，表1.字段2，…，表2.字段1，表2.字段2，… FROM  表1，表2….；
CREATE VIEW view AS SELECT table1.field1,…,table2.field1… 
FROM table1,table2…. ;
```

### ALTER
修改表结构

```SQL
    #添加列
    ALTER TABLE table_name ADD column_name  datatype ;
    alter table score add id int
    #删除表中的列
    ALTER TABLE table_name DROP COLUMN column_name ;
    alter table score drop column id
    #改变表中列的数据类型
    ALTER TABLE table_name ALTER COLUMN column_name datatype； 
    alter table score alter column id nvarchar(50)

```


### DROP
DROP语句作用：删除内容和定义，释放空间。简单来说就是把整个表去掉.以后要新增数据是不可能的,除非新增一个表。
```SQL
DROP语句格式：
DROP TABLE 表名
```
### TRUNCATE
TRUNCATE TABLE 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。
删除表再创建新表的方式删除

---
## 注意点
### TRUNCATE和DELETE有以下几点区别
- TRUNCATE在各种表上无论是大的还是小的都非常快。如果有ROLLBACK命令DELETE将被撤销，而TRUNCATE则不会被撤销。
- TRUNCATE是一个DDL语言，向其他所有的DDL语言一样，他将被隐式提交，不能对TRUNCATE使用ROLLBACK命令。
- TRUNCATE将重新设置高水平线和所有的索引。在对整个表和索引进行完全浏览时，经过TRUNCATE操作后的表比DELETE操作后的表要快得多。


