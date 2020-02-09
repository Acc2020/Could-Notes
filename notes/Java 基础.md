# Java 基础常用知识点

[toc]


## 重载和重写
**1、重写Overrite**
完成重写（里式替换原则）需要满足三个限制条件。  
- 子类方法的访问权限必须大于等于父类方法；  
- 子类方法的返回类型必须是父类方法返回类型或为其子类型。
- 子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。  
使用 @Override 注解，可以让编译器帮忙检查是否满足上面的三个限制条件。


**2、重载**
- 存在于同一个类中，指一个方法与已经存在的的方法在名称上相同，但参数个数，类型，顺序有所不同。
- 返回值相同但其它都相同的不算是重载。



## 抽象和接口
**抽象类需要注意的点**<br >
- 抽象类不能创建 new 对象
- 必须用一个子类来继承抽象父类
- 子类必须覆盖重写抽象类中的所有抽象方法（覆盖重写）（实现）
- 抽象类可以实现多抽象子类继承，以筛选中间的抽象方法。





## 接口在不同 Java 版本中的不同内容

**版本区别**
- Java7 接口中包含的内容有 1.常量 2。抽象方法
- Java8 额外包含： 3.默认方法 4.静态方法
- Java9 额外包含：5.私有方法(普通/静态)

**接口使用步骤**
1.接口不能直接使用，必须要有一个实现类来实现接口。
```java
public class 实现类名称 implements 接口名称{
    //
}
2.接口实现类必须覆盖重写接口中所有的抽象方法，
```

**注意事项**
1. 如果实现类没有覆盖重写接口中的所有方法，那么这个实现类必须抽象类
2. 接口中的默认方法能够解决接口更新，可以重写。
格式：[public] default 返回值 方法名称（参数列表） { 方法体 } 
3. 接口的静态方法不能通过接口的实现类对象去调用，需要使用接口的名称去调用。
4. 对于 Java 9 以后的版本中能够在接口中定义私有方法，能够增加封装性。
5. 接口中的常量能够直接通过接口的名称直接访问 MyInterfaceConst.NUM_OF_MY_CLASS
格式： [public] [static] [final] 数据类型 常量名称 = 数据值 )


## 多态
父类引用指向子类对象

```java
格式：
父类名称 对象名 = new 子类名称（）
接口名称 对象名 = new 实现类名称（）
```
多态中访问成员变量和成员方法
- 访问成员变量 <br>
    直接通过对象名称访问成员变量时，父类是谁，优先谁，没有向上找  
    间接通过成员方法访问成员变量时，方法属于谁，有限谁，没有向上找  
- 访问成员方法<br >
    谁 new 优先谁，没有向上找
    编译看左边，运行看右边
    
向上转型，正常多态的写法，安全的写法，从小范围转向大范围

向下转型，为了保证安全性可以试用 intanceof 关键字判断对象的类型




## 常用 API 的使用方法
### Object 类
`java.lang.Object`类是Java语言中的根类，即所有类的父类。它中描述的所有方法子类都可以使用。在对象实例化的时候，最终找的父类就是Object。

如果一个类没有特别指定父类，	那么默认则继承自Object类。例如：

```java
public class MyClass /*extends Object*/ {
  	// ...
}
```
#### toString()
toString() 方法 属于 Object 类 每个类都有这个方法，这个方法默认返回该对象的字符串表示，其实该字符串内容就是对象的类型+@+内存地址值。


由于toString方法返回的结果是内存地址，而在开发中，经常需要按照对象的属性得到相应的字符串表现形式，因此也需要重写它。

如果不希望使用toString方法的默认行为，则可以对它进行覆盖重写。例如自定义的Person类：

```java
public class Person {  
    private String name;
    private int age;

    @Override
    public String toString() {
        return "Person{" + "name='" + name + '\'' + ", age=" + age + '}';
    }

    // 省略构造器与Getter Setter
}
```
注：
在我们直接使用输出语句输出对象名的时候,通过该对象调用了其toString()方法。


#### equals 方法
* public boolean equals(Object obj) ：指示其他某个对象是否与此对象“相等”。
在没有默认重写的情况下,Object 类默认进行‘==’运算符的对象地址比较，基本数据类型比较多的是数据，
引用类型比较的是对象地址。

```java
public class Person {	
	private String name;
	private int age;
    @Override
    public boolean equals(Object o) {
        // 如果对象地址一样，则认为相同
        if (this == o)
            return true;
        // 如果参数为空，或者类型信息不一样，则认为不同
        if (o == null || getClass() != o.getClass())
            return false;
        // 转换为当前类型
        Person person = (Person) o;
        // 要求基本类型相等，并且将引用类型交给java.util.Objects类的equals静态方法取用结果
        return age == person.age && Objects.equals(name, person.name);
    }
}
```


### Date类
- `public Date()`：分配Date对象并初始化此对象，以表示分配它的时间（精确到毫秒）。
- `public Date(long date)`：分配Date对象并初始化此对象，以表示自从标准基准时间（称为“历元（epoch）”，即1970年1月1日00:00:00 GMT）以来的指定毫秒数。

> tips: 由于我们处于东八区，所以我们的基准时间为1970年1月1日8时0分0秒。

```java
import java.util.Date;

public class Demo01Date {
    public static void main(String[] args) {
        // 创建日期对象，把当前的时间
        System.out.println(new Date()); // Tue Jan 16 14:37:35 CST 2018
        // 创建日期对象，把当前的毫秒值转成日期对象
        System.out.println(new Date(0L)); // Thu Jan 01 08:00:00 CST 1970
    }
}
```
> tips:在使用println方法时，会自动调用Date类中的toString方法。Date类对Object类中的toString方法进行了覆盖重写，所以结果为指定格式的字符串。

Date类中的多数方法已经过时，常用的方法有：

* `public long getTime()` 把日期对象转换成对应的时间毫秒值。

### DateFormat类
`java.text.DateFormat` 是日期/时间格式化子类的抽象类，我们通过这个类可以帮我们完成日期和文本之间的转换,也就是可以在Date对象与String对象之间进行来回转换。

* **格式化**：按照指定的格式，从Date对象转换为String对象。
* **解析**：按照指定的格式，从String对象转换为Date对象。

#### 构造方法

由于DateFormat为抽象类，不能直接使用，所以需要常用的子类`java.text.SimpleDateFormat`。这个类需要一个模式（格式）来指定格式化或解析的标准。构造方法为：

* `public SimpleDateFormat(String pattern)`：用给定的模式和默认语言环境的日期格式符号构造SimpleDateFormat。

参数pattern是一个字符串，代表日期时间的自定义格式。
| 标识字母（区分大小写） | 含义   |
| ----------- | ---- |
| y           | 年    |
| M           | 月    |
| d           | 日    |
| H           | 时    |
| m           | 分    |
| s           | 秒    |
> 备注：更详细的格式规则，可以参考SimpleDateFormat类的API文档0。

创建SimpleDateFormat对象的代码如：

```java
import java.text.DateFormat;
import java.text.SimpleDateFormat;

public class Demo02SimpleDateFormat {
    public static void main(String[] args) {
        // 对应的日期格式如：2018-01-16 15:06:38
        DateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    }    
}
```
DateFormat类的常用方法有：

- `public String format(Date date)`：将Date对象格式化为字符串。
- `public Date parse(String source)`：将字符串解析为Date对象。


### System 类

    public final class Systemextends ObjectSystem 类包含一些有用的类字段和方法。它不能被实例化。
    
- public static long currentTimeMillis()`：返回以毫秒为单位的当前时间。
- public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)`：将数组中指定的数据拷贝到另一个数组中。    
    使用public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)`：将数组中指定的数据拷贝到另一个数组中。进行数组复制
   arrarcopy 属于static方法可以直接使用类名进行调用
   src - 源数组。
    srcPos - 源数组中的起始位置。
    dest - 目标数组。
    destPos - 目标数据中的起始位置。
    length - 要复制的数组元素的数量。
    

## StringBuilder 类
相比于 String 这种不可变的字符串，在进行字符串拼接需要在内存中重新创建一个对象，StringBuilder 类底层不是使用 final 类修饰的类似 String 类缓冲区，增加过程中直接属于字符串数组
当数组空间不足时，自动扩容初始长度为 16
### 构造方法
new StingBuilder()  
new StingBuilder(String str) // 可以使用这个构造方法进行 String 向 StringBuilder 的转换
### 常用方法
常用方法有两个
- public StringBuilder append(...)：添加任意类型数据的字符串形式，并返回当前对象自身。满足链式编程
- public String toString()：将当前StringBuilder对象转换为String对象。
#### append方法
append方法具有多种重载形式，可以接收任意类型的参数。任何数据作为参数都会将对应的字符串内容添加到StringBuilder中。例如：

```java
public class Demo02StringBuilder {
	public static void main(String[] args) {
		//创建对象
		StringBuilder builder = new StringBuilder();
		//public StringBuilder append(任意类型)
		StringBuilder builder2 = builder.append("hello");
		//对比一下
		System.out.println("builder:"+builder);
		System.out.println("builder2:"+builder2);
		System.out.println(builder == builder2); //true
	    // 可以添加 任何类型
		builder.append("hello");
		builder.append("world");
		builder.append(true);
		builder.append(100);
		// 在我们开发中，会遇到调用一个方法后，返回一个对象的情况。然后使用返回的对象继续调用方法。
        // 这种时候，我们就可以把代码现在一起，如append方法一样，代码如下
		//链式编程
		builder.append("hello").append("world").append(true).append(100);
		System.out.println("builder:"+builder);
	}
}
```
> tips：StringBuilder已经覆盖重写了Object当中的toString方法。
#### toString 方法
通过toString方法，StringBuilder对象将会转换为不可变的String对象。如：

```java
public class Demo16StringBuilder {
    public static void main(String[] args) {
        // 链式创建
        StringBuilder sb = new StringBuilder("Hello").append("World").append("Java");
        // 调用方法
        String str = sb.toString();
        System.out.println(str); // HelloWorldJava
    }
}
```

## 包装类

Java提供了两个类型系统，基本类型与引用类型，使用基本类型在于效率，然而很多情况，会创建对象使用，因为对象可以做更多的功能，如果想要我们的基本类型像对象一样操作，就可以使用基本类型对应的包装类，如下：

| 基本类型    | 对应的包装类（位于java.lang包中） |
| ------- | --------------------- |
| byte    | Byte                  |
| short   | Short                 |
| int     | **Integer**           |
| long    | Long                  |
| float   | Float                 |
| double  | Double                |
| char    | **Character**         |
| boolean | Boolean               |

### 装箱与拆箱

基本类型与对应的包装类对象之间，来回转换的过程称为”装箱“与”拆箱“：

* **装箱**：从基本类型转换为对应的包装类对象。

* **拆箱**：从包装类对象转换为对应的基本类型。

用Integer与 int为例：（看懂代码即可）

基本数值---->包装对象

~~~java
Integer i = new Integer(4);//使用构造函数函数
Integer iii = Integer.valueOf(4);//使用包装类中的valueOf方法
~~~

包装对象---->基本数值

~~~java
int num = i.intValue();
~~~
### 自动装箱与自动拆箱

由于我们经常要做基本类型与包装类之间的转换，从Java 5（JDK 1.5）开始，基本类型与包装类的装箱、拆箱动作可以自动完成。例如：

```java
Integer i = 4;//自动装箱。相当于Integer i = Integer.valueOf(4);
i = i + 5;//等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5;
//加法运算完成后，再次装箱，把基本数值转成对象。
```


### 基本类型与字符串之间的转换

#### 基本类型转换为String

   基本类型转换String总共有三种方式，查看课后资料可以得知，这里只讲最简单的一种方式： 

~~~
基本类型直接与””相连接即可；如：34+""
~~~

String转换成对应的基本类型 

除了Character类之外，其他所有包装类都具有parseXxx静态方法可以将字符串参数转换为对应的基本类型：

- `public static byte parseByte(String s)`：将字符串参数转换为对应的byte基本类型。
- `public static short parseShort(String s)`：将字符串参数转换为对应的short基本类型。
- `public static int parseInt(String s)`：将字符串参数转换为对应的int基本类型。
- `public static long parseLong(String s)`：将字符串参数转换为对应的long基本类型。
- `public static float parseFloat(String s)`：将字符串参数转换为对应的float基本类型。
- `public static double parseDouble(String s)`：将字符串参数转换为对应的double基本类型。
- `public static boolean parseBoolean(String s)`：将字符串参数转换为对应的boolean基本类型。

代码使用（仅以Integer类的静态方法parseXxx为例）如：

```java
public class Demo18WrapperParse {
    public static void main(String[] args) {
        int num = Integer.parseInt("100");
    }
}
```

> 注意:如果字符串参数的内容无法正确转换为对应的基本类型，则会抛出`java.lang.NumberFormatException`异常。
>



## 内部类
如果对象和对象间存在一种内部包含的状态需要试用内部类

内部类分为两种，成员内部类和局部内部类（匿名内部类）

成员内部类的使用：<br >
1. 间接方式：在外部类方法中使用内部类的方法，外部类方法被调用时
2. 直接方式：外部类名称.内部类名称 对象名 = new 外部类（）.new 内部类（）


**注意：**  
定义类时的权限修饰符规则：
1. 外部类：public / （default）
2. 成员内部类：public / protected /（default） /private
3. 局部内部类：均不允许写
局部内部类：
1。如果希望访问所在方法中的局部变量，局部变量必须是有效 final。（生成空间）  

## 可变参数

在**JDK1.5**之后，如果我们定义一个方法需要接受多个参数，并且多个参数类型一致，我们可以对其简化成如下格式：

```
修饰符 返回值类型 方法名(参数类型... 形参名){  }
```

其实这个书写完全等价与

```
修饰符 返回值类型 方法名(参数类型[] 形参名){  }
```

只是后面这种定义，在调用时必须传递数组，而前者可以直接传递数据即可。

**JDK1.5**以后。出现了简化操作。**...** 用在参数上，称之为可变参数。

同样是代表数组，但是在调用这个带有可变参数的方法时，不用创建数组(这就是简单之处)，直接将数组中的元素作为实际参数进行传递，其实编译成的class文件，将这些元素先封装到一个数组中，在进行传递。这些动作都在编译.class文件时，自动完成了。

代码演示：    

```java
public class ChangeArgs {
    public static void main(String[] args) {
        int[] arr = { 1, 4, 62, 431, 2 };
        int sum = getSum(arr);
        System.out.println(sum);
        //  6  7  2 12 2121
        // 求 这几个元素和 6  7  2 12 2121
        int sum2 = getSum(6, 7, 2, 12, 2121);
        System.out.println(sum2);
    }
    /*
     * 完成数组  所有元素的求和 原始写法
     
      public static int getSum(int[] arr){
        int sum = 0;
        for(int a : arr){
            sum += a;
        }
        return sum;
      }
    */
    //可变参数写法
    public static int getSum(int... arr) {
        int sum = 0;
        for (int a : arr) {
            sum += a;
        }
        return sum;
    }
}
```

> tips: 上述add方法在同一个类中，只能存在一个。因为会发生调用的不确定性
>
> 注意：如果在方法书写时，这个方法拥有多参数，参数中包含可变参数，可变参数一定要写在参数列表的末尾位置。




## 异常
异常机制其实是帮助我们**找到**程序中的问题，异常的根类是`java.lang.Throwable`，其下有两个子类：`java.lang.Error`与`java.lang.Exception`，平常所说的异常指`java.lang.Exception`。
Throwables 的体系分为两种：
- **Error**:严重错误Error，无法通过处理的错误，只能事先避免，好比绝症。
- **Exception**:表示异常，异常产生后程序员可以通过代码的方式纠正，使程序继续运行，是必须要处理的。好比感冒、阑尾炎。
![Excetpin-异常体系](./img/Java/Excetpin-异常体系.png)
主要分为两大类编译期异常或者运行期异常，也就是**受检异常**和**非受检异常**

- **编译时期异常**:checked异常。在编译时期,就会检查,如果没有处理异常,则编译失败。(如日期格式化异常)
- **运行时期异常**:runtime异常。在运行时期,检查异常.在编译时期,运行异常不会编译器检测(不报错)。(如数学异常)
![Excetpin-异常的分类](./img/Java/Excetpin-异常的分类.png)

## throw 关键字
使用 throw 关键字在指定的方法中抛出指定的异常
使用格式：throw new xxxException("the reason of Exceptions!")
> tips：   
        1、throw 关键字体必须在方法内部  
        2、throw 关键字后面 new 的对象必须是 Exception 或者 Exception 的子类对象  
        3、throw 关键字抛出异常对象后，就必须处理这个异常对象      
        throws 关键字后面创建的对象是 RuntimeException 或者是 RuntimeException 的子类对象，我们可以不做处理，默认交给JVM处理（打印和中断）  
        throws 关键字后面创建的是编译异常的话，需要处理异常，使用 throws 或者 try...catch
        