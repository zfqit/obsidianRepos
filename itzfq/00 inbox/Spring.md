
## 引言

### 简介

> Spring 是什么?

	Spring 是一种轻量级的框架,解决代码移植性差和运行环境苛刻还有为 JavaEE 提供了一系列的解决方案, 其中整合了大量设计模式

> 在 spring 之前使用的是什么技术,他的缺点是什么?

	在 Spring 之前使用的技术是 EJB, 而 EJB 有代码移植性差和运行环境苛刻的问题

> Spring 优点是什么?

1. 提供一系列解决方案
2. 代码移植性好
3. 运行环境无要求

> Spring 使用的设计模式?

	工厂设计模式\代理设计模式\装饰设计模式\单例设计模式等等不同的设计模式
### 工厂设计模式
#### 对象创建

> Spring 不使用原始 new 创建对象,它的缺点是什么?

	代码耦合性太高了,不利于代码的维护

> 工厂设计模式的好处?

1. 解耦合
2. 提高代码的可维护性

> Spring 使用的是什么类型的工厂?

	使用的是通用工厂设计模式

> 工厂设计模式的演化

1. 简单工厂
2. 反射工厂
3. 通用工厂

> 简单工厂和反射工厂之间的区别是什么?

	简单工厂创建对象是通过 new 来创建对象而反射工厂是通过反射技术来创建对象,从而使反射工厂有更好的灵活性和扩展性

> 反射工厂和通用工厂的区别是什么?

	反射工厂和通用工厂都是用反射来创建对象, 区别是反射工厂只关注对象的创建,而不关注对象的生命周期和依赖注入等高级功能

## 入门

### 快速入门

```xml
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.13.1</version>
	</dependency>
	
	<!--添加 Spring 依赖-->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>5.1.6.RELEASE</version>
	</dependency>

```

```java
public class Person {  
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns="http://www.springframework.org/schema/beans"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">  
  
    <bean id="p" class="com.zhou.pojo.Person"/>  
  
    <bean id="p1" class="com.zhou.pojo.Person"/>  
  
</beans>
```

```java
public class SpringTest {  
  
    @Test  
    public void test1() {  
        ApplicationContext ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");  
        Person p = (Person) ctx.getBean("p");  
        System.out.println("p = " + p);  
    }  
  
  
}
```

### ApplicationContext 核心接口

简介

	ApplicationContext 是一个工厂接口, 作用就是屏蔽不同工厂的实现

典型实现类

1. 非 Web 环境
	1. ClassPathXmlApplicationContext
2. Web 环境
	1. XmlWebApplicationContext

getBean()方法的解释

```xml
<bean id="p" class="com.zhou.pojo.Person"/>  
<bean id="p1" name="p3,p4" class="com.zhou.pojo.Person"/>
```

作用

	通过 id 和别名(name)的方式从工厂中获取对象


接口方法

```java
	ApplicationContext ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");

	// 获取所有的 Bean 的 id
	String[] beanDefinitionNames = ctx.getBeanDefinitionNames();
	for (String beanDefinitionName : beanDefinitionNames) {
		System.out.println("beanDefinitionName = " + beanDefinitionName);
	}

	// 获取指定类所有的 Bean 的 id
	String[] beanNamesForType = ctx.getBeanNamesForType(Person.class);
	for (String s : beanNamesForType) {
		System.out.println("s = " + s);
	}

	// 判断是否存在指定 id 的 bean
	if (ctx.containsBeanDefinition("p")) {
		System.out.println("true");
	} else {
		System.out.println("false");
	}

	// 判断是否存在指定 id 和别名(name)的 bean
	if (ctx.containsBean("p")) {
		System.out.println("true");
	} else {
		System.out.println("false");
	}

```

## 注入

> 注入的作用是什么?

	通过 Spring 工厂和配置文件,为所创建的对象的成员变量赋值

> 注入要解决的问题是什么?

	通过 new 和 set 方法对对象的成员属性赋值是有耦合的,所以需要注入的方式来解耦合

如何注入

1. 为类的成员变量添加 get 和 set 方法
2. 配置 Spring 的配置文件

配置文件

```xml
<bean id="p5" class="com.zhou.pojo.Person">
	<property name="name">
		<value>zhou</value>
	</property>
	<property name="age">
		<value>18</value>
	</property>
</bean>
```

>注入的好处是什么?

	解耦合

### Set 注入

> set 注入是什么意思?

	spring 调用 set 方法,并通过配置文件,为成员变量赋值

>set 注入的前提条件

	为成员变量提供 set 和 get 方法

#### Set注入成员属性类型

JDK 类型注入

| 类型         | 标签(下面的标签都在 property 或者 constructor-arg 标签中) |
| ---------- | ------------------------------------------- |
| 八种基本类型     | value 标签                                    |
| 数组         | list 标签                                     |
| List       | list 标签                                     |
| Set        | set 标签                                      |
| Map        | map 标签嵌套 entry 标签在嵌套 key 和 value 标签         |
| Properites | props 标签嵌套 prop 标签                          |

^80ea76


自定义类型注入

通过 ref 标签进行注入

第一种实现方式

```xml
<bean id="userDAO" class="com.zhou.dao.UserDAOImpl"></bean>

<bean id="userService" class="com.zhou.service.UserServiceImpl">
    <property name="userDAO">
        <ref bean="userDAO"/>
    </property>
</bean>

```

第二种实现方式

```xml
<bean id="userDAO" class="com.zhou.dao.UserDAOImpl"></bean>

<bean id="userService" class="com.zhou.service.UserServiceImpl">
	<property name="userDAO" ref="userDAO"/>
</bean>

```

### 构造注入

> 构造注入是什么意思?

	spring 调用构造方法,并通过配置文件,为成员变量赋值

> 构造注入的前提条件

	为成员变量提供有对应参数的构造方法

开发步骤

1. 为成员变量提供对应有参的构造方法
2. 在 spring 配置文件中的 bean 标签使用 constructor-arg 标签为成员变量赋值 
	![[Spring#^80ea76]]

### 控制反转

> 什么是反转控制?

	把成员变量赋值的控制权,从代码中反转到 spring 工厂和配置文件中来完成

> 反转控制的好处?

	解耦合
	
> 实现原理是什么?

	工厂设计模式, 也可以叫做通用工厂设计模式

依赖注入

> 什么是依赖注入?

	一个类需要另一个类(就是依赖),需要把另一个类作为本类的成员变量, 通过 spring 工厂和配置文件为成员变量进行赋值(这就是注入)

> 依赖注入好处是什么?

	解耦合

## Spring 对象创建

### 简单对象

> 什么是简单对象?

	通过 new 创建的对象就是简单对象

操作步骤

配置文件配置

```xml
<bean id="userDAO" class="com.zhou.dao.UserDAOImpl"></bean>
```

获取对象

```java
public class SpringTest {  
  
    @Test  
    public void test2() {  
        ApplicationContext ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");  
        // 获取对象
        UserDAO userDao = (UserDAO) ctx.getBean("userDAO");  
        System.out.println("userDAO = " + userDAO);  
    }  
}
```
### 复杂对象

> 什么是复杂对象?

	不能通过 new 创建的对象就是复杂对象
	如: SqlSessionFacorty/Connection

FactoryBean 接口创建复杂对象


## AOP


## 整合框架


## 注解开发
