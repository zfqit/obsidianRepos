# Java多线程(Thread)

## 线程的基本介绍

> 线程就是独立的执行路径 (如一支笔画一条线, 两支笔同时就可以画两条线了)
>
> 在Java 程序中,即使你没有创建线程,后台也会有多个线程 如 Gc线程 主线程
>
> 在一个进程中,如果开辟了多个线程,线程的运行的顺序是由系统调度器来调节,不能人为进行干预
>
> 对同一份资源操作时,会存在资源抢夺问题,需要加入(同步操作)并发操作  (100人买票,而票只有10张,这就需要同步操作,即排队购买)
>
> 线程会带来额外的开销,如cpu的调度时间,并发控制开销
>
> 每个线程在自己的工作内存交互,内存控制不当会造成数据不一致
>

## 线程的创建的三种方式

### `Thread类`

> 线程的创建
>
> 创建自定义线程类继承 Thread 类, 重写run()方法,并编写线程体, 创建线程对象并调用 start()方法,开启线程
>
> `总结: 线程开启不一定立即执行,由CPU调度执行,不建议使用,因为Java中单继承的局限性`

```java
package com.thread.demo;

/**
 * 线程的第一种创建方式Thread
 * 继承 Thread类,重写run方法,创建线程类并开启线程start
 *
 * @author zhou
 * @date 2021/01/31 21:52
 **/
public class ThreadDemo extends Thread {

    @Override
    public void run() {
        // 重写run方法并编写线程体
        // 新线程
        for (int i = 0; i < 30; i++) {
            System.out.println("我在看书~~~" + i);
        }
    }

    public static void main(String[] args) {
        // 创建线程对象
        ThreadDemo threadDemo = new ThreadDemo();
        // 开启线程
        threadDemo.start();
        // 主线程
        for (int i = 0; i < 100; i++) {
            System.out.println("我在学习多线程~~" + i);
        }
    }
}
```

Thread实现图片下载器案例

```xml
    <dependencies>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.6</version>
        </dependency>
    </dependencies>
```

```java
package com.thread.demo;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;

/**
 * 多线程下载图片
 *
 * @author zhou
 * @date 2021/01/31 22:14
 * 2. 继承Thread类
 **/
public class ThreadDownloaderDome extends Thread {

    public String url;
    public String name;

    public ThreadDownloaderDome(String url, String name) {
        this.url = url;
        this.name = name;
    }

    @Override
    public void run() {
        // 3.重写run方法并编写线程体
        Downloader downloader = new Downloader();
        downloader.Downloader(url, name);
    }

    public static void main(String[] args) {
        // 4. 创建线程并下载
        ThreadDownloaderDome t1 = new ThreadDownloaderDome("https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3363295869,2467511306&fm=26&gp=0.jpg","1.jpg");
        ThreadDownloaderDome t2 = new ThreadDownloaderDome("https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fa4.att.hudong.com%2F27%2F67%2F01300000921826141299672233506.jpg&refer=http%3A%2F%2Fa4.att.hudong.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1614695082&t=547f7083d22a1cd5c78d47061443256c","2.jpg");
        ThreadDownloaderDome t3 = new ThreadDownloaderDome("https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fa0.att.hudong.com%2F52%2F62%2F31300542679117141195629117826.jpg&refer=http%3A%2F%2Fa0.att.hudong.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1614695082&t=5e879d18b7f87ed3a4d8ce42e254c85a","3.jpg");
        t1.start();
        t2.start();
        t3.start();
    }
}

class Downloader {
    /**
     * 1. 文件下载器
     *
     * @param url
     * @param name
     * @return void
     * @author zhou
     * @date 2021/01/31 22:17
     */
    public void Downloader(String url, String name) {
        try {
            FileUtils.copyURLToFile(new URL(url), new File(name));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

### `Runnable接口`

> 线程的创建
>
> 创建自定义线程类实现 Runnable 接口, 重写run()方法,并编写线程体, 创建线程对象并创建线程代理对象(Thread)调用 start()方法,开启线程 如: new Thread(自定义线程对象).start()
>
> `总结: 推荐使用Runnable接口实现多线程,因为避免单继承的局限性,灵活方便,方便同一个对象被多个线程使用`

```java
package com.thread.demo;

/**
 * 线程的第二种创建方式Runnable
 *
 * 创建自定义线程类实现 Runnable 接口
 * 重写run()方法,并编写线程体
 * 创建线程对象并创建线程代理对象(Thread)调用 start()方法
 * 开启线程 如: new Thread(自定义线程对象).start()
 *
 * @author zhou
 * @date 2021/01/31 22:43
 **/
public class RunnableDome implements Runnable {

    @Override
    public void run() {
        // 重写run方法并编写线程体
        // 新线程
        for (int i = 0; i < 30; i++) {
            System.out.println("我在看书~~~" + i);
        }
    }

    public static void main(String[] args) {
        // 创建一个自定义线程对象
        RunnableDome runnableDome = new RunnableDome();
        // 创建一个线程代理对象,并传入自定义线程对象,并开启线程
        // Thread thread = new Thread(runnableDome);
        // thread.start();
        // 简化方式
        new Thread(runnableDome).start();

        // 主线程
        for (int i = 0; i < 100; i++) {
            System.out.println("我在学习多线程~~" + i);
        }
    }
}

```

Runnable实现图片下载器案例

```java
package com.thread.demo;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;

/**
 * 多线程下载图片
 *
 * @author zhou
 * @date 2021/01/31 22:14
 * 2. 实现Runnable类
 **/
class RunnableDownloadDome implements Runnable {

    public String url;
    public String name;

    public RunnableDownloadDome(String url, String name) {
        this.url = url;
        this.name = name;
    }

    @Override
    public void run() {
        // 3.重写run方法并编写线程体
        Downloader downloader = new Downloader();
        downloader.Downloader(url, name);
    }

    public static void main(String[] args) {
        // 4. 创建线程并下载
        ThreadDownloaderDome t1 = new ThreadDownloaderDome("https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3363295869,2467511306&fm=26&gp=0.jpg","1.jpg");
        ThreadDownloaderDome t2 = new ThreadDownloaderDome("https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fa4.att.hudong.com%2F27%2F67%2F01300000921826141299672233506.jpg&refer=http%3A%2F%2Fa4.att.hudong.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1614695082&t=547f7083d22a1cd5c78d47061443256c","2.jpg");
        ThreadDownloaderDome t3 = new ThreadDownloaderDome("https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fa0.att.hudong.com%2F52%2F62%2F31300542679117141195629117826.jpg&refer=http%3A%2F%2Fa0.att.hudong.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1614695082&t=5e879d18b7f87ed3a4d8ce42e254c85a","3.jpg");
        new Thread(t1).start();
        new Thread(t2).start();
        new Thread(t3).start();
    }
}

class Download {
    /**
     * 1. 文件下载器
     *
     * @param url
     * @param name
     * @return void
     * @author zhou
     * @date 2021/01/31 22:17
     */
    public void Downloader(String url, String name) {
        try {
            FileUtils.copyURLToFile(new URL(url), new File(name));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```

模拟抢票例子

```java
package com.thread.demo;

/**
 * 抢票模拟
 * 这就是避免单继承的局限性,灵活方便,方便同一个对象被多个线程使用
 * 总结: 这样抢票会出现一个数据不一致的问题,这就是线程不安全
 *
 * @author zhou
 * @date 2021/01/31 23:18
 **/
public class GrabVotesDome implements Runnable {

    private int votes = 10;

    @Override
    public void run() {
        while (true) {
            if (votes <= 0) {
                break;
            }
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "--->抢到第" + votes-- + "票");
        }
    }

    public static void main(String[] args) {
        // 创建线程并抢票
        GrabVotesDome grabVotesDome = new GrabVotesDome();
        new Thread(grabVotesDome, "小明").start();
        new Thread(grabVotesDome, "小李").start();
        new Thread(grabVotesDome, "小黑").start();
    }
}
```

龟兔赛跑例子

```java
package com.exception.demo;

/**
 * 龟兔赛跑
 *
 * @author zhou
 * @date 2021/01/31 23:24
 **/
public class Race implements Runnable {
    // 胜利者
    private static String winner;

    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            // 让兔子中途碎觉
            if (Thread.currentThread().getName().equals("兔子") && i % 5 == 0) {
                try {
                    Thread.sleep(2);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println(Thread.currentThread().getName() + "--->跑了" + i + "步");
            // 判断胜利者,如果有胜利者就结束线程
            if (gameOver(i)) {
                break;
            }
        }
    }

    /**
     * 判断谁赢了
     * @param steps 步数
     * @return
     */
    private boolean gameOver(int steps) {
        // 判断是否有胜利者
        if (winner != null) {
            return true;
            //判断步数等于100的胜利者,并输出胜利者
        } else if (steps == 100) {
            winner = Thread.currentThread().getName();
            System.out.println("winner is " + winner);
        }
        return false;
    }

    public static void main(String[] args) {
        Race race = new Race();
        new Thread(race, "乌龟").start();
        new Thread(race, "兔子").start();
    }
}
```

### Callable接口

> 线程的创建
>
> 1. 创建自定义线程类实现Callable接口,需要返回值
> 2. 并重写call()方法,需要抛出异常,
> 3. 创建线程对象
> 4. 创建FutureTask类来获取返回值
> 5. 创建Thread类并使用start()方法来启动线程

Callable创建线程

```java
package com.thread.demo;

import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

/**
 * Callable实现多线程
 *
 * @author zhou
 * @date 2021/02/02 11:11
 **/
public class CallableDome implements Callable<Boolean> {

    @Override
    public Boolean call() throws Exception {
        // 新线程
        for (int i = 0; i < 30; i++) {
            System.out.println("我在看书~~~" + i);
        }
        return true;
    }

    public static void main(String[] args) {

        // 创建自定义线程类
        CallableDome callableDome = new CallableDome();
        // 因为有返回值所以创建FutureTask类来获取返回值
        FutureTask<Boolean> booleanFutureTask = new FutureTask<>(callableDome);
        // 通过Thread类,来启动线程
        new Thread(booleanFutureTask).start();

        // 主线程
        for (int i = 0; i < 100; i++) {
            System.out.println("我在学习多线程~~" + i);
        }
    }
}

```

## 静态代理

> 多线程是用一个静态代理对象来实现的,如结婚, 婚庆公司,你结婚,你委托婚庆公司对你结婚前的准备工作进行处理,对结婚后的收尾款进行处理.而多线程也是如此,Runnable的run()方法是让程序员专注业务逻辑,Thread(自定义线程类)等方法.会代理你的业务逻辑实现多线程

```java
package com.thread.demo;

/**
 * 静态代理的演示
 *
 * @author zhou
 * @date 2021/02/04 11:46
 **/
public class ProxyDome {
    public static void main(String[] args) {
        // 创建一个结婚的代理对象,并把结婚对象传入进去,让结婚代理对象帮你实现
        Wedding wedding = new Wedding(new You());
        wedding.marry();
    }
}

// 你结婚
class You implements Marry {
    @Override
    public void marry() {
        System.out.println("今天我结婚啦");
    }
}

// 结婚
interface Marry {
    void marry();
}

// 婚庆公司
class Wedding implements Marry {

    // 代理结婚对象
    private Marry target;

    public Wedding(Marry target) {
        this.target = target;
    }

        public void after(){
            System.out.println("结婚之前准备工作");
        }

        public void before(){
        System.out.println("收尾款");
    }

    @Override
    public void marry() {
        after();
        target.marry();
        before();
    }
}
```

## lambda表达式

### 为什么使用lambda表达式?

> 简化代码并避免定义太多匿名内部类,有繁琐而重复的代码
>
> 让代码看起来很简洁
>
> 去掉了一堆没有意义的代码,专注与核心逻辑

### Functional Interface (函数式接口)

> 因为使用lambda表达式,用到了jdk 8 的函数式接口,所以使用lambda表达式需要对函数式接口做一个掌握

函数式接口的定义

1. 任何接口,如果只包含唯一一个抽象方法,那它就是一个函数式接口

2. 对于函数式接口,就可以通过lambda表达式来创建该接口的对象

### lambda表达式的使用格式

```java
函数式接口类型 引用 = () -> 业务逻辑;
函数式接口类型 引用 = (参数1,...N) -> {
	业务逻辑;
}
函数式接口类型 引用 = (参数1,...N) -> 业务逻辑;
函数式接口类型 引用 = (参数1,...N) -> {
	业务逻辑
};
```

### 总结

> 1. 前提得是函数式接口
> 2. 需要写多行业务逻辑的代码,需要加上大括号
> 3. 传参数的使用方式,要么都写类型,要么都不写

## 线程状态

![image-20210204133208037](images/image-20210204133208037.png)

创建状态

> Thread t = new Thread(); 就是创建状态

就绪状态

> t.start(); 就是就绪状态,但是不意味立即进入运行状态

运行状态

>  由CPU分配哪个线程到运行状态

阻塞状态

> 通过Thread.sleep()方法或wait()方法或同步锁定时,线程进入阻塞状态,就是代码不继续往下执行,阻塞状态结束后,会变成就绪状态,重新等待CPU的调度执行

死亡状态

> destroy()方法可以让线程销毁,或让线程自然执行完毕销毁,一旦进入死亡状态,就不能再次启动

### 停止线程

> 不建议使用 JDK 提供的stop destroy方法来让线程停止 [已废弃]
>
> 推荐让线程自己停下来 (利用次数)
>
> 建议使用标志位来进行终止线程,当flag = false, 则线程终止运行

```java
package com.thread.status.dome;

/**
 * 线程终止
 *
 * @author zhou
 * @date 2021/02/04 21:15
 * 1. 建议使用标志位来进行终止线程,如flag = false来终止线程
 * 2. 建议让线程自然终止,如 设定次数来为终止条件
 * 3. 不建议使用JDK提供的stop destroy方法来终止线程
 **/
public class ThreadStatusDome implements Runnable {

    /**
     * 设置标志位
     */
    private boolean flag = true;

    @Override
    public void run() {
        int i = 0;
        while (flag) {
            System.out.println(Thread.currentThread().getName() + "--->" + i++);
        }
    }

    // 创建终止方法,用标志位等于false来终止
    public void stop() {
        this.flag = false;
    }

    public static void main(String[] args) {
        // 创建线程并开启线程
        ThreadStatusDome threadStatusDome = new ThreadStatusDome();
        new Thread(threadStatusDome, "好好").start();
        // 设置条件来终止线程
        for (int i = 0; i < 1000; i++) {
            System.out.println("main" + i);
            if (i >= 900) {
                // 使用终止标志位来终止
                threadStatusDome.stop();
                System.out.println("该结束了");
            }
        }
    }
}

```

### 线程休眠

- sleep(时间) 指当前线程阻塞的毫秒数
- sleep存在异常InterruptedException
- sleep 时间达到后就会变成就绪状态
- sleep可以模拟网络延时和倒计时
- 每一个对象都有一个锁,sleep不会释放锁

网络延时例子:

```java
package com.thread.demo;

/**
 * 抢票抢票的网络延时
 * 这就是避免单继承的局限性,灵活方便,方便同一个对象被多个线程使用
 * 总结: 这样抢票会出现一个数据不一致的问题,这就是线程不安全 
 *
 * @author zhou
 * @date 2021/01/31 23:18
 **/
public class GrabVotesDome implements Runnable {

    private int votes = 10;

    @Override
    public void run() {
        while (true) {
            if (votes <= 0) {
                break;
            }
            // 网络延时
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "--->抢到第" + votes-- + "票");
        }
    }

    public static void main(String[] args) {
        // 创建线程并抢票
        GrabVotesDome grabVotesDome = new GrabVotesDome();
        new Thread(grabVotesDome, "小明").start();
        new Thread(grabVotesDome, "小李").start();
        new Thread(grabVotesDome, "小黑").start();
    }
}

```

模拟倒计时例子:

```java
package com.thread.status.dome;

import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * 模拟倒计时
 *
 * @author zhou
 * @date 2021/02/04 21:43
 **/
public class ThreadSleepDate {

    public static void main(String[] args) {
        // 获取当前系统时间
        Date date = new Date(System.currentTimeMillis());
        while (true) {
            try {
                Thread.sleep(1000);
                System.out.println(new SimpleDateFormat("HH:mm:ss").format(date));
                // 更新当先系统时间
                date = new Date(System.currentTimeMillis());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

}

```

### 线程礼让

> 礼让线程,让当前正在执行的线程暂停,但不阻塞
>
> 线程礼让不一定成功,调用礼让方法后,只能让运行中的线程变成就绪状态,变为就绪状态后,继续看CPU的调度

线程礼让例子:

```java
package com.thread.status.dome;

/**
 * 线程礼让
 *
 * @author zhou
 * @date 2021/02/04 21:54
 **/
public class ThreadYieldDome implements Runnable {

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "线程开始了");
        // 线程礼让
        Thread.yield();
        System.out.println(Thread.currentThread().getName() + "线程结束了");
    }

    public static void main(String[] args) {
        ThreadYieldDome threadYieldDome = new ThreadYieldDome();
        new Thread(threadYieldDome, "a").start();
        new Thread(threadYieldDome, "b").start();
    }
}
```

礼让失败:

![image-20210204220052285](images/image-20210204220052285.png)

礼让成功

![image-20210204220130291](images/image-20210204220130291.png)

### 线程强制执行

> join合并线程,使用join方法的线程会等该线程执行完毕后,在执行其他线程,其他线程阻塞(可以想象插队)	

```java
package com.thread.status.dome;

/**
 * Thread线程的插队,即join 方法
 *
 * @author zhou
 * @date 2021/02/07 10:39
 **/
public class ThreadJoinDome implements Runnable{

    @Override
    public void run() {
        // 使用join插队
        for (int i = 0; i < 1000; i++) {
            System.out.println("join -->" + i);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        // 创建线程并启动线程
        ThreadJoinDome threadJoinDome = new ThreadJoinDome();
        Thread thread = new Thread(threadJoinDome);
        thread.start();

        for (int i = 0; i < 100; i++) {
            // 使用Join方法插队
            if (i == 50) {
                thread.join();
            }
            System.out.println("main -->" + i);
        }
    }
}
```

### 观测线程状态

> 线程状态。线程可以处于下列状态之一：
>
> - [`NEW`]
>   至今尚未启动的线程处于这种状态。
> - [`RUNNABLE`]
>   正在 Java 虚拟机中执行的线程处于这种状态。
> - [`BLOCKED`]
>   受阻塞并等待某个监视器锁的线程处于这种状态。
> - [`WAITING`]
>   无限期地等待另一个线程来执行某一特定操作的线程处于这种状态。
> - [`TIMED_WAITING`]
>   等待另一个线程来执行取决于指定等待时间的操作的线程处于这种状态。
> - [`TERMINATED`]
>   已退出的线程处于这种状态。

```java
package com.thread.status.dome;

/**
 * 观测线程状态
 *
 * @author zhou
 * @date 2021/04/07 09:09
 **/
public class ThreadStatus {

    public static void main(String[] args) {
        // 创建线程
        Thread thread = new Thread(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("线程结束了");
        });
        // 打印初始状态,也就是新生状态 new
        Thread.State state = thread.getState();
        System.out.println(state);

        // 运行状态 Runnable
        thread.start();
        state = thread.getState();
        System.out.println(state);

        // 观测阻塞状态和终止状态
        while (state != Thread.State.TERMINATED) {
            try {
                Thread.sleep(100);
                // 更新线程状态
                state = thread.getState();
                System.out.println(state);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

    }
}

```

### 设置线程优先级

> 优先级有效范围是 1 - 10, 1是最小优先级,数字越大优先级越高,但是优先级越高也不一定优先执行,也看CPU调度
>
> 注意: 要在设置完线程优先级后在启动

```java
package com.thread.status.dome;

/**
 * 设置线程的优先级,并观测
 *
 * 注意: 先要设置线程优先级,再启动线程
 *
 * @author zhou
 * @date 2021/04/07 09:17
 **/
public class ThreadPriority {

    public static void main(String[] args) {
        // 打印主线程优先级
        System.out.println(Thread.currentThread().getName() + "线程状态:" + Thread.currentThread().getPriority());
        // 创建线程
        TestThread testThread = new TestThread();
        // 设置优先级并启动线程
        Thread t1 = new Thread(testThread);
        Thread t2 = new Thread(testThread);
        Thread t3 = new Thread(testThread);
        Thread t4 = new Thread(testThread);
        Thread t5 = new Thread(testThread);
        Thread t6 = new Thread(testThread);
        Thread t7 = new Thread(testThread);
        // 默认优先级,启动
        t1.start();
        // 优先级设置2
        t2.setPriority(2);
        t2.start();
        // 设置最大优先级 10
        t3.setPriority(Thread.MAX_PRIORITY);
        t3.start();

        // 设置优先级为 7 和 8
        t4.setPriority(7);
        t4.start();

        t5.setPriority(8);
        t5.start();

        // 设置优先级超过最大值和小于最小值
        t6.setPriority(-1);
        t6.start();

        t7.setPriority(11);
        t7.start();
    }

}

class TestThread implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "线程状态:" + Thread.currentThread().getPriority());
    }
}
```

### 守护线程

* 线程分为用户线程和守护线程
* 虚拟机必须确保用户线程执行完毕
* 虚拟机不用等待守护线程执行完毕
* 如 : 后台记录操作日志 监控内存 垃圾回收等等

> 虚拟机会等待用户线程执行完毕,但是不会等待守护线程执行完毕
> 那么由此得出结论,用户线程执行完毕后,守护线程也会被虚拟机给关闭

```java
package com.thread.status.dome;

/**
 * 守护线程
 * 上帝守护你的Dome
 *
 * @author zhou
 * @date 2021/04/07 09:56
 **/
public class ThreadDaemenTest {

    public static void main(String[] args) {
        God god = new God();
        You you = new You();

        Thread thread = new Thread(god);
        // 设置为守护线程,该值是布尔值,默认为false(用户线程),为true就是守护线程
        thread.setDaemon(true);
        thread.start();
        // 虚拟机会等待用户线程执行完毕,但是不会等待守护线程执行完毕
        // 那么由此得出结论,用户线程执行完毕后,守护线程也会被虚拟机给关闭
        new Thread(you).start();
    }
}

/**
 * 守护线程
 */
class God implements Runnable {
    @Override
    public void run() {
        while (true) {
            System.out.println("守护你的一生");
        }
    }
}

/**
 * 用户线程
 */
class You implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 30000; i++) {
            System.out.println("每天开心快乐的生活");
        }
    }
}
```

## 线程同步

> 由于同一进程的多个线程共享同一块存储空间,在带来方便的同时,也带来访问冲突问题,为了保证数据的在方法中访问时的正确性,在访问时加入锁机制 `synchronized`,当一个线程获取它的排他锁,独占资源,其他线程必须等待,使用后释放锁即可,存在以下问题:
>
> * 一个线程持有锁会导致其他所有需要此锁的线程挂起
> * 在多线程竞争下,加锁和释放锁会导致比较多的上下文切换和调度延时,引起性能问题
> * 如果一个优先级高的线程等待一个优先级低的线程释放锁,会引起性能问题

> 同步方法 public synchronized void method(int agrs){}
>
> synchronized 方法控制对对象的访问,每个对象对应一把锁,每个synchronized方法都必须要调用该方法的对象的锁才可以执行,否则线程就会阻塞,方法一旦执行,就独占该锁,直到该方法返回才释放锁,后面被阻塞的线程才可以获得这个锁,继续执行. synchronized默认锁的是 this

> 同步代码块 synchronized (obj) {}
>
> * obj可以是任何对象,但是推荐使用共享资源作为同步监视器
> * 同步方法无需指定任何同步监视器,因为同步方法的同步监视器是this.就是这个对象本身,或者class
> * synchronized是隐性锁

线程不安全(同步)的案例:

```java
package com.thread.demo;

/**
 * 抢票模拟
 * 总结: 这样抢票会出现一个数据不一致的问题,这就是线程不安全
 *
 * @author zhou
 * @date 2021/01/31 23:18
 **/
public class GrabVotesDome implements Runnable {

    private int votes = 10;

    @Override
    public void run() {
        while (true) {
            if (votes <= 0) {
                break;
            }
            // 网络延时
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "--->抢到第" + votes-- + "票");
        }
    }

    public static void main(String[] args) {
        // 创建线程并抢票
        GrabVotesDome grabVotesDome = new GrabVotesDome();
        new Thread(grabVotesDome, "小明").start();
        new Thread(grabVotesDome, "小李").start();
        new Thread(grabVotesDome, "小黑").start();
    }
}

```

```java
package com.thread.demo;

/**
 * 线程不安全的取钱
 * 模拟银行取钱,取钱需要账号也就是卡号
 *
 * @author zhou
 * @date 2021/04/07 10:48
 **/
public class UnsafeBank {
    public static void main(String[] args) {
        Account account = new Account(100, "旅游基金");
        Drawing you = new Drawing(account, 50, "你");
        Drawing girl = new Drawing(account, 100, "老婆");
        you.start();
        girl.start();
    }
}

/**
 * 账户
 */
class Account {
    /**
     * 账户余额
     */
    int money;
    /**
     * 账户卡号或者卡名
     */
    String name;

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

class Drawing extends Thread {
    /**
     * 账户
     */
    Account account;
    /**
     * 取了多少钱
     */
    int drawingMoney;
    /**
     * 现在手里多少钱
     */
    int nowMoney;

    public Drawing(Account account, int drawingMoney, String name) {
        // 调用父类的构造方法把线程的名称传入进去
        super(name);
        this.account = account;
        this.drawingMoney = drawingMoney;
    }

    /**
     * 模拟取钱
     */
    @Override
    public void run() {
        // 判断取得钱是否大于账户里的钱
        if (account.money - drawingMoney < 0) {
            System.out.println(this.getName() + "取得钱超过账户余额");
            return;
        }
        // 模拟延时
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 账户里的钱 = 账户钱 - 取出来的钱
        account.money = account.money - drawingMoney;
        // 手里的钱 = 手里的钱 + 取出来的钱
        nowMoney = nowMoney + drawingMoney;

        System.out.println(account.name + "余额为" + account.money);
        // this.getName() 因为继承了Thread类,所以可以通过this.getName()方法获得该线程的名字
        // 和Thread.currentThread().getName()类似
        System.out.println(this.getName() + "手里的钱=" + nowMoney);
    }
}
```

线程不安全(同步)变成同步的案例:

```java
package com.thread.sync;

/**
 * 抢票模拟
 * 总结: 这样抢票会出现一个数据不一致的问题,这就是线程不安全
 * 锁住要共享资源的对象,即可使线程同步
 * @author zhou
 * @date 2021/01/31 23:18
 **/
public class GrabVotes implements Runnable {

    private int votes = 10;

    @Override
    public synchronized void run() {
        while (true) {
            if (votes <= 0) {
                break;
            }
            // 网络延时
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "--->抢到第" + votes-- + "票");
        }
    }

    public static void main(String[] args) {
        // 创建线程并抢票
        GrabVotes grabVotesDome = new GrabVotes();
        new Thread(grabVotesDome, "小明").start();
        new Thread(grabVotesDome, "小李").start();
        new Thread(grabVotesDome, "小黑").start();
    }
}

```

```java
package com.thread.sync;

/**
 * 线程不安全的取钱
 * 模拟银行取钱,取钱需要账号也就是卡号
 * 用同步代码块来锁住共享资源从而达到同步的效果
 *
 * @author zhou
 * @date 2021/04/07 10:48
 **/
public class UnsafeBank {
    public static void main(String[] args) {
        Account account = new Account(100, "旅游基金");
        Drawing you = new Drawing(account, 50, "你");
        Drawing girl = new Drawing(account, 100, "老婆");
        you.start();
        girl.start();
    }
}

/**
 * 账户
 */
class Account {
    /**
     * 账户余额
     */
    int money;
    /**
     * 账户卡号或者卡名
     */
    String name;

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

class Drawing extends Thread {
    /**
     * 账户
     */
    Account account;
    /**
     * 取了多少钱
     */
    int drawingMoney;
    /**
     * 现在手里多少钱
     */
    int nowMoney;

    public Drawing(Account account, int drawingMoney, String name) {
        // 调用父类的构造方法把线程的名称传入进去
        super(name);
        this.account = account;
        this.drawingMoney = drawingMoney;
    }

    /**
     * 模拟取钱
     */
    @Override
    public void run() {
        // 用同步代码块来锁住共享资源从而达到同步的效果
        // 因为用同步方法来锁的话,它锁的是this,因为this本身不是要共享的资源,所以锁它没有任何作用
        synchronized (account) {
            // 判断取得钱是否大于账户里的钱
            if (account.money - drawingMoney < 0) {
                System.out.println(this.getName() + "取得钱超过账户余额");
                return;
            }
            // 模拟延时
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            // 账户里的钱 = 账户钱 - 取出来的钱
            account.money = account.money - drawingMoney;
            // 手里的钱 = 手里的钱 + 取出来的钱
            nowMoney = nowMoney + drawingMoney;

            System.out.println(account.name + "余额为" + account.money);
            // this.getName() 因为继承了Thread类,所以可以通过this.getName()方法获得该线程的名字
            // 和Thread.currentThread().getName()类似
            System.out.println(this.getName() + "手里的钱=" + nowMoney);
        }
    }
}
```

### 死锁

> 多个线程各自占有一些共享资源,并且相互等待其他线程占有的资源才能运行,而导致两个或者多个线程都在等待对方释放资源,都停止执行的情形,某一个同步块同时拥有 `两个以上的对象锁`时,就可能会发生死锁问题
>
> `产生死锁的四个必要条件`
>
> 1. 互斥条件: 一个资源每次只能被一个进程使用
> 2. 请求与保持条件: 一个进程因请求资源而阻塞时,对已获得的资源保持不放.
> 3. 不剥夺条件: 进程已获得的资源,在未使用完之前,不能强行剥夺
> 4. 循环等待条件: 若干进程之间形成一种头尾相接的循环等待资源关系
>
> 上面列出死锁的四个必要条件,想办法破其中的任意一个或多个条件就可以避免死锁的发生

```markdown
synchronized 锁方法,锁的是本类(this) (一般情况下锁资源类)

synchronized 锁静态方法,锁的是当前类所有实例

synchronized 锁class,锁的是当前类所有实例
```

死锁案例: 

```java
package com.thread.sync;

/**
 * 死锁案例
 *
 * @author zhou
 * @date 2021/04/11 14:20
 * 死锁: 多个线程互相持有对方所需要的资源,然后形成僵持
 **/
public class DeadLock {
    public static void main(String[] args) {
        new Makeup(0, "灰姑娘").start();
        new Makeup(1, "白雪公主").start();
    }
}

/**
 * 口红
 */
class Lipstick {

}

/**
 * 镜子
 */
class Mirror {

}

/**
 * 化妆
 */
class Makeup extends Thread {

    static Mirror mirror = new Mirror();
    static Lipstick lipstick = new Lipstick();

    /**
     * 选择
     */
    int choice;
    /**
     * 化妆的人
     */
    String name;

    Makeup(int choice, String name) {
        this.choice = choice;
        this.name = name;
    }

    @Override
    public void run() {
        // 化妆
        try {
            makeUp();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void makeUp() throws InterruptedException {
        if (choice == 0) {
            // 产生死锁,因为一个进程因请求资源而阻塞时,对已获得的资源保持不放,若干进程之间形成一种头尾相接的循环等待资源关系
            synchronized (mirror) {
                System.out.println(this.name + "获得了镜子");
                Thread.sleep(1000);
                synchronized (lipstick) {
                    System.out.println(this.name + "获得了口红");
                }
            }
        } else {
            synchronized (lipstick) {
                System.out.println(this.name + "获得了口红");
                Thread.sleep(2000);
                synchronized (mirror) {
                    System.out.println(this.name + "获得了镜子");
                }
            }
        }
    }


}
```

解决案例:

```java
package com.thread.sync;

/**
 * 死锁案例
 *
 * @author zhou
 * @date 2021/04/11 14:20
 * 死锁: 多个线程互相持有对方所需要的资源,然后形成僵持
 **/
public class DeadLock {
    public static void main(String[] args) {
        new Makeup(0, "灰姑娘").start();
        new Makeup(1, "白雪公主").start();
    }
}

/**
 * 口红
 */
class Lipstick {

}

/**
 * 镜子
 */
class Mirror {

}

/**
 * 化妆
 */
class Makeup extends Thread {

    static Mirror mirror = new Mirror();
    static Lipstick lipstick = new Lipstick();

    /**
     * 选择
     */
    int choice;
    /**
     * 化妆的人
     */
    String name;

    Makeup(int choice, String name) {
        this.choice = choice;
        this.name = name;
    }

    @Override
    public void run() {
        // 化妆
        try {
            makeUp();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void makeUp() throws InterruptedException {
        if (choice == 0) {
            // 产生死锁,因为一个进程因请求资源而阻塞时,对已获得的资源保持不放,若干进程之间形成一种头尾相接的循环等待资源关系
            // 避免死锁,就要防止一个进程因请求资源而阻塞时,对已获得的资源保持不放,若干进程之间形成一种头尾相接的循环等待资源关系
            synchronized (mirror) {
                System.out.println(this.name + "获得了镜子");
                Thread.sleep(1000);
            }
            synchronized (mirror) {
                System.out.println(this.name + "获得了镜子");
            }
        } else {
            synchronized (lipstick) {
                System.out.println(this.name + "获得了口红");
                Thread.sleep(2000);
            }
            synchronized (mirror) {
                System.out.println(this.name + "获得了镜子");
            }
        }
    }


}
```

### Lock锁

> * Lock锁是显性锁,(显性锁是要手动开启和关闭),synchronized是隐性锁,出了作用域自动释放
> * lock锁只有代码块锁,synchronized有方法锁和代码块锁
> * 使用Lock锁,JVM将花费较少的时间来调度线程,性能更好.并且具有更好的扩展性(提供更多子类)
> * ReentrantLock类实现了Lock,是可重入锁,是比较常用,可以手动开启锁和释放锁
> * 优先使用顺序
>   * lock > 同步代码块 > 同步方法

```java
package com.thread.sync;

import java.util.concurrent.locks.ReentrantLock;

/**
 * Lock锁的代码案例
 * 抢票模拟
 *
 * @author zhou
 * @date 2021/04/11 14:51
 **/
public class TestLock {
    public static void main(String[] args) {
        Lock2 lock2 = new Lock2();
        new Thread(lock2).start();
        new Thread(lock2).start();
        new Thread(lock2).start();
    }
}

class Lock2 implements Runnable {

    int ticketNumb = 10;

    /**
     * 创建可重入锁
     */
    private final ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true) {
            try {
                // 用lock加锁
                lock.lock();
                if (ticketNumb > 0) {
                    Thread.sleep(1000);
                    System.out.println(ticketNumb--);
                } else {
                    break;
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                // 用lock解锁
                lock.unlock();
            }
        }
    }
}
```

### 生产者消费者模型

> 利用缓冲区:
>
>  管程法 生产者 消费者 产品 缓冲容器

```java
package com.thread.sync;

/**
 * 生产者消费者模型:-->利用缓冲区: 管程法
 * <p>
 * 生产者 消费者 产品 缓冲容器
 *
 * @author zhou
 * @date 2021/04/11 15:27
 **/
public class TestPc {
    public static void main(String[] args) {
        SyncContainer syncContainer = new SyncContainer();
        new Producer(syncContainer).start();
        new Consumer(syncContainer).start();
    }
}

/**
 * 生产者
 */
class Producer extends Thread {

    private SyncContainer syncContainer;

    Producer(SyncContainer syncContainer) {
        this.syncContainer = syncContainer;
    }

    @Override
    public void run() {
        // 生产
        for (int i = 0; i < 100; i++) {
            syncContainer.push(new Product(i));
            System.out.println("生产了第" + i);
        }
    }
}

/**
 * 消费者
 */
class Consumer extends Thread {

    private SyncContainer syncContainer;

    Consumer(SyncContainer syncContainer) {
        this.syncContainer = syncContainer;
    }


    @Override
    public void run() {
        // 消费
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了第" + syncContainer.pop().id);
        }
    }
}

/**
 * 产品
 */
class Product {
    /**
     * 产品id
     */
    int id;

    public Product(int id) {
        this.id = id;
    }
}

/**
 * 缓冲容器
 */
class SyncContainer {
    /**
     * 默认容器大小,也就是同时能让多少个消费者消费的区域
     */
    Product[] producers = new Product[10];


    /**
     * 程序计数器
     */
    int count = 0;


    /**
     * 生产者把生产的产品放入容器
     */
    public synchronized void push(Product producer) {
        // 如果生产的产品等于容器大小,也就是容器满了,则通知消费者消费
        // 注意,这里不能使用else,使用else的话程序会等待
        if (count == producers.length) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        // 否则就生产产品,并唤醒消费者消费
        producers[count] = producer;
        count++;
        this.notifyAll();

    }

    /**
     * 生产者通知消费者消费
     *
     * @return 消费的对象
     */
    public synchronized Product pop() {
        //如果容器没有产品了,需要等待,并唤醒生产者生产
        // 注意,这里不能使用else,使用else的话程序会等待
        if (count == 0) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        // count为什么先--,是因为生产者生产完毕后,count多加了1,所以先--,才可以取出你想取出的数据
        count--;
        //否则就消费产品,并唤醒生产者生产
        Product producer = producers[count];
        this.notifyAll();
        return producer;
    }

}

```

> 利用信号灯法
>
> 也就是标志位方法来实现

```java
package com.thread.sync;

/**
 * 信号灯法实现生产者消费者模型
 * 利用标志位
 *
 * @author zhou
 * @date 2021/04/11 16:48
 * <p>
 * 生产者 --> 书店
 * 消费者 --> 买书的人
 * 产品 --> 书
 **/
public class TestPC2 {
    public static void main(String[] args) {
        Book book = new Book();
        new Bookstore(book).start();
        new Person(book).start();
    }
}

/**
 * 书店
 */
class Bookstore extends Thread {
    Book book;

    public Bookstore(Book book) {
        this.book = book;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            if(i%2==0){
                book.enterBook("人性的弱点");
            }else{
                book.enterBook("断舍离");
            }
        }
    }
}

/**
 * 人
 */
class Person extends Thread {
    Book book;

    public Person(Book book) {
        this.book = book;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            book.consumeBook();
        }
    }
}

/**
 * 书
 */
class Book {

    /**
     * 书
     */
    String book;
    /**
     * true是书店
     * false是人
     */
    boolean flag = true;

    /**
     * 书店进了哪些书
     */
    public synchronized void enterBook(String book) {
        // 当消费者消费时,生产者不能生产,需要等待消费者消费完
        if (!this.flag) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        // 书店进了什么书,并唤醒人去消费,并把标志位改为false让消费者消费
        System.out.println("书店进了" + book + "书");
        this.book = book;
        this.notifyAll();
        this.flag = !this.flag;
    }


    /**
     * 人消费了什么书
     */
    public synchronized void consumeBook() {
        // 判断是否为True,为true就等待书店进书,不为false则消费者购买书,并唤醒书店生产书,并把标志位更改
        if (this.flag) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("人购买了" + this.book);
        this.notifyAll();
        this.flag = !flag;
    }
}

```

## 线程池的创建

> 为什么使用线程池?
>
> 因为经常创建和销毁线程,使用特别大的资源,比如并发情况下的线程,对性能影响很大.
>
> 线程池是什么?
>
> 提前创建好多个线程,放入线程池中,使用时直接获取,使用完后直接放入线程池中,这就避免了频繁创建和销毁线程,实现可重复利用.
>
> 线程池的优点:
>
> * 提高响应速度(减少创建线程的时间)
>
> * 降低资源消耗(重复利用线程池中的线程,不需要每次都创建)
>
> * 便于线程管理 (主要可以控制 线程池大小 最大线程数 线程没有任务时最多可以保持多长时间后会终止)

> * ExecutorService : 真正的线程池接口,常见子类ThreadPoolExecutror
>   * void execute(Runnable command) : 执行线程, 没有返回值,一般用于执行Runnable
>   * <T> Future<T> sumit(Callable<T> task) : 执行线程,有返回值,一般用于执行Callable
>   * void  shutdown() 关闭线程池
>
> * Executros : 工具类,线程池的工厂类,用于创建并返回不同类型的线程池 

#### 线程池的创建(Runnable)

> 1. 创建自定义线程类实现Runnable接口
> 2. 并重写run()方法
> 3. 创建线程对象
> 4. 创建执行服务 `ExecutorService service = Executors.newFixedThreadPool(3);`
> 5. 提交执行 `service.execute(new MyTest());`
> 6. 关闭服务 `service.shutdownNow();`

```java
package com.thread.demo;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Runnable线程池的创建
 *
 * @author zhou
 * @date 2021/04/12 07:47
 **/
public class ThreadPoolTest {
    public static void main(String[] args) {
        // 1. 创建线程池
        ExecutorService service = Executors.newFixedThreadPool(10);
        // 2. 执行线程
        service.execute(new MyTest());
        service.execute(new MyTest());
        service.execute(new MyTest());
        service.execute(new MyTest());
        // 3. 关闭线程池
        service.shutdownNow();
    }
}

class MyTest implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

#### 线程池的创建(Callable)

> 1. 创建自定义线程类实现Callable接口
> 2. 重写call()方法,需要抛出异常
> 3. 创建线程对象
> 4. 创建执行服务 `ExecutorService service = Executors.newFixedThreadPool(10);`
> 5. 提交执行 `Future<String> t1 = service.submit(new Test());`
> 6. 获取结果  `t1.get();`
> 7. 关闭服务 `executorService.shutdown();`

图片下载例子

```java
package com.thread.demo;

import java.util.concurrent.*;

/**
 * Callable接口实现线程池
 *
 * @author zhou
 * @date 2021/04/12 08:07
 **/
public class ThreadPoolTestCallable {
    public static void main(String[] args) {
        // 1.创建线程池服务
        ExecutorService service = Executors.newFixedThreadPool(10);
        // 2.执行线程,并接收Callable返回值
        Future<String> t1 = service.submit(new Test());
        Future<String> t2 = service.submit(new Test());
        Future<String> t3 = service.submit(new Test());
        Future<String> t4 = service.submit(new Test());
        // 3.把接收出来的值打印出来
        try {
            System.out.println(t1.get());
            System.out.println(t2.get());
            System.out.println(t3.get());
            System.out.println(t4.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }finally {
            // 4.关闭线程池
            service.shutdown();
        }
    }
}

class Test implements Callable<String> {
    @Override
    public String call() throws Exception {
        return Thread.currentThread().getName();
    }
}

```

## 