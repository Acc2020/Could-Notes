# Spring ： 自动装配



## Bean 的自动装配

- 自动装配是使用 spring 满足 bean 依赖的一种方式
- spring 会在应用上下文中为某个 bean 寻找其以来的 bean

 Spring 中 bean 有三种装配机制
 - XML 中显示配置
 - Java 中显示配置
 - 隐式的 bean 发现机制（自动化装配机制）

 通过组件扫描（commponet scanning）： spring 会自动发现应用上下文中所创建的 bean ;

 自动装配（autowiring）： spring 自动满足 bean 之间的依赖，IoC/DI


 ### 测试

 1、 创建两个实体类， Cat Dog 分配一个方法。

 ```Java
public class Cat {
    public void shout(){
        System.out.println("miao~");
    }
}

 ```
 
 ```Java
public class Dog {
    public void shout(){
        System.out.println("wang~");
    }
}

 ```
2、创建一个用户类 People

 ```Java
public class People {
    private Cat cat;
    private Dog dog;
    private String str;
    //省略 get set toString 方法
 ```
3、配置文件 beans.XML
 
 ```Java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="cat" class="com.acc.pojo.Cat"> </bean>
    <bean id="dog" class="com.acc.pojo.Dog"> </bean>
    <bean id="people" class="com.acc.pojo.People">
        <property name = "cat" ref = "cat" />
        <property name = "dog" ref = "dog" />
        <property name = "str" value = "String" />
    </bean>
</beans>
 ```

 4、测试类
 ```Java
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        People people = context.getBean("people", People.class);
        System.out.println(people);
        people.getCat().shout();
        people.getDog().shout();
    }
}
 ```
如果正常输出，测试环境正常。

### ByName
**autowiring ByName** （按名称自动装配）  
1、修改 beans.xml 增加 autowire = "byName" 
 ```xml
    <bean id="people" class="com.acc.pojo.People" autowire="byName">
        <property name="str" value="唐" />
    </bean>
 ```
2、测试
3、修改 car id 的名字继续测试，发现是 NullPointerException byName 找不到对应的 set 方法，setCat 方法无法执行，对象没有进行初始化。

==过程：bean 节点带有 byName 时，将查找所有的 set 方法名，获得去掉 set 并且首字母小写的字符串，再去 spring 容器中寻找是否有此字符串名称的 id 对象名，有注入，没有空指针== 
 



 ### ByType
**autowiring ByType** （按名称自动装配）  
1、修改 beans.xml 增加 autowire = "ByType" 
 ```xml
    <bean id="cat" class="com.acc.pojo.Cat"> </bean>
    <bean id="xxx" class="com.acc.pojo.Cat"> </bean>
    <bean id="dog" class="com.acc.pojo.Dog"> </bean>
    <bean id="people" class="com.acc.pojo.People" autowire="ByType">
        <property name="str" value="唐" />
    </bean>
 ```
2、测试,报错 NoUniqueBeanDefinitionException
3、删除 xxx id 的名字继续测试，按照类型装配 id 属性没有也能装配。



### 使用注解
JDK 1.5 后支持注解，spring 2.5 后支持注解
1、spring 配置文件中增加 context 文件头和依赖
 ```xml
xmlns:context="http://www.springframework.org/schema/context
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd
 ```
2、开启注解支持

```xml
<context:annotation-config />
```


#### @autowired

- @Autowired是按类型自动转配的，不支持id匹配。
- 需要导入 spring-aop的包！
1、将 People 类中的set方法去掉，使用@Autowired注解
```Java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String str;

    public Cat getCat() {
        return cat;
    }
    public Dog getDog() {
        return dog;
    }
    public String getStr() {
        return str;
    }
}
```
2、修改 xml 文件
```xml
    <context:annotation-config />
    <bean id="cat" class="com.acc.pojo.Cat"> </bean>
    <bean id="dog" class="com.acc.pojo.Dog"> </bean>
    <bean id="people" class="com.acc.pojo.People" autowire="byName">
        <property name="str" value="唐" />
    </bean>
```
3、测试

> @Autowired(required=false) 说明： false，对象可以为null；true，对象必须存对象，不能为null。

#### @Qualifier
- @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
- @Qualifier不能单独使用。



#### @Resource
- @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
- 其次再进行默认的byName方式进行装配；
- 如果以上都不成功，则按byType的方式自动装配。
- 都不成功，则报异常。


### @Resource 和 @autowired 异同
1、@Resource 和 @autowired 都可以用来装配 bean，都可以写在字段上或者在 setter 方法上  
2、@autowired 默认是按照类型装配（byName）默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用  
3、@Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。 当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先byName。