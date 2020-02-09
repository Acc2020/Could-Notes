# MyBatis 学习笔记

## MyBatis 简介
MyBatis 属于一个 **Java持久层框架**，主要通过 XML 配置和注解两种形式把 SQL 语句关联（同时只能使用一种方式，否则或报错），相比较于 Hibernate 属于半自动框架，性能来看  
> JDBC > MyBatis > Hibernate
 
>- MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。
>- MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。
>- MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通简单 Java 对象）为数据库中的记录。

### 整体框架结构
可以通过整体 MyBatis 结构框架进行一个初步的了解
![MyBatis整体框架.png](./img/Framework/Mybatis-config.png)

- MyBatis 的运行主要依靠 xml 的核心配置，核心配置中包含进行连接数据库实列， 和数据源，连接池，以及事务管理器等，一般的命名方式为  MaybatisConfig.xml
- 还有就是 Mapper.xml 映射文件，可以有多个，里面配置的是 Statement （简单说是 SQL 也行），如果基于注解的方式不用配置该 xml 文件
- SqlSessionFactory 执行的时候会通过主配置文件构建出 SqlSessionFactory ，然后获得 SqlSession 对象，利用 SqlSession 就可以操作数据库了
- SqlSession 的底层会通过一个执行器来执行 Statement（SQL），执行器一般有两种实现，一种是基本的，一种是带有缓存功能的
- Mapper Statement SQL 语句的输入执行和输出

### 文档资料
下载地址： https://github.com/mybatis/mybatis-3  
中文文档： http://www.mybatis.org/mybatis-3/zh/index.html


### 入门案例
1、MaybatisConfig.xml 配置  
入门最好的方式是通过官网的入门配置案列进行配置。
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!-- JDCB 的配置文件存放在外部文件中，可以引入到主配置文件中-->
    <properties resource="jdbcConfig.properties"></properties>      
    <!--别名-->
    <typeAliases>
        <package name="com.example.domain"></package>
    </typeAliases>
    <!--环境配置-->
    <environments default="mybatis">
        <!--mysql配置-->
        <environment id="mybatis">
            <transactionManager type="JDBC"/>
            <!-- 连接池配置 -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--mapper 配置 如果使用maven可以使用 resource ，如果使用别名可以使用 package -->
    <mappers>
        <!-- <package name="com.example.dao"></package> -->
<!--        <mapper resource="com.example.dao.IUserDao"></mapper>-->
        <mapper resource="com/example/IUserDao.xml"></mapper>
    </mappers>
</configuration>
```
2、xxmapper.xml 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 <mapper namespace="com.example.dao.IUserDao">
    <select id="findAll" resultType="com.example.domain.User">
        select * from user;
    </select>
</mapper>
```

3、对应的 java 代码,然后就可以看出，通过 session 的 selectOne 方法进行查询的时候是用 namespace.id 来定位 Statement 的
```java
public class MybatisTest {
    private InputStream is;
    private SqlSession sqlSession;
    private IUserDao dao;
    @Before
    public void init() throws IOException {
        //1、读取配置文件
        is = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2、获取 sqlsessionFactory对象
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);
        //3、获取sqlsession 对象
        sqlSession = factory.openSession();
        //4、获取 dao 的代理对象
        dao = sqlSession.getMapper(IUserDao.class);
        //5、执行查询所有的方法
    }
    @After
    public void distroy() throws IOException {
        sqlSession.commit();
        //6、释放资源
        sqlSession.close();
        is.close();
    }
    @Test
    public void MybataisTest01() throws IOException {
        //5、执行查询所有的方法
        List<User> users = dao.findAll();
        for (User user : users) {
            System.out.println(user);
        }
    }
}
```
4、进行 CRUD
mapper 和一个 Dao 层的文件进行对应，找到对应的标签
```java
public interface IUserDao {
 /**
     * 查询所有
     * @return
     */
    List<User> findAll();

    /**
     * 保存用户
     * @param user
     */
    void saveUser(User user);

    /**
     * 更新 user
     * @param user
     */
    void updateUser(User user);

    /**
     * 根据用户 id 删除用户
     */
    void deleteUser(Integer userId);

    /**
     * 根据 userId 查询一个用户
     * @param userId
     * @return
     */
    User findById(Integer userId);
}
```
定义映射文件 UserMapper （习惯上，命名为 xxxMapper）：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.dao.IUserDao">
    <select id="findAll" resultType="com.example.domain.User">
        select * from user;
    </select>

    <insert id="saveUser" parameterType="com.example.domain.User">
        <selectKey keyProperty="id" keyColumn="id" resultType="int" order="AFTER">
            select last_insert_id();
        </selectKey>
        insert into user(username,birthday, sex, address)
         values (#{username}, #{birthday}, #{sex}, #{address});
    </insert>


    <update id="updateUser" parameterType="com.example.domain.User">
        update user set username=#{username},birthday=#{birthday}, sex=#{sex}, address=#{address}
        where id=#{id}
    </update>

    <delete id="deleteUser" parameterType="java.lang.Integer">
        delete  from user where id =#{userId}
    </delete>

    <select id="findById"  parameterType="INT" resultType="com.example.domain.User">
        select * from user where id = #{userId}
    </select>
</mapper>
```
### 动态代理实现DAO
MyBatis 提供了使用动态代理的方式来实现 DAO，也就是说只需要写个 dao 的接口就行了，而不需要写具体的实现类；
然后先来介绍映射文件 XML 法，让我们只要配置好映射文件就不需要再写实现类：

Mapper 中的 namespace 指定为接口
id 对应接口中的方法名
接口方法中的参数必须和 Mapper 中 parameterType 配置的一致（可以省略，会根据传入值进行判断）
接口方法的返回值必须和 Mapper 中 resultType 配置的一致（不可省略）
满足上面四个条件应该就可以使用了，所以 dao 层接口又称为是 mapper 接口，使用的时候直接通过下面一行代码获取代理对象：
```java
// 设置事务自动提交
SqlSession session = sqlSessionFactory.openSession(true);
userDAO = sqlSession.getMapper(UserDAO.class);
```
这样就省去了写 dao 实现类的时间，如果还想省去 XML 文件那么就需要使用注解了，它就是专门做这个的，关于注解等会再说

那么为什么不需要具体的实现类呢，很显然使用的是动态代理技术；源码中有个 MapperProxy 类来做这件事，它实现了 InvocationHandler 接口，当我们调用 getMapper 方法时，就会创建出相应的一个代理对象，当我们执行这个接口的方法时就会调用 MapperProxy 中的 invoke 方法，从而不需要具体的实现类了。
那么它如何找到相关的 SQL 语句呢，在 MyBatis 加载主配置文件的同时，映射文件也一同加载了，如果接口和配置文件是对应的，那么就可以利用动态代理以及反射拿到接口名、方法名等数据通过方法名等找到对应的映射配置文件，也就知道对应的 SQL 语句了。
我们使用 getMapper 方法的时候还传入了一个类，内部通过使用泛型做了强转，所以即使是返回的代理对象我们可以直接用 UserDAO 去接收。


----

这里的 getMapper 方法的实现应该是：
```java
@Override
public <T> T getMapper(Class<T> type) {
  return configuration.<T>getMapper(type, this);
}
```
它是在 DefaultSqlSession 中被定义的，可以看出深层次的调用中还传入了 this （DefaultSqlSession），这是为后面调用 selectOne、selectList 等方法做准备。
在 MyBatis 加载配置文件的时候（准确说是 build 的时候），就会判断映射文件配置的 namespace 是不是个接口，如果是就做为 key （class 对象），并且根据这个接口创建一个 MapperProxyFactory 对象作为值，put 进一个叫 knownMappers 的 Map 对象中去。
在调用 getMapper 的时候，其实就是在从这个 Map 中获取相应的代理工厂，最终通过 MapperProxyFactory 的 newInstance 方法生产出一个具体的代理对象（ MapperProxy ），其中会根据返回值而调用不同的 SQL 查询方法，调用会触发其 invoke 方法。


### MyBatis的缓存机制

MyBatis 包含一个非常强大的查询缓存特性,它可以非常方便地配置和定制。缓存可以极大的提升查询效率。MyBatis系统中默认定义了两级缓存，一级缓存和二级缓存。

- 默认情况下，只有一级缓存（SqlSession级别的缓存，也称为本地缓存）开启。
- 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
- 为了提高扩展性。MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

#### 一级缓存（本地缓存）
与数据库同一次会话期间查询的数据会放在本地缓存中，以后如果需要获取相同数据，直接从缓存中拿，不再查询数据库

**一级缓存失效的四种情况**

一级缓存是sqlSession级别的缓存，也就说一个sqlSession拥有自己的一级缓存，一级缓存是一直开启的，没有使用一级缓存的情况

- 不同的sqlSession对应不同的一级缓存
- 同一个sqlSession,但是查询条件不一样
- 同一个sqlSession两次查询期间执行了任何一次增删改操作
- 同一个sqlSession两次查询期间手动清空了缓存
#### 二级缓存（全局缓存）
- 二级缓存基于namespace默认不开启，需要手动配置
- MyBatis提供二级缓存的接口以及实现，缓存实现要求POJO实现Serializable接口
- 二级缓存在SqlSession 关闭或提交之后才会生效
##### 工作机制

- 一个会话，查询一条数据，这个数据就会放在当前会话的一级缓存中
- 如果会话关闭，一级缓存中的数据就会保存到二级缓存中，新的查询信息，就参照二级缓存  

**使用步骤**

全局配置文件中开启二级缓存
```XML
<setting name="cacheEnabled" value="true"/>
```
需要使用二级缓存的映射文件mapper.xml处使用cache配置缓存
```XML
<cach></cach>
```
- 注意：POJO需要实现Serializable接口
Mybatis提供了整合ehcache缓存，具体整合方法参考官网文档，

