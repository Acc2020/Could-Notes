# Mybatis 第一个程序
[toc]
环境说明：
- JDK 8 + 
- MySQL 5.7.27
- maven 3.5
- IDEA 2019

## Mybatis 第一个程序
**流程思路：环境搭建-->导入 Mybatis-->编写代码-->测试**

### 代码过程
1.搭建实验数据库

```sql
CREATE DATABASE `mybatis`;

USE `mybatis`;
-- ----------------------------
-- Table structure for user
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` ( 
  `id` int(20) NOT NULL,
  `name` varchar(30) DEFAULT NULL,
  `pwd` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', '唐', '123456');
INSERT INTO `user` VALUES ('2', '张三', 'abcdef');
INSERT INTO `user` VALUES ('3', '李四', '987654');

```

2.导入相关 jar 包
- maven 仓库或者 Github 上可以寻找
```xml
    <!-- https://mvnrepository.com/artifact/junit/junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        <scope>test</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.3</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
```
3.配置 Mybaits.xml 配置文件
- 官方文档可以找到：[中文文档](http://www.mybatis.org/mybatis-3/zh/index.html)
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/acc/mapper/UserMapper.xml" />
        <!--资源名一样可以使用 class
        <mapper class="com.acc.mapper.UserMapper.xml" />
        -->
    </mappers>
</configuration>
```
4.创建Mybatis工具类
```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.IOException;
import java.io.InputStream;

/*
    @author www.github.com/Acc2020
    @version 1.0 2020/2/4
*/
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream is = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
        //获取 sqlSession 连接
    public static SqlSession getSession(){
        return sqlSessionFactory.openSession();
    }
}
```
5.创建实体类 User
```java
public class User {
    private int id;
    private String name;
    private String pwd;
    //相关get set toString 方法省略，可以使用 lombok 自动生成
}
```
6.创建 UserMapper 接口
```java
import com.acc.pojo.User;
import java.util.List;
/*
    @author www.github.com/Acc2020
    @version 1.0 2020/2/4
*/
public interface UserMapper {
    List<User> selectUser();
}
```
7.配置 UserMapper.xml 
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.acc.mapper.UserMapper">
    <select id="selectUser" resultType="com.acc.pojo.User">
        select * from user;
    </select>
</mapper>
```
8.测试使用
```java
import com.acc.mapper.UserMapper;
import com.acc.pojo.User;
import com.acc.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import java.util.ArrayList;
import java.util.List;

/*
    @author www.github.com/Acc2020
    @version 1.0 2020/2/4
*/
public class MyTest {
    @Test
    public void selectUser() {
        SqlSession session = MybatisUtils.getSession();
        // List<User> users = session.selectList("com.acc.acc.mapper.UserMapper.selectUser");
        UserMapper mapper = session.getMapper(UserMapper.class);
        List<User> users = mapper.selectUser();
        for (User user : users) {
            System.out.println(user);
        }
        session.close();
    }
}
```

### 问题排错
如果出现找不到资源文件，需要配置 maven 中 pom.xml
```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```
