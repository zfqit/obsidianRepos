# Java异常

## 什么是异常？

在程序运行时，发生不被期望的事件，它阻止了程序预期效果叫做异常

## 异常类型

> 在Java中Throwable是所有异常类的超类，在Throwable中分别有Error 和 Exception，Error是错误类，一般是由Java虚拟机抛出，Exception是异常类，异常类分为运行时异常和非运行时异常

### Error

> 是程序中无法处理的错误，表示运行应用程序中出现了严重的错误。此类错误一般表示代码运行时JVM出现问题。通常有Virtual MachineError（虚拟机运行错误）、NoClassDefFoundError（类定义错误）等。比如说当jvm耗完可用内存时，将出现OutOfMemoryError。此类错误发生时，JVM将终止线程。非代码性错误。因此，当此类错误发生时，应用不应该去处理此类错误。

### Exception

- 非运行时异常(受检异常)：Exception中除RuntimeException极其子类之外的异常。编译器会检查此类异常，如果程序中出现此类异常，比如说IOException，必须对该异常进行处理，要么使用try-catch捕获，要么使用throws语句抛出，否则编译不通过。
- 运行时异常(不受检异常)：RuntimeException类极其子类表示JVM在运行期间可能出现的错误。编译器不会检查此类异常，并且不要求处理异常，比如用空值对象的引用（NullPointerException）、数组下标越界（ArrayIndexOutBoundException）。此类异常属于不可查异常，一般是由程序逻辑错误引起的，在程序中可以选择捕获处理，也可以不处理。

## 异常的处理

> 抛出异常 throw，throws 
>
> 捕获异常 try，catch，finally

抛出异常的使用格式

```java
throw new 异常类名(参数)；
```

声明抛出异常throws 运用于方法声明之上，用于表示当前方法不处理异常，而是提醒该方法的调用者来处理异常

```java
修饰符 返回值类型 方法名（参数） throws 异常类名1，异常类名2 ...
```

捕获异常

try 捕获可能出现问题的代码

catch 针对问题的处理，异常捕获建议从最小的异常到最大的异常捕获

finally是捕获异常或没有捕获异常都会执行的代码，一般用于释放资源

基础格式:

```java
try {	
可能出现问题的代码;
} catch(异常类 变量) ...... catch(异常类 变量){
针对问题的处理;
} finally {	
释放资源;
}
```

## 自定义异常

>  自定义异常类

``` java
package com.exception.demo;

/**
 * 自定义异常类
 *
 * @author zhou
 * @date 2021/01/31 20:19
 **/
public class DomeException extends RuntimeException {

    public String message;
    public int id;

    public DomeException(String message, int id) {
        this.message = message;

    }

    @Override
    public String toString() {
        return "DomeException{" +
                "message='" + message + '\'' +
                ", id=" + id +
                '}';
    }
}

```

>  测试类

```java
package com.exception.demo;

/**
 * 测试类
 *
 * @author zhou
 * @date 2021/01/31 20:16
 **/
public class Test {
    public static void main(String[] args) {
        // try 把可能会出现异常的代码放进去
        try {
            new Test().demoTest(0);
        } catch (DomeException e) {
            // catch 一般捕获异常是从小到大来捕获异常(如先从子类在到父类在到超类)
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // finally一般用于关闭资源使用
            System.out.println("无论捕获的到异常,都会执行的代码");
        }
    }

    public void demoTest(int id) {
        if (id <= 0) {
            throw new DomeException("id不能为0", 0);
        }
    }

}

```

