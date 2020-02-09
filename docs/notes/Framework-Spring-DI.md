# Spring： 依赖注入（DI）

[toc]

## 依赖注入（DI）
- 依赖注入（Dependency Injection,DI）。
- 依赖 : 指Bean对象的创建依赖于容器 . Bean对象的依赖资源 。
- 注入 : 指Bean对象所依赖的资源 , 由容器来设置和装配 。


### 构造器注入
构造器注入保证一些必要的属性在Bean实例化时就得到设置，并且确保了Bean实例在实例化后就可以使用。

**使用方式：**

在类中，不用为属性设置setter方法，但是需要生成该类带参的构造方法。
在配置文件中配置该类的bean，并配置构造器，在配置构造器中用到了<constructor-arg>节点，该节点有四个属性：
- index：指定注入属性的顺序索引，从0开始；
- type：指该属性所对应的类型；
- ref：引用的依赖对象；
- value：当注入的不是依赖对象，而是基本数据类型时，就用value；

### Set 注入

要求被注入的属性 , 必须有set方法 , set方法的方法名由set + 属性首字母大写

测试 pojo 类：  
`Adress.java`
```java
public class Address {
    private String  address;
    public String getAddress() {
        return address;
    }
    public void setAddress(String address) {
        this.address = address;
    }
    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}
```
`Student.java`

```java
package com.acc.pojo;

import java.util.*;

public class Student {
    private  String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String, String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

   ... //省略 get set toString 方法

```

1、常量注入
```xml
        <bean id="address" class="com.acc.pojo.Address">
            <property name="address" value="中国"></property>
        </bean>

```
2|数组注入
```xml
        <property name="books">
            <array>
                <value>时间简史</value>
                <value>未来简史</value>
                <value>今日简史</value>
            </array>
        </property>

```
3、List 注入
```xml
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>看电影</value>
            </list>
        </property>

```
4、Map 注入
```xml
        <property name="card">
            <map>
                <entry key="身份证" value="1123213213213"></entry>
                <entry key="银行卡" value="1123213213213"></entry>
            </map>
        </property>

```
5、set 注入

```xml
        <property name="games">
            <set>
                <value>lol</value>
                <value>loco</value>
            </set>
        </property>

```

6、null注入
```xml
        <property name="wife" >
            <null></null>
        </property>


```
7、Properties注入
```xml
        <property name="info">
            <props>
                <prop key="admin">@dsa.com</prop>
                <prop key="password">1213</prop>
            </props>
        </property>

```


