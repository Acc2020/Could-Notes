[toc]
#  java.io 
通过数据流、序列化和文件系统提供系统输入和输出。

根据数据的流向分为：**输入流**和**输出流**。

- **输入流** ：把数据从`其他设备`上读取到`内存`中的流。 
- **输出流** ：把数据从`内存` 中写出到`其他设备`上的流。

格局数据的类型分为：**字节流**和**字符流**。

- **字节流** ：以字节为单位，读写数据的流。
- **字符流** ：以字符为单位，读写数据的流。
![IO-IO.png](./img/java/IO-IO.png)
- **File**（磁盘操作）
- **InputStream/OutputStream** (字节操作)
- **Reader/Writer** (字符操作)
- **Buffered(////)** （缓存流）
    * **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream` 
    * **字符缓冲流**：`BufferedReader`，`BufferedWriter`

## File 
File` 类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作。


```java
public class FileFor {
    public static void main(String[] args) {
        File dir = new File("d:\\java_code");
      
      	//获取当前目录下的文件以及文件夹的名称。
		String[] names = dir.list();
		for(String name : names){
			System.out.println(name);
		}
        //获取当前目录下的文件以及文件夹对象，只要拿到了文件对象，那么就可以获取更多信息
        File[] files = dir.listFiles();
        for (File file : files) {
            System.out.println(file);
        }
    }
}
```

> tips：
>
> 调用listFiles方法的File对象，表示的必须是实际存在的目录，否则返回null，无法进行遍历。

## InputStream/OutputStream (字节流)
一切文件数据(文本、图片、视频等)在存储时，都是以二进制数字的形式保存，都一个一个的字节。字节流可以传输任意文件数据。在操作流的时候，无论使用什么样的流对象，底层传输的始终为二进制数据。

```java
public static void copyFile(String src, String dist) throws IOException {
    FileInputStream fis = new FileInputStream(sec);
    FileOutputStream fos = new FileOutputStream(dist);
    byte[] bs = new byte[n*1024];
    int len;
    while ((len = fis.read(bs)!=-1)){
        fos.Writer(bs,0,len);
    }
    fis.close();
    fos.close();
}

```


## Reader/Writer (字符操作)
由于考虑的字符的编码问题汉字的编码在 UTF-8 中占3个字节，如果使用字节流就无法很好的显示对应表示的汉字，就需要字符操作（Reader/Writer）完成对字符的操作，但是除了字符无法对图片、视频等进行操作
> tips:
>1. 字符编码：字节与字符的对应规则。Windows系统的中文编码默认是GBK编码表。
>idea中UTF-8
>2. 字节缓冲区：一个字节数组，用来临时存储字节数据。

```java
public class FWWrite {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象
        FileWriter fw = new FileWriter("fw.txt");     
      	// 字符串转换为字节数组
      	char[] chars = "字节操作".toCharArray();
      
      	// 写出字符数组
      	fw.write(chars); // 字节操作
        
		// 写出从索引2开始，2个字节。
        fw.write(b,2,2); // 操作
      
      	// 关闭资源
        fos.close();
    }
}
```

### 关闭和刷新

因为内置缓冲区的原因，如果不关闭输出流，无法写出字符到文件中。但是关闭的流对象，是无法继续写出数据的。如果我们既想写出数据，又想继续使用流，就需要`flush` 方法了。

- `flush` ：刷新缓冲区，流对象可以继续使用。
- `close` ：先刷新缓冲区，然后通知系统释放资源。流对象不可以再被使用了。

## Properties (属性集)
`java.util.Properties ` 继承于` Hashtable` ，来表示一个持久的属性集。<br>
    它使用键值结构存储数据，每个键及其对应值都是一个字符串。<br>
    `System.getProperties` 方法就是返回一个`Properties`对象。<br>
    
```java

/*
void store(OutputStream out, String comments)
        以适合使用 load(InputStream) 方法加载到 Properties 表中的格式，将此 Properties 表中的属性列表（键和元素对）写入输出流。
        void store(Writer writer, String comments)
        以适合使用 load(Reader) 方法的格式，将此 Properties 表中的属性列表（键和元素对）写入输出字符。
*/
public class DemoPropStore {
    public static void main(String[] args) throws IOException {
        // 创建属性集对象
        Properties prop = new Properties();
        //添加元素
        prop.setProperty("玛依拉","12");
        prop.setProperty("李钟硕","20");
        prop.setProperty("王世锦","32");
        //打印元素
        System.out.println(prop);

        //加载文本到属性集中
        prop.store((new FileWriter(src)),"save data");

//        FileWriter fw = new FileWriter(src);
        prop.store(new FileOutputStream(src),"save data");
    }
}

```
## 缓冲流（Buffered）
缓冲流,也叫高效流，是对4个基本的`FileXxx` 流的增强，所以也是4个流，按照数据类型分类：

- **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream` 
- **字符缓冲流**：`BufferedReader`，`BufferedWriter`

缓冲流的基本原理，是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率。

### 字节缓冲流
```java
public class BufferedDemo {
    public static void main(String[] args) throws FileNotFoundException {
      	// 记录开始时间
        long start = System.currentTimeMillis();
		// 创建流对象
        try (
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream(src));
		 BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(dist);
        ){
          	// 读写数据
            int len;
            byte[] bytes = new byte[8*1024];
            while ((len = bis.read(bytes)) != -1) {
                bos.write(bytes, 0 , len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
		// 记录结束时间
        long end = System.currentTimeMillis();
        System.out.println("缓冲流使用数组复制时间:"+(end - start)+" 毫秒");
    }
}
//可通过对比常规 Filexx 流进行对比
```
### 字符缓冲流
```java
public class BufferedWriterDemo throws IOException {
    public static void main(String[] args) throws IOException  {
      	// 创建流对象
		BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"));
      	// 写出数据
        for(int i=0; i<10; i++){
            bw.write("hello git");
            bw.newLine();
        }
        bw.close();
    }
}
```

## InputStreamReader/OutputStreamWriter（转换流）
### 字符编码
计算机中储存的信息都是用二进制数表示的，而我们在屏幕上看到的数字、英文、标点符号、汉字等字符是二进制数转换之后的结果。按照某种规则，将字符存储到计算机中，称为**编码** 。反之，将存储在计算机中的二进制数按照某种规则解析显示出来，称为**解码** 。比如说，按照A规则存储，同样按照A规则解析，那么就能显示正确的文本符号。反之，按照A规则存储，再按照B规则解析，就会导致乱码现象。

编码:字符(能看懂的)--字节(看不懂的)

解码:字节(看不懂的)-->字符(能看懂的)

- **字符编码`Character Encoding`** : 就是一套自然语言的字符与二进制数之间的对应规则。

  编码表:生活中文字和计算机中二进制的对应规则

### 字符集

- **字符集 `Charset`**：也叫编码表。是一个系统支持的所有字符的集合，包括各国家文字、标点符号、图形符号、数字等。

![IO-charset.png](./img/java/IO-charset.png)

### InputStreamReader
  转换流`java.io.InputStreamReader`，是Reader的子类，是从字节流到字符流的桥梁。它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

**指定编码读取**
```java
public class Reader {
    public static void main(String[] args) throws IOException {
      	// 定义文件路径,文件为gbk编码
        String FileName = src;
      	// 创建流对象,默认UTF8编码
        InputStreamReader isr = new InputStreamReader(new FileInputStream(FileName));
      	// 创建流对象,指定GBK编码
        InputStreamReader isr2 = new InputStreamReader(new FileInputStream(FileName) , "GBK");
		// 定义变量,保存字符
        int read;
      	// 使用默认编码字符流读取,乱码
        while ((read = isr.read()) != -1) {
            System.out.print((char)read); // ��Һ�
        }
        isr.close();
      
      	// 使用指定编码字符流读取,正常解析
        while ((read = isr2.read()) != -1) {
            System.out.print((char)read);// 大家好
        }
        isr2.close();
    }
}
```
### OutputStreamWriter
转换流`java.io.OutputStreamWriter` ，是Writer的子类，是从字符流到字节流的桥梁。使用指定的字符集将字符编码为字节。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

**指定编码写出**
```java
public class Output {
    public static void main(String[] args) throws IOException {
      	// 定义文件路径
        String FileName = src;
      	// 创建流对象,默认UTF8编码
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(FileName));
        // 写出数据
      	osw.write("你好"); // 保存为6个字节
        osw.close();
      	
		// 定义文件路径
		String FileName2 = "E:\\out2.txt";
     	// 创建流对象,指定GBK编码
        OutputStreamWriter osw2 = new OutputStreamWriter(new FileOutputStream(FileName2),"GBK");
        // 写出数据
      	osw2.write("你好");// 保存为4个字节
        osw2.close();
    }
}
```
**转换流是字节与字符间的桥梁！**
![IO-字节-字符.png](./img/java/IO-字节-字符.png)

## ObjectOutputStream/ObjectInputStream（序列化）
Java 提供了一种对象**序列化**的机制。用一个字节序列可以表示一个对象，该字节序列包含该`对象的数据`、`对象的类型`和`对象中存储的属性`等信息。字节序列写出到文件之后，相当于文件中**持久保存**了一个对象的信息。 

反之，该字节序列还可以从文件中读取回来，重构对象，对它进行**反序列化**。`对象的数据`、`对象的类型`和`对象中存储的数据`信息，都可以用来在内存中创建对象。

![IO-字节-对象.png](./img/java/IO-字节-对象.png)

### ObjectOutputStream类






## 打印流

平时我们在控制台打印输出，是调用`print`方法和`println`方法完成的，这两个方法都来自于`java.io.PrintStream`类，该类能够方便地打印各种数据类型的值，是一种便捷的输出方式。
### PrintStream类

```java
PrintStream ps = new PrintStream("ps.txt")；
```

### 改变打印流向

`System.out`就是`PrintStream`类型的，只不过它的流向是系统规定的，打印在控制台上。不过，既然是流对象，我们就可以玩一个"小把戏"，改变它的流向。

```java
public class PrintDemo {
    public static void main(String[] args) throws IOException {
		// 调用系统的打印流,控制台直接输出97
        System.out.println(97);
      
		// 创建打印流,指定文件的名称
        PrintStream ps = new PrintStream("ps.txt");    	
      	// 设置系统的打印流流向,输出到ps.txt
        System.setOut(ps);
      	// 调用系统的打印流,ps.txt中输出97
        System.out.println(97);
    }
}
```

## 网络通信
一个正常的网络通信
1. 【服务端】启动,创建ServerSocket对象，等待连接。
2. 【客户端】启动,创建Socket对象，请求连接。
3. 【服务端】接收连接,调用accept方法，并返回一个Socket对象。
4. 【客户端】Socket对象，获取OutputStream，向服务端写出数据。
5. 【服务端】Scoket对象，获取InputStream，读取客户端发送的数据。

> 到此，客户端向服务端发送数据成功。

![IO-简单网络通信.png](./img/java/IO-简单网络通信.png)

> 自此，服务端向客户端回写数据。

6. 【服务端】Socket对象，获取OutputStream，向客户端回写数据。
7. 【客户端】Scoket对象，获取InputStream，解析回写数据。
8. 【客户端】释放资源，断开连接。
