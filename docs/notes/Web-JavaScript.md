[toc]
## 前言


# JavaScript

客户端脚本语言(控制网页行为)  
* 运行在客户端浏览器中的。每一个浏览器都有JavaScript的解析引擎
* 脚本语言：不需要编译，直接就可以被浏览器解析执行了
* 可以来增强用户和html页面的交互过程，可以来控制html元素，让页面有一些动态的效果，增强用户的体验。

**JavaScript发展史：**
1. 1992年，Nombase公司，开发出第一门客户端脚本语言，专门用于表单的校验。命名为 ： C--	，后来更名为：ScriptEase
2. 1995年，Netscape(网景)公司，开发了一门客户端脚本语言：LiveScript。后来，请来SUN公司的专家，修改LiveScript，命名为JavaScript
3. 1996年，微软抄袭JavaScript开发出JScript语言
4. 1997年，ECMA(欧洲计算机制造商协会)，制定出客户端脚本语言的标准：ECMAScript，就是统一了所有客户端脚本语言的编码方式。
![Web-javascript-history.png](./img/web/Web-javascript-history.png)
![web-javascript-env.png](./img/web/web-javascript-env.png)


# ECMAScript
### 基本语法
**数据类型**  
``` javascript
1. 原始数据类型(基本数据类型)：
    1. number：数字。 整数/小数/NaN(not a number 一个不是数字的数字类型)
    2. string：字符串。 字符串  "abc" "a" 'abc'
    3. boolean: true和false
    4. null：一个对象为空的占位符
    5. undefined：未定义。如果一个变量没有给初始化值，则会被默认赋值为undefined
    
2. 引用数据类型：对象
```
**变量**
* 变量：一小块存储数据的内存空间
* Java语言是强类型语言，而JavaScript是弱类型语言。
    * 强类型：在开辟变量存储空间时，定义了空间将来存储的数据的数据类型。只能存储固定类型的数据
    * 弱类型：在开辟变量存储空间时，不定义空间将来的存储数据类型，可以存放任意类型的数据。
* 语法：
    * var 变量名 = 初始化值;

* typeof运算符：获取变量的类型。
    * 注：null运算后得到的是object


**关键字与保留字**  
JavaScript 在语言定义中保留的字段，这些字段在语言使用中存在特殊意义或功能，在程序编写的过程中不可以当做变量或函数名称使用。无需记忆，报错修改即可。  


**字符敏感**  
字符串的大小写是有所区分的，不同字符指代不同的变量。