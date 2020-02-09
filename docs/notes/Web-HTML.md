
[toc]

##前言
java web：<br /> 
使用 java 开发的基于互联网的项目，遵循 web 标准。  
web 标准介绍
- w3c：[World Wide Web Consortium（万维网联盟）HTML、XHTML、CSS、XML的标准就是由W3C来定制。](https://baike.baidu.com/item/W3C组织)  
- web标准：制作网页的规范，分为结构标准，表现标准，行为标准  
- 结构：html 表现：css 行为:JavasCript  
 

浏览器内核：

|浏览器|内核|
|---|---|
|IE	|trident|
|chrome / 欧鹏|	blink
|火狐	|gecko
|Safari	|webkit
tips：「浏览器内核」也就是浏览器所采用的「渲染引擎」，渲染引擎决定了浏览器如何显示网页的内容以及页面的格式信息。渲染引擎是兼容性问题出现的根本原因。
# 一、html
--- 
**Hyper Text Markup Language 超文本标记语言**
* 超文本:
    * 超文本是用超链接的方法，将各种不同空间的文字信息组织在一起的网状文本.
* 标记语言:
    * 由标签构成的语言。<标签名称> 如 html，xml
    * 标记语言不是编程语言

**HTML的网络术语**  
- 网页 ：由各种标记组成的一个页面就叫网页。
- 主页(首页) : 一个网站的起始页面或者导航页面。
- 标记：< p>称为开始标记 ，</ p>称为结束标记，也叫标签。每个标签都规定好了特殊的含义。
- 元素：< p>内容</ p>称为元素.
- 属性：给每一个标签所做的辅助信息。
- xhtml： 符合XML语法标准的HTML。
- dhtml：dynamic，动态的。javascript + css + html合起来的页面就是一个dhtml。
- http：超文本传输协议。用来规定客户端浏览器和服务端交互时数据的一个格式。
- SMTP：邮件传输协议 
- ftp：文件传输协议。


### 语法：
1. html文档后缀名 .html 或者 .htm
2. 标签分为
    1. 围堵标签：有开始标签和结束标签。如 <html> </html>
    2. 自闭和标签：开始标签和结束标签在一起。如 <br/>

3. 标签可以嵌套：
    需要正确嵌套，不能你中有我，我中有你
    错误：<a><b></a></b>
    正确：<a><b></b></a>

4. 在开始标签中可以定义属性。属性是由键值对构成，值需要用引号(单双都可)引起来
5. html的标签不区分大小写，但是建议使用小写。

```html
<html>
    <head>
        <title>title</title>
    </head>
    <body>
        <FONT color='red'>Hello World</font><br/>
        <font color='green'>Hello World</font>
    </body>
</html>
```

### HTML标签

[HTML5 标签集合](http://www.html5star.com/manual/html5label-meaning/)  
![web-html01.png](./img/web/web-html01.png)
**文档章节**
```html
<body> 页面内容 <header> 文档头部 <nav> 导航 <aside> 侧边栏 <article> 定义外部内容（如外部引用的文章） <section> 一个独立的块 <footer> 尾部

```
**页面通常结构**  
![web-html01.png](./img/web/web-html02.png)

**文本标签**  
```html
<!-- 默认超链接  -->
<a href="http://sample-link.com" title="Sample Link">Sample</a>
<!-- 当前窗口显示 -->
<a href="http://sample-link.com" title="Sample Link" target="_self">Sample</a>
<!-- 新窗口显示 -->
<a href="http://sample-link.com" title="Sample Link" target="_blank">Sample</a>
<!-- iframe 中打开链接 -->
<a href="http://sample-link.com" title="Sample Link" target="iframe-name">Sample</a>
<iframe name="iframe-name" frameborder="0"></iframe>

<!-- 页面中的锚点 -->
<a href="#achor">Achor Point</a>
<section id="achor">Achor Content</section>

<!-- 邮箱及电话需系统支持 -->
<a href="mailto:sample-address@me.com" title="Email">Contact Us</a>
<!-- 多个邮箱地址 -->
<a href="mailto:sample-address@me.com, sample-address0@me.com" title="Email">Contact Us</a>
<!-- 添加抄送，主题和内容 -->
<a href="mailto:sample-address@me.com?cc=admin@me.com&subject=Help&body=sample-body-text" title="Email">Contact Us</a>

<!-- 电话示例 -->
<a href="tel:99999999" title="Phone">Ring Us</a>
```

**组合内容标签**
- < div>
- < p>
- < ol>
- < ul>
- < dl>
- < pre>
- < blockquote>

**文档章节**
```html
<body> 页面内容 <header> 文档头部 <nav> 导航 <aside> 侧边栏 <article> 定义外部内容（如外部引用的文章） <section> 一个独立的块 <footer> 尾部
```

**引用**
- < cite> 引用作品的名字、作者的名字等
- < q> 引用一小段文字（大段文字引用用< blockquote>）
- < blockquote> 引用大块文字
- < pre> 保存格式化的内容（其空格、换行等格式不会丢失）
```html
<pre>
  <code>
    int main(void) {
      printf('Hello, world!');
      return 0;
  }
</code>
</pre>
```

**表单**
```html

<form action="WebCreation_submit" method="get" accept-charset="utf-8">
  <fieldset>
    <legend>title or explanatory caption</legend>
    <!-- 第一种添加标签的方法 -->
    <label><input type="text/submit/hidden/button/etc" name="" value=""></label>
    <!-- 第二种添加标签的方法 -->
    <label for="input-id">Sample Label</label>
    <input type="text" id="input-id">
  </fieldset>
  <fieldset>
    <legend>title or explanatory caption</legend>
    <!-- 只读文本框 -->
    <input type="text" readonly>
    <!-- 隐藏文本框，可提交影藏数据 -->
    <input type="text" name="hidden-info" value="hiden-info-value" hidden>
  </fieldset>
  <button type="submit">Submit</button>
  <button type="reset">Reset</button>
</form>
```


### 实体字符

实体字符（ASCII Encoding Reference）是用来在代码中以实体代替与HTML语法相同的字符，避免浏览解析错误。它的两种表示方式，第一种为 & 外加实体字符名称，例如 &nbsp;，第二种为 & 加实体字符序号，例如 &#160;。

![web-特殊字符表.png](./img/web/web-特殊字符表.png)



### 浏览器兼容
主流浏览器都兼容 HTML5 的新标签，对于 IE8 及以下版本不认识 HTML5的新元素，可以使用 JavaScript 创建一个没用的元素来解决，例如：
```JavaScript
<script>
    document.createElement("header");
</script>
```
也可以使用 shiv 来解决兼容性问题，详情可参考 [HTML5 Shiv](https://github.com/afarkas/html5shiv)