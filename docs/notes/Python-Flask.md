


**修饰器**
使用嵌套函数的形式对执行函数进行抓取对应的执行状态，主要是一种抓取 log 的方式。

```python
def log(level, *arg, **kvargs):
    def inner(func):
        def wrapper(*arg, **kvargs):
        '''dd'''
        print('1')
        func(*arg, **kvargs)
        print('2')
```

---
**routing**
* 多映射
* 参数变量，/结尾自动补齐

```python

# -*- encoding = UTF-8 -*-
from flask import Flask
app = Flask(__name__)

@app.route("/")
@app.route('/index/')  #参数自动补全
def hello():    
    return "Hello World!"
@app.route('/profile/<int:uid>/', methods=['GET', 'post'])
def profile(uid):
    return 'profile:' + str(uid)
 
```


---


HTTP Method

* Get： 获取接口信息
* HEAD：紧急查看接口HTTP的头
* POST：提交数据到服务器
* PUT：支持幂等性的POST
* DELETE：删除服务器的资源
* OPTITONS：查看支持的方法

---

* **jinja2 语法使用**

[官方文档](http://docs.jinkan.org/docs/jinja2/)
![390fbd33521c0bb76a90e2cd729ae6ca.png](en-resource://database/431:1)
页面展示

---


**requests / reponse**

网页的网文核心就是这两个，请求和返回需要导入的库有 request、make_response 



---
**重定向/Error**
```py

@app.route('/newpath')
def newpath():
    return 'newpath'
 @app.route('/re/<int:code>')
 def redirect_demo(code):
    return redirect('/newpath', code=code)


```



---
##### Flash Message
页面通过 flash 的展示



##### logs
