# UML

## UML 基本概念

> 什么是 UML？

UML 是 OMG 在1997年1月提出了创建由对象管理组和 UML1.0 规范草案；
UML 是一种为面向对象开发系统的产品进行说明、可视化、和编制文档的标准语言；
UML 作为一种模型语言，它使开发人员专注于建立产品的模型和结构，而不是选用什么程序语言和算法实现；
UML 是不同于其他常见的编程语言，如 C + +，Java中，COBOL 等，它是一种绘画语言，用来做软件蓝图；
UML 不是一种编程语言，但工具可用于生成各种语言的代码中使用 UML 图；
UML 可以用来建模非软件系统的处理流程，以及像在一个制造单元等.

> UML 图的作用

画 UML 图与写文章差不多，都是把自己的思想描述给别人看，关键在于思路和条理，UML图分类：
- 用例图（use case）
- 静态结构图：**类图**、对象图、包图、组件图、部署图
- 动态行为图：交互图（时序图与协作图）、状态图、活动图

## UML 类图

- 用于描述系统中的类（对象）本身的组成和类（对象）之间的各种静态关系
- 类之间的关系：依赖、泛化（继承）、实现、关联、聚合与组合

类图简单举例
```java
public class Person {
    private Integer id;
    private String name;
    public void setName(String name){
        this.name = name;
    }
    public String getName(){
        return name;
    }
}
```

类图:
![](http://img.zfqit.top/img/202206101502531.png)


### 类图依赖 Dependence

**只要是在类中用到了对方，那么他们之间就存在依赖关系**。如果没有对方，连编译都通过不了

```java
public class PersonServiceBean {
    // 类的成员属性
    private PersonDao personDao;

    // 方法接收的参数类型
    public void save(Person person) {
    }

    // 方法的返回类型
    public IDCard getIDCard(Integer personid) {
        return null;
    }

    // 方法中使用到
    public void modify() {
        Department department = new Department();
    }

}
```
类图
![](http://img.zfqit.top/img/202206101517035.png)

* 总结
	- 类中用到了对方
	- 类的成员属性
	- 方法的返回类型
	- 方法接收的参数类型
	- 方法中使用到

### 类图泛化 Generalization

**泛化关系实际上就是继承关系**，它是依赖关系的特例

```java
public abstract class DaoSupport {  
    public void save(Object entity) {  
    }  
  
    public void delete(Object id) {  
    }  
}
public class PersonServiceBean extends DaoSupport {  
}
```
继承关系UML图
![](http://img.zfqit.top/img/202206101519079.png)

* 总结
	- 泛化关系实际上就是继承关系
	- 如果 A 类继承了 B 类，我们就说 A 和 B 存在泛化关系

### 类图关联 Association

关联关系实际上就是类与类之间的联系，它是依赖关系的特例
-   关联具有导航性：即双向关系或单向关系
-   关系具有多重性：如“1”（表示有且仅有一个），“0...”（表示0个或者多个），“0，1”（表示0个或者一个），“n...m”（表示n到m个都可以），“m...`*`”（表示至少m个）


一对一关系
```java
public class Person {
	private IDCard card;
}
class IDCard {}
```
UML类图
![](http://img.zfqit.top/img/202206101546077.png)

双向一对一关系
```java
public class Person2 {
	private IDCard2 card;
}
class IDCard2 {
	private Person2 person;
}
```
UML类图
![](http://img.zfqit.top/img/202206101545351.png)

### 类图聚合 Aggregation

**聚合关系表示的是整体和部分的关系，整体与部分可以分开**。聚合关系是关联关系的特例，所以它具有关联的导航性与多重性
如：一台电脑由键盘（keyboard）、显示器（monitor），鼠标等组成；组成电脑的各个配件是可以从电脑上分离出来的，使用带空心菱形的实线来表示：

```java
public class Mouse {}
public class Monitor {}
public class Computer {
    private Mouse mouse;
    private Monitor monitor;

    public void setMouse(Mouse mouse) {
        this.mouse = mouse;
    }

    public void setMonitor(Monitor monitor) {
        this.monitor = monitor;
    }
}
```
UML 类图
![](http://img.zfqit.top/img/202206101540759.png)

### 类图组合 Composition

**组合关系也是整体与部分的关系，但是整体与部分不可以分开**
如果我们认为 Mouse、Monitor 和 Computer 是不可分离的，则升级为组合关系

```java  
class Mouse {  
}  
  
class Monitor {  
}  
  
public class Computer {  
    private Mouse mouse = new Mouse();  
    private Monitor monitor = new Monitor();  
}
```
UML类图
![](http://img.zfqit.top/img/202206101552215.png)
