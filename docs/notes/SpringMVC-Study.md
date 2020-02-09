# SpringMVC 学习笔记

## 回顾 MVC 

- MCV 是模型（Model）、视图（View）、控制器（Controller）属于软件的设计规范

- 能够将业务逻辑、数据、显示分离的方法来组织代码

- 作用是为了**能够降低视图与业务逻辑间的双向耦合**

- MVC 不是设计模式，**MVC 是一种架构模式**，不同形式的 MVC 存在差异

	**Model（模型）：** 数据模型，主要是为了展示数据，包含数据和行为，主要是 JavaBean 组件，现在一般都进行分离：Value Object （数据 Dao） 和 服务层（行为 Service）。模型提供了数据查询和模型数据更新，包括数据和业务。

  **View（视图）：** 负责对模型的展示，一般的用户界面，主要是一些客户需要看到的东西。
  
  **Controller（控制器）：** 接受用户请求，并把请求委托给模型进行处理，处理完毕后把返回的数据传送到视图层，由视图进行展示。
  
  **常见的 MVC 是 JSP + servlet + JavaBean 模式**  

  ![springmvc-mvc-01.png.png](http://images.vsnode.com/springmvc-mvc-01.png.png)





### JavaWeb 发展

#### Model 1

JavaWeb 发展是围绕 Model 1 和 Model 2 来开展的，Model 1 只要是早期的 JSP 小型网站，主要使用这种模式。把所有的请求也业务过程都放在了 JSP 中，增加了后期维护的困难。

![springmvc-mvc-02.png](http://images.vsnode.com/springmvc-mvc-02.png)
#### Model 2

Model 是在 Model 1 的基础上增加了 Servlet 去接受用户的请求，serlvet 中包含控制器逻辑，和简单的前端处理，最后 JavaBean 完成最终的实际逻辑处理，然后再转发到 JSP 显示。

![springmvc-mvc-03.png.png](http://images.vsnode.com/springmvc-mvc-03.png.png)

