# 数据结构和算法

## 数据结构和算法是什么?

### **数据结构和算法是什么**

> 是**数据对象**在计算机中的组织方式
>
> 1. 逻辑结构 如 线性结构 非线性结构都是逻辑结构
>
> 2. 物理存储结构 (是指在内存中的组织形式,如是连续的内存空间,还是不连续的内存空间)
>
> 数据对象必定与一系列加上的其上的**操作**相关性(相当与提出解决方案,并不管你怎么实现)
>
> 完成这些操作所用的方法就是**算法** (实现并解决一系列所用的方法是算法)

### **什么是好的程序**

> 优秀的程序 = 数据结构 + 算法

## 数据结构

### 线性结构

#### 线性结构的特点

1. 数据数据之间是一对一的线性关系
2. 线性结构分为顺序存储结构和链式存储结构
   > 顺序存储的线性表又称（顺序表），顺序表中存储的元素是连续的。如数组
   >
   > 链式存储线性表又称（链表）链表存储中的元素不一定是连续的，元素节点上存放着相邻的元素的地址信息
3. 线性结构常见的有： 数组、、队列、链表和栈等等

#### 稀疏数组（Sparse）

##### **为什么使用稀疏数组**

> 当一个数组中大部分元素为0或相同值时，可以用稀疏数组来对原数组进行优化并保存该数组

##### **稀疏数组基本概念**

> 记录数组一共有几行几列和有多少不同的值
>
> 再记录每个不同值的对应的行、列、值，并记录到一个小规模的数组中，从而达到优化程序的目的

![image-20210128194444836](E:\Nodes\images\image-20210128194444836.png)

##### **二维数组转稀疏数组思路**

1. 遍历二维数组有多少不等于0的值（sum）
2. 通过sum创建一个稀疏数组` int[][] sparseArr = new int [sun + 1] [3];`
3. 将二维数组中的有效数据和行列存入稀疏数组中

##### **稀疏数组转二维数组思路**

1. 通过稀疏数组中第一行的行和列创建一个二维数组` int [][] cheesArr2 = new int [sparseArr[0][0]][sparseArr[0][1]]`

2. 在读取稀疏数组第二行后面的数据，并赋给二维数组（也就是将后面值的数据（用稀疏数组的行和列来进行对那个位置的二维数组的值进行改变）添加到二维数组中去）

##### **代码的实现**


```java
  package sparse;
  
  /**
   * @Descriprion 棋盘制作
   * @Author zhou
   * @Date 2020/12/17 16:18
   */
  public class SparseArray {
  
      public static void main(String[] args) {
          // 二维数组转稀疏数组
          int[][] sparse = SparseArray.sparse();
          // 稀疏数组转二维数组
          SparseArray.chess(sparse);
  
      }
  
      /**
       * Descriprion 稀疏数组转二维数组
       *
       * @param: sparse 二维数组
       * @author zhou
       * @date 2020-12-18
       */
      public static void chess(int[][] sparse) {
          // 稀疏数组转二维数组
          // 1. 通过稀疏数组中的行和列创建二维数组
          int[][] chessArray = new int[sparse[0][0]][sparse[0][1]];
          // 2. 把稀疏数组中的值存入二维数组中去
          System.out.println("稀疏数组转二维数组");
          for (int i = 1; i < sparse.length; i++) {
              chessArray[sparse[i][0]][sparse[i][1]] = sparse[i][2];
          }
          for (int[] ros : chessArray) {
              for (int ro : ros) {
                  System.out.print("\t" + ro);
              }
              System.out.println();
          }
      }
  
  
      /**
       * Descriprion
       * 二维数组转稀疏数组
       *
       * @return: int[][] 稀疏数组
       * @author zhou
       * @date 2020-12-18
       */
      public static int[][] sparse() {
          // 1. 创建原始数组并填写原始数据
          int chessArray[][] = new int[11][11];
          // 给棋盘赋值 0表示没有值，1表示白棋 2表示黑棋
          chessArray[1][2] = 1;
          chessArray[2][3] = 2;
          chessArray[3][4] = 2;
          System.out.println("\t原始棋盘");
          // 2. 遍历数组获得棋盘中的有效数据的个数（sum），通过这个有效数据的个数就可以创建一个知道大小的稀疏数组
          int sum = 0;
          for (int[] ros : chessArray) {
              for (int data : ros) {
                  if (data != 0) {
                      sum++;
                  }
                  System.out.print("\t" + data);
              }
              System.out.println();
          }
          // 创建一个稀疏数组
          int[][] sparseArr = new int[sum + 1][3];
          // 3. 将有效数据的数据存入稀疏数组中。
          // 第一组数据是把二维数组中的行和列和有效数据存入稀疏数组中
          sparseArr[0][0] = chessArray.length;
          sparseArr[0][1] = chessArray[0].length;
          sparseArr[0][2] = sum;
          // 程序计数器
          int count = 0;
          for (int i = 0; i < chessArray.length; i++) {
              for (int j = 0; j < chessArray[i].length; j++) {
                  // 当值不等于0时，把后续稀疏数组中的行和列的有效值存入稀疏数组中
                  if (chessArray[i][j] != 0) {
                      count++;
                      sparseArr[count][0] = i;
                      sparseArr[count][1] = j;
                      sparseArr[count][2] = chessArray[i][j];
                  }
  
              }
          }
          // 稀疏数组的打印
          for (int[] ros : sparseArr) {
              for (int ro : ros) {
                  System.out.print("\t" + ro);
              }
              System.out.println();
          }
  
          return sparseArr;
      }
  
  }
```

#### 队列(Queue)

##### **为什么使用队列**

> 解决实际生活中排队问题,先进先出,后进后出问题

##### **队列的基本概念**

> 队列是一个有序列表,可以用数组和链表来实现
>
> 队列遵循先进先出原则,先进来的数据先出去,后进来的数据后出去

![image-20210128194614379](E:\Nodes\images\image-20210128194614379.png)

##### **数组方式实现队列的思路**

1. 创建队列的基本元素 队列最大存放数量`(sizeMax) `队列头部(front) 队列尾部(rear) 队列存放的数据`(DataArr[])`
2. 初始化队列, front 和 rear 默认设置为 -1,通过传入进来的队列最大存放数量,创建一个数组队列
3. 判断队列是否为空(front == rear) , 判断队列是否存满 `(sizeMax - 1 == rear)`
4. 添加数据时判断队列是否存满,存满 抛异常,没存漫 rear 加1,并添加数据, 取出元素时判断队列是否为空,为空 抛异常, 不为空front 加1,并取出数据

代码实现

```java
​```java
/**
 * @Descriprion 模拟队列
 * @Author zhou
 * @Date 2020/12/19 16:59
 */
public class Queue {
    /**
     * 队列最大存放数量
     */
    private int sizeMax;
    /**
     * 队列头部
     */
    private int front;
    /**
     * 队列尾部
     */
    private int rear;
    /**
     * 队列存放的数据
     */
    private int DataArr[];

    /**
     * 队列的初始化
     *
     * @param sizeMax
     * @author zhou
     * @date 2020/12/22 18:57
     */
    public Queue(int sizeMax) {
        // 初始化队列sizeMax为最大存放的数量并创建对应数量的数组
        this.sizeMax = sizeMax;
        this.DataArr = new int[sizeMax];
        // 初始化队列前面和后面数据下标为 -1
        this.front = -1;
        this.rear = -1;
    }

    /**
     * 判断队列是否为空
     *
     * @return boolean
     * @author zhou
     * @date 2020/12/22 19:15
     */
    public boolean isEmpty() {
        return front == rear;
    }

    /**
     * 判断队列数据是否存满
     *
     * @return boolean
     * @author zhou
     * @date 2020/12/22 19:15
     */
    public boolean isFull() {
        return sizeMax - 1 == rear;
    }

    /**
     * 添加数据到队列
     *
     * @param data
     * @return void
     * @author zhou
     * @date 2020/12/22 19:19
     */
    public void addQueue(int data) {
        if (isFull()) {
            throw new RuntimeException("队列已满,不能存放数据了");
        }
        // 添加数据队列尾部元素 +1, 并添加数据
        rear++;
        DataArr[rear] = data;
    }

    /**
     * 取队列中的数据,也就是取先进先出的数据
     *
     * @return int
     * @author zhou
     * @date 2020/12/22 19:22
     */
    public int getQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        // 队列前部 +1, 并取出头部数据
        front++;
        return DataArr[front];
    }

    /**
     * 显示队列元素
     * @author zhou
     * @date 2020/12/22 19:55
     * @return void
     */
    public void showQueue() {
        if (isEmpty()) {
            new RuntimeException("队列为空");
        }
        for (int data : DataArr) {
            System.out.println(data);
        }
    }

    /**
     * 显示队列头部元素
     * @author zhou
     * @date 2020/12/22 19:56
     * @return int
     */
    public int headQueue(){
        if (isEmpty()) {
           throw new RuntimeException("队列为空");
        }
        return DataArr[front + 1];
    }
}
​```
```
##### **数组方式实现循环队列的实现思路** 

之前的队列实现出来有一个队列无法重复使用的问题, 解决该问题的思路如下

1. front 指向队列第一个数据的下标
2. rear 指向队列最后一个数据后一个数据的下标
3. 判断队列是否存满的公式.` (rear + 1) % maxSize == front`
4. 判断队列是否为空 `front == rear`
5. 获取队列中有效数据的个数公式: `(rear + maxSize - front) % sizeMax`

##### **代码实现**

```java
      /**
 * @Descriprion 数组队列的实现,使用循环队列
 * @Author zhou
 * @Date 2020/12/19 16:54
 */
public class ArrayQueue {

    /**
     * 之前的队列实现出来有一个队列无法重复使用的问题, 解决该问题的思路如下:
     * 1. front 指向队列第一个数据的下标
     * 2. rear 指向队列最后一个数据后一个数据的下标
     * 3. 判断队列是否存满的公式. (rear + 1) % maxSize == front
     * 4. 判断队列是否为空 front == rear
     * 5. 获取队列中有效数据的个数公式: (rear + maxSize - front) % sizeMax
     */


    /**
     * 队列最大存放数量
     */
    private int maxSize;
    /**
     * 队列头部
     */
    private int front;
    /**
     * 队列尾部
     */
    private int rear;
    /**
     * 队列存放的数据
     */
    private int DataArr[];

    /**
     * 队列的初始化
     *
     * @param maxSize
     * @author zhou
     * @date 2020/12/22 18:57
     */
    public ArrayQueue(int maxSize) {
        // 初始化队列sizeMax为最大存放的数量并创建对应数量的数组
        this.maxSize = maxSize;
        this.DataArr = new int[maxSize];
    }

    /**
     * 判断队列是否为空
     *
     * @return boolean
     * @author zhou
     * @date 2020/12/22 19:15
     */
    public boolean isEmpty() {
        return front == rear;
    }

    /**
     * 判断队列数据是否存满
     *
     * @return boolean
     * @author zhou
     * @date 2020/12/22 19:15
     */
    public boolean isFull() {
        return (rear + 1) % maxSize == front;
    }

    /**
     * 添加数据到队列
     *
     * @param data
     * @return void
     * @author zhou
     * @date 2020/12/22 19:19
     */
    public void addQueue(int data) {
        if (isFull()) {
            throw new RuntimeException("队列已满,不能存放数据了");
        }
        DataArr[rear] = data;
        rear = (rear + 1) % maxSize;
    }

    /**
     * 取队列中的数据,也就是取先进先出的数据
     *
     * @return int
     * @author zhou
     * @date 2020/12/22 19:22
     */
    public int getQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        int value = DataArr[front];
        front = (front + 1) % maxSize;
        return value;
    }

    /**
     * 显示队列元素
     *
     * @return void
     * @author zhou
     * @date 2020/12/22 19:55
     */
    public void showQueue() {
        if (isEmpty()) {
            new RuntimeException("队列为空");
        }
        for (int i = front; i < front + size(); i++) {
            System.out.println(DataArr[i % maxSize]);
        }
    }

    /**
     * 计算队列中有效数据个数
     *
     * @return int
     * @author zhou
     * @date 2020/12/29 19:11
     */
    public int size() {
        return (rear + maxSize - front) % maxSize;
    }

    /**
     * 显示队列头部元素
     *
     * @return int
     * @author zhou
     * @date 2020/12/22 19:56
     */
    public int headQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        return DataArr[front];
    }
}

```

**测试方法**

```java
import java.util.Scanner;

/**
 * 数组队列的使用
 *
 * @author zhou
 * @date 2020/12/22 19:02
 **/
public class QueueArray {
    public static void main(String[] args) {
        // 数组循环队列和数组队列改一下类名和添加最大存放数量即可
        ArrayQueue queueArray = new ArrayQueue(3);
        char key = ' ';
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        while (loop) {
            System.out.println("s (show): 显示队列");
            System.out.println("a (add): 添加数据");
            System.out.println("g (get): 取出队列头部数据");
            System.out.println("h (head): 显示头部数据");
            System.out.println("e (exit): 退出程序");
            key = scanner.next().charAt(0);
            switch (key) {
                case 's':
                    queueArray.showQueue();
                    break;
                case 'a':
                    try {
                        System.out.println("请输入一个数字");
                        int value = scanner.nextInt();
                        queueArray.addQueue(value);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    break;
                case 'g':
                    try {
                        System.out.println("取出的数据是" + queueArray.getQueue());
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    break;
                case 'h':
                    try {
                        System.out.println(queueArray.headQueue());
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    break;
                case 'e':
                    loop = false;
                    break;
                default:
                    break;
            }
        }
    }
}

```

#### 链表(Linked List)

##### **单链表**

###### **单链表的基本概念**

> 链表是用节点方式存储的
>
> 每个节点都包含 data域 和 next域 : 指向下一个节点
>
> 链表中每个节点的内存地址不一定是连续存储的
>
> 链表中分带头结点的链表和不带头结点的链表 具体带不带头结点,根据实际需求来定

![image-20210128202017452](E:\Nodes\images\image-20210128202017452.png)

###### **思路分析**

**添加**

* 不带排序的添加代码
  > 1. 通过遍历找到链表中最后一个位置
  > 2. 在链表最后一个位置添加节点` temp.next = HeroNode;`
* 带排序添加的代码
  > 1. 找到链表中想要插入位置的前一个节点
  > 2. 插入节点  `heroNode.next = temp.next;
  >    temp.next = heroNode;`

**遍历**

> 1. 通过循环的temp后移来实现遍历` temp = temp.next;`

**更改**

> 1. 找到id匹配的节点位置
> 2. 修改节点

**删除**

> 1. 先找到要删节点的前一个节点
> 2. 删除节点` temp.next = temp.next.next;`

###### **代码实现**

```java
package linkedList;

import java.util.Stack;

/**
 * 需求: 实现一个水浒传各英雄的排名,使用链表方式
 * 带头结点的单链表
 *
 * @author zhou
 * @date 2021/01/04 11:05
 **/
public class SingleLinkedListDemo {
    public static void main(String[] args) {
        HeroNode hero1 = new HeroNode(1, "test1", "t1");
        HeroNode hero2 = new HeroNode(2, "test2", "t2");
        HeroNode hero3 = new HeroNode(3, "test3", "t3");
        HeroNode hero4 = new HeroNode(4, "test4", "t4");
        // 创建头链表
        SingleLinkedList singleLinkedList = new SingleLinkedList();
        SingleLinkedList singleLinkedList2 = new SingleLinkedList();
        // singleLinkedList.add(hero1);
        // singleLinkedList.add(hero4);
        // singleLinkedList.add(hero2);
        // singleLinkedList.add(hero3);
        // 没有排序的添加
        // singleLinkedList.list();

        singleLinkedList.addOrder(hero1);
        singleLinkedList.addOrder(hero4);
        singleLinkedList.addOrder(hero2);
        singleLinkedList.addOrder(hero3);

        singleLinkedList.reverse(singleLinkedList.getHead());
        // System.out.println("排序后的添加");
        singleLinkedList.list();
        System.out.println(singleLinkedList.size(singleLinkedList.getHead()));
        HeroNode heroNode = singleLinkedList.find(singleLinkedList.getHead(), 2);
        System.out.println(heroNode);
        // singleLinkedList.reversalLikedList(singleLinkedList.getHead());
        System.out.println("反转后的链表打印");
        // singleLinkedList.list();

        singleLinkedList.printLikedList(singleLinkedList.getHead());
        // System.out.println("通过id 来更改姓名");
        // linkedList.HeroNode h2 = new linkedList.HeroNode(2, "wo", "飞");
        // singleLinkedList.update(h2);
        // singleLinkedList.list();
        // singleLinkedList.delete(1);
        // System.out.println("删除后结果");
        // singleLinkedList.list();
    }
}

/**
 * 节点类
 */
class HeroNode {
    public int id;
    public String name;
    public String nickname;
    public HeroNode next;

    public HeroNode(int id, String name, String nickname) {
        this.id = id;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "linkedList.HeroNode{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' +
                '}';
    }
}

/**
 * 实现链表的添加节点和遍历节点的思路
 * 1. 创建链表
 * 2. 把节点添加到链表最后, 怎么知道链表最后? next == null 就代表该节点为最后节点
 * 3. 遍历链表 也需要判断链表是最后,来退出循环遍历 next == null
 */
class SingleLinkedList {
    /**
     * 头结点
     */
    private HeroNode head = new HeroNode(0, "", "");

    public HeroNode getHead() {
        return head;
    }

    /**
     * 不自动排序的添加节点
     *
     * @param heroNode
     */
    public void add(HeroNode heroNode) {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode temp = head;
        while (true) {
            if (temp.next == null) {
                break;
            }
            temp = temp.next;
        }
        temp.next = heroNode;
    }

    /**
     * 排序的方式添加节点, 通过元素的id
     * 实现一个水浒传各英雄的排名, 通过排序方式添加节点
     */
    public void addOrder(HeroNode heroNode) {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode temp = head;
        boolean flag = false;
        while (true) {
            if (temp.next == null) {
                break;
            }
            // 找到链表中元素的插入位置
            if (temp.next.id > heroNode.id) {
                break;
            } else if (heroNode.id == temp.next.id) {
                // 判断编号是否一致
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if (flag) {
            System.out.println("不能插入相同的id的节点");
        } else {
            // 插入
            heroNode.next = temp.next;
            temp.next = heroNode;
        }
    }

    /**
     * 通过id 来修改节点
     *
     * @param heroNode
     */
    public void update(HeroNode heroNode) {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode temp = head.next;
        if (temp == null) {
            return;
        }
        // 创建一个 boolean flag 来判断是否找到该节点
        boolean flag = false;
        while (true) {
            if (temp == null) {
                break;
            }
            if (temp.id == heroNode.id) {
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if (flag) {
            temp.name = heroNode.name;
            temp.nickname = heroNode.nickname;
        } else {
            System.out.println("您要修改的节点不存在");
        }
    }

    /**
     * 删除节点
     *
     * @param id
     * @return void
     * @author zhou
     * @date 2021/01/05 15:16
     */
    public void delete(int id) {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode temp = head;
        // 标记是否找到前一个节点
        boolean flag = false;
        // 1. 先找到要删节点的前一个节点
        while (true) {
            if (temp == null) {
                break;
            }
            if (temp.next.id == id) {
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if (flag) {
            // 2.删除节点 temp.next = temp.next.next;
            temp.next = temp.next.next;
        } else {
            System.out.println("无法删除节点,id不匹配");
        }
    }

    /**
     * 链表的反转
     *
     * @param heroNode
     */
    public void reverse(HeroNode heroNode) {
        // 判断要反转的链表节点是否为空和判断链表只有一个节点
        if (heroNode.next == null || heroNode.next.next == null) {
            return;
        }
        // 创建一个临时链表来存储要反转的节点
        HeroNode reverse = new HeroNode(0, "", "");
        // 通过一个临时变量指向原有节点
        HeroNode temp = heroNode.next;
        // 创建一个保存下一个节点的临时变量
        HeroNode next = null;
        // 3. 循环遍历节点并实现反转
        while (temp != null) {
            // 把下一个节点存储到临时节点上,为了方便对原链表进行操作
            next = temp.next;
            // 将反转链表的下一个节点赋值给原链表的下一个节点
            temp.next = reverse.next;
            // 在将更改后的原节点赋值给反转节点,就实现了反转
            reverse.next = temp;
            // 将保存好的原链表的下一个节点赋值给原节点,好进行下一次的反转
            temp = next;
        }
        // 将反转后的链表赋值给原链表
        heroNode.next = reverse.next;
    }

    /**
     * 遍历
     *
     * @return void
     * @author zhou
     * @date 2021/01/05 15:15
     */
    public void list() {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode temp = head;
        while (true) {
            if (temp.next == null) {
                break;
            }
            // temp后移遍历
            temp = temp.next;
            System.out.println(temp);
        }
    }

    /**
     * 求单链表中有效节点的个数
     *
     * @param head
     * @return int
     * @author zhou
     * @date 2021/01/11 18:35
     */
    public int size(HeroNode head) {
        // 判断链表是否为空
        if (head.next == null) {
            return 0;
        }
        HeroNode temp = head.next;
        int size = 0;
        // 循环遍历链表,并记录节点个数
        while (temp != null) {
            size++;
            temp = temp.next;
        }
        return size;
    }

    /**
     * 查找单链表中的倒数第k个结点 【新浪面试题】
     *
     * @param head
     * @param value
     * @return linkedList.HeroNode
     * @author zhou
     * @date 2021/01/11 18:46
     */
    public HeroNode find(HeroNode head, int value) {
        // 判断单链表是否为空和是否只有一个节点
        if (head.next == null) {
            return null;
        }
        HeroNode temp = head.next;
        // 通过遍历得到总数
        int size = size(head);
        // 对查询倒数第几个进行校验
        if (value <= 0 || value > size) {
            return null;
        }
        // 得到总数用总数减去取出第几个节点就可以拿到该节点
        for (int i = 0; i < size - value; i++) {
            temp = temp.next;
        }
        return temp;
    }

    /**
     * 单链表的反转【腾讯面试题，有点难度】
     *
     * @param head
     * @return void
     * @author zhou
     * @date 2021/01/11 19:39
     */
    public void reversalLikedList(HeroNode head) {
        if (head.next == null || head.next.next == null) {
            return;
        }
        // 定义两个辅助变量
        HeroNode temp = head.next;
        HeroNode next = null;
        // 创建一个新的链表
        HeroNode reversal = new HeroNode(0, "", "");
        while (temp != null) {
            // 将原链表的下一个节点存储到辅助变量上,
            next = temp.next;
            // 将原链表的下一个节点指向新链表的下一个节点
            temp.next = reversal.next;
            // 将链表当前的节点赋值给新链表的下一个节点
            reversal.next = temp;
            // 将原链表的下一个节点重新赋值给临时变量
            temp = next;
        }
        // 将反转后的链表赋值给原链表
        head.next = reversal.next;
    }

    /**
     * 从尾到头打印单链表 【百度，要求方式1：反向遍历 。 方式2：Stack栈】
     *
     * @param head
     * @return void
     * @author zhou
     * @date 2021/01/11 19:36
     */
    public void printLikedList(HeroNode head) {
        if (head.next == null) {
            return;
        }
        HeroNode temp = head.next;
        Stack<HeroNode> heroNodes = new Stack<>();
        while (temp != null) {
            heroNodes.add(temp);
            temp = temp.next;
        }
        while (heroNodes.size() > 0) {
            System.out.println(heroNodes.pop());
        }
    }

    /**
     * 合并两个有序的单链表，合并之后的链表依然有序【课后练习.】
     *
     * @param head1
     * @param head2
     * @return linkedList.HeroNode
     * @author zhou
     * @date 2021/01/11 19:36
     */
    public HeroNode linkedListOrder(HeroNode head1, HeroNode head2) {
        // 判断链表是否为空
        if (head1 == null && head2 == null) {
            return null;
        }
        // 2. 创建一个链表用于保存合并之后的链表
        HeroNode heroNode = new HeroNode(0, "", "");
        // 3. 通过循环添加把链表存入新链表中
        HeroNode temp = head1.next;
        while (temp != null) {
            addOrder(temp);
            temp = temp.next;
        }
        temp = head2;
        while (temp != null) {
            addOrder(temp);
            temp = temp.next;
        }
        heroNode.next = this.head.next;
        return heroNode;
    }
}

```

##### 双链表

###### 双向链表的特点

> 可以实现自我删除
> 遍历可以向前遍历也可以向后遍历

###### 思路分析

**遍历**

> 和单链表基本相同,不过可以向前或向后遍历

**添加**

>和单链表基本相同,不同的是要将双向链表的前一个节点和后一个节点关联起来
>	`temp.next = heronode; heroNode.per = temp`

**修改**

>和单链表相同

**删除**

> 找到要删除节点的位置,然后实现自我删除
> 	`temp.per.next = temp.next; temp.next.per = temp.per;`

###### 代码实现

```java
package linkedList;

import java.util.function.DoubleBinaryOperator;

/**
 * 双向链表的代码实现
 *
 * @author zhou
 * @date 2021/01/24 19:15
 **/
public class DoubleLinkedListDome {

    public static void main(String[] args) {
        HeroNode2 hero1 = new HeroNode2(1, "test1", "t1");
        HeroNode2 hero2 = new HeroNode2(2, "test2", "t2");
        HeroNode2 hero3 = new HeroNode2(3, "test3", "t3");
        HeroNode2 hero4 = new HeroNode2(4, "test4", "t4");

        DoubleLinkedList doubleLinkedList = new DoubleLinkedList();
        // 添加
        doubleLinkedList.add(hero1);
        doubleLinkedList.add(hero2);
        doubleLinkedList.add(hero3);
        doubleLinkedList.add(hero4);

        doubleLinkedList.list();

        // 修改
        System.out.println("修改");
        HeroNode2 heroNode2 = new HeroNode2(3, "小山", "test66");
        doubleLinkedList.update(heroNode2);

        doubleLinkedList.list();

        // 删除
        System.out.println("删除");
        doubleLinkedList.delete(2);
        doubleLinkedList.list();
    }
}

/**
 * 双向链表的控制类
 */
class DoubleLinkedList {

    /**
     * 头结点
     */
    private HeroNode2 head = new HeroNode2(0, "", "");

    public HeroNode2 getHead() {
        return head;
    }

    /**
     * 思路分析
     * 1.遍历
     * 和单链表基本相同,不过可以向前或向后遍历
     * 2.添加
     * 和单链表基本相同,不同的是要将双向链表的前一个节点和后一个节点关联起来
     * temp.next = heroNode;
     * heroNode.per = temp
     * 3.修改
     * 和单链表相同
     * 4.删除
     * 找到要删除节点的位置,然后实现自我删除
     * temp.per.next = temp.next
     * temp.next.per = temp.per
     */

    /**
     * 不自动排序的添加节点
     *
     * @param heroNode
     */
    public void add(HeroNode2 heroNode) {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode2 temp = head;
        while (true) {
            if (temp.next == null) {
                break;
            }
            temp = temp.next;
        }
        heroNode.per = temp;
        temp.next = heroNode;
    }

    /**
     * 遍历
     *
     * @return void
     * @author zhou
     * @date 2021/01/05 15:15
     */
    public void list() {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode2 temp = head;
        while (true) {
            if (temp.next == null) {
                break;
            }
            // temp后移遍历
            temp = temp.next;
            System.out.println(temp);
        }
    }

    /**
     * 通过id 来修改节点
     *
     * @param heroNode
     */
    public void update(HeroNode2 heroNode) {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode2 temp = head.next;
        if (temp == null) {
            return;
        }
        // 创建一个 boolean flag 来判断是否找到该节点
        boolean flag = false;
        while (true) {
            if (temp == null) {
                break;
            }
            if (temp.id == heroNode.id) {
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if (flag) {
            temp.name = heroNode.name;
            temp.nickname = heroNode.nickname;
        } else {
            System.out.println("您要修改的节点不存在");
        }
    }

    /**
     * 删除节点
     *
     * @param id
     * @return void
     * @author zhou
     * @date 2021/01/05 15:16
     */
    public void delete(int id) {
        // 头结点不能动,所以用一个临时变量来指向
        HeroNode2 temp = head.next;
        // 标记是否找到节点
        boolean flag = false;
        // 1. 先找到要删除的节点
        while (true) {
            if (temp == null) {
                break;
            }
            if (temp.id == id) {
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if (flag) {
            // 2.删除节点
            temp.per.next = temp.next;
            // 判断当前节点后一个节点是否为空,为空就不执行把下一个节点的上一个节点的指针指向当前节点的上一个节点
            if (temp.next != null) {
                temp.next.per = temp.per;
            }
        } else {
            System.out.println("无法删除节点,id不匹配");
        }
    }

}


/**
 * 双向链表的节点类
 */
class HeroNode2 {
    public int id;
    public String name;
    public String nickname;
    public HeroNode2 next;
    public HeroNode2 per;

    public HeroNode2(int id, String name, String nickname) {
        this.id = id;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "linkedList.HeroNode2{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' +
                '}';
    }
}
```



### 非线性结构

二维数组、多维数组、广义表、树结构、图结构

## 算法

