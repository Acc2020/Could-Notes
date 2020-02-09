
[toc]



 - - -
# Servlet 
Java Web 的一个容器或规范（接口），能够接收浏览器的请求并做出响应。  
[百度百科:Servlet](https://baike.baidu.com/item/servlet/477555?fr=aladdin)  
一个Servlet的运行过程：  
1、 Web 接收到服务器的请求并转发给 servlet 容器
2、查看对应的配置文件（web.xml）或者注解，看浏览器访问的对应 servlet 有没有存在，不存在返回 404  
3、根据请求查看对应的配置文件或者注解，看对应的 Servlet 是否已经被实例化，若是相应的Servlet没有被实例化，则容器将会加载相应的Servlet到Java虚拟机并实例化
4、调用 service 方法，开启一个新的线程进行执行
## BS结构/CS结构 
**软件架构：**  
- C/S: Client/Server 客户端/服务器端  
需要本地有客户端，安装部署升级比较麻烦
- B/S: Browser/Server 浏览器/服务器端  
直接通过浏览器去访问即可，易于维护升级，对服务器硬件要求比较高

**B/S 架构资源分类：**  
1.静态资源：如果用户请求的是静态资源，那么服务器会直接将静态资源发送给浏览器。浏览器中内置了静态资源的解析引擎，可以展示静态资源。  
2.动态资源：如果用户请求的是动态资源，那么服务器会执行动态资源，转换为静态资源，再发送给浏览器。
## Servlet 的体系结构
![](./img/web/web-servlet.png)  

	Servlet -- 接口
		|
	GenericServlet -- 抽象类
		|
	HttpServlet  -- 抽象类
    

	* GenericServlet：将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象
		* 将来定义Servlet类时，可以继承GenericServlet，实现service()方法即可

	* HttpServlet：对http协议的一种封装，简化操作
		1. 定义类继承HttpServlet
		2. 复写doGet/doPost方法

**通过idea创建一个 Servlet**
```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebServlet(name = "servlet1", urlPatterns = "/servlet/demo1")
public class Servlet1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
      //添加对应的逻辑代码
        System.out.println(getServletConfig().getServletName());

        System.out.println(getServletInfo());

        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=utf-8");
        response.getWriter().print("这是一个Srvlet Demo"); // 输出
    //  request.getRequestDispatcher("http://www.baidu.com").forward(request, response); //转发
    //  response.sendRedirect("http://www.baidu.com"); // 重定向  
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}

```



## request
### 请求转发-能够在服务器内部实现资源跳转
通过 requset 对象获取请求转发器对象：getRequestDispatcher(String path)
使用 RequestDispatcher 对象调用 forward 方法进行转发
```java
 request.getRequestDispatcher("requestpelay").forward(request,response);
```
> tips:转发过程中浏览器地址栏路径不会发生改变  
>只能转发当前服务器中的资源  
>转发是一次请求


# 会话技术
1、会话：一次会话中包含多次请求和响应  
   从会话开始到会话结束的时间段，直到一方断开会话，会话结束  
   会话功能：在一次的会发范围内的多次请求实现数据共享  
2、会话方式  
 - 客户端会话技术：Cookie  
 - 服务器端会话技术：Session
    
## Cookie
将数据保存的客户端，
