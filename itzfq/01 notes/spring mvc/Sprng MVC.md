# Spring MVC
## Spring MVC 概念

> 什么是Spring MVC?

​	概念: Spring MVC 框架是基于Spring Framework衍生而来的一个MVC框架.主要解决了原有MVC框架开发过程中,控制器(Controlle) 的问题

> Spring MVC的优点

​	MVC是一个架构思想,在Java开发中多用于Web开发

​	使用MVC架构思想会把程序分为3大部分, M 模型层 V 视图层 C 控制层

> Spring MVC 为什么要基于Spring Framework

* 通过工厂(容器)创建对象,解耦合
* 通过AOP,为目标类添加额外功能
* 方便与第三方框架整合

> 原有MVC开发中的控制器是通过哪些技术来实现的?

* Servlet 基于 Model2模式
* Struts2中的Action

> Servlet 实现控制器存在的问题

* 控制器的作用

  ```markdown
  1. 接收用户请求
  2. 调用业务功能 
  3. 根据处理结果控制程序的运行流程
  ```

* 控制器的核心代码

  ```markdown
  1. 接收用户请求参数
  2. 调用业务对象 (Service)
  3. 流程跳转 (页面跳转)
  ```

  ![image-20220409193402641](https://s2.loli.net/2022/04/09/O2t8vFwMZC7jQGL.png)

> 控制器存在的问题

* 接收请求参数

  ```markdown
  1. 代码冗余
  2. 只能接收字符串类型的数据,需要手工进行类型转换
  3. 无法自动封装对象
  ```

  ![image-20220409193835613](https://s2.loli.net/2022/04/09/YbQK5c273gFXZNe.png)

* 调用业务对象

  ```java
  UserService userService = new UserServiceImpl();
  userService.login();
  ```

  ```markdown
  通过new创建业务对象,会存在耦合
  ```

* 流程跳转 (页面跳转)

  ```markdown
  1. 跳转路径存在耦合
  2. 与视图层技术存在耦合
  ```

![image-20220409194634227](https://s2.loli.net/2022/04/09/GVWJ9qPoTygDIOr.png)

![image-20220409194712329](https://s2.loli.net/2022/04/09/JlUNy9E7gewGvtq.png)

## Spring MVC的三种开发模式

1. 传统的视图开发

   ```markdown
   1. 通过作用域(request session)进行数据的传递
   2. 通过视图层技术进行数据的展示 (JSP FreeMarker Thymeleaf)
   ```

   

2. 前后端分离的开发

   ```markdown
   1. 多种新的请求发送方式
   2. Restful的访问
   3. 通过HttpMessageConverter进行数据的响应
   ```

   

3. Spring WebFlux开发

   ```markdown
   1. 替换传统的Java Web开发的一种新的Web开发方式
   2. 通过NettServer, 进行Web通信
   ```

## 总结
```markdown
Spring MVC开发主要围绕
    1. 接收Client请求参数
    2. 调用业务对象
    3. 流程跳转
```
