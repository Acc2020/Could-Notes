
[toc]

## 前言


# CSS   
---
Cascading Style Sheets 层叠样式表 ,它用于设置页面的表现。  
层叠：多个样式可以作用在同一个html的元素上，同时生效
* 降低耦合度。解耦
* 让分工协作更容易
* 提高开发效率


###CSS的使用：CSS与html结合方式
1. 内联样式
2. 内部样式
3. 外部样式
```html
// 外部样式表
<head>
  <link rel="stylesheet" type="text/css" href="style.css">
</head>
// 内部样式表
<head>
  <style type="text/css">
    p {
      margin: 10px;
    }
  </style>
</head>
// 内嵌样式(可在动态效果中同 JavaScript 一同使用)
<p style="color: red">Sample Text</p>

<style>
    @import "css/a.css";
</style>
```
### 语法
```html
css语法：
* 格式：
    选择器 {
        属性名1:属性值1;
        属性名2:属性值2;
        ...
    }
* 选择器:筛选具有相似特征的元素
* 注意：
    * 每一对属性需要使用；隔开，最后一对属性可以不加；
```

### 选择器
选择器可被看做表达式，通过它可以选择相应的元素并应用不同的样式。
- 简单选择器
- 元素选择器
- 组合选择器
- 简单选择器
- 简单选择器可组合使用。

**标签选择器**
```html
<div>
  <p>Sample Paragraph</p>
  <p>Sample Paragraph</p>
  <p>Sample Paragraph</p>
</div>

<style type="text/css">
  p {
    color: blue;
  }
</style>
```
**类选择器**  
.className 以 . 开头，名称可包含字母，数字，-，_，但必须以字母开头。它区分大小写并可出现多次。
```html
<div>
  <p>Sample Paragraph</p>
  <p class="special bold">Sample Paragraph</p>
  <p>Sample Paragraph</p>
</div>

<style type="text/css">
  p {
    color: blue
  }
  .special {
    color: orange;
  }
  .bold {
    font-weight: bold;
  }
</style>
```
**id 选择器**    
 #idName 以 # 开头且只可出现一次，其命名要求于 .className 相同。
```html
<div>
  <p id="special">Sample Paragraph</p>
</div>

<style type="text/css">
  #special {
    color: red;
  }
</style>
```
**通配符选择器**
```html
<div>
  <p>Sample Paragraph</p>
  <p>Sample Paragraph</p>
</div>

<style type="text/css">
  * {
    color: blue;
  }
</style>
```
**属性选择器**
```html
<!--
[attr] 或 [attr=val] 来选择相应的元素。#nav{...} 既等同于 [id=nav]{...}。IE7+

[attr~=val] 可选用与选择包含 val 属性值的元素，像class="title sports" 与 class="sports"。.sports{...} 既等同于 [class~=sports]{...} IE7+

[attr|=val] 可以选择val开头及开头紧接-的属性值。IE7+

[attr^=val] 可选择以val开头的属性值对应的元素，如果值为符号或空格则需要使用引号 ""。IE7+

[attr$=val] 可选择以val结尾的属性值对应的元素。IE7+

[attr*=val] 可选择以包含val属性值对应的元素。IE7+
-->
<div>
  <form action="">
    <input type="text" value="Xinyang" disabled>
    <input type="password" placeholder="Password">
    <input type="button" value="Button">
  </form>
</div>
<style type="text/css">
  [disabled] {
    background-color: orange;
  }
  [type=button] {
    color: blue;
  }
</style>
```
