# Mysql数据库

## 数据库介绍与安装

### 为什么要学习数据库?

> 因为在一般程序中,保存数据到磁盘或内存,数据量一大,会带来不好维护数据和数据安全性和数据数据完整性的问题,所以为了解决上述问题,所以开发了数据库,所以为了解决上述问题,所以需要掌握数据库.
>
> 也因为日趋变化,要从事程序开发,都必须使用到数据库,而Mysql 是其中的佼佼者,所以要掌握它

### 什么是数据库

> 数据库是按照一定的数据结构来组织 存储和管理数据的仓库

### DBMS(数据库管理系统)

>  是一种操纵和管理数据库的大型软件,用于建立 使用和维护数据库,简称DBMS
>
> 它对数据库进行统一的管理和控制,以保证数据库的安全性和完整性
>
> 数据库管理系统是数据库系统的核心,是管理数据库的软件
>
> 我们一般说的数据库,就是指的DBMS,数据库管理系统

### Mysql简介

> 前世:瑞典MySQL AB 公司开发
>
> 今生:现在属于 Oracle 旗下产品
>
> MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一
>
> 开源的数据库软件,体积小、速度快、总体拥有成本低,一般中小型网站的开发都选择 MySQL 作为网站数据库,大型网站也使用,不过用的是Mysql中的集群
>
> Mysql是使用SQL语言来操作数据库的

### 数据库的分类类型

#### 关系型数据库 (SQL)

* Mysql Oracle DB2 Sql Server
* 通过表与表之间,行和列之间的关系进行数据的存储, 如: 学生表 课程表

#### 非关系型数据库 (NoSQL) not only

* Readis MongDB
* 非关系型数据库,对象存储,通过对象的自身属性来决定

### Mysql数据库的安装

> TODO

### Mysql基本操作命令

```mysql
-- 单行注释
/*
  多行注释
*/
-- 所有命令最后都要加英文分号结尾(;)
show databases; -- 查看所有数据库
use 数据库名; -- 切换数据库
show tables; -- 查看数据库中的所有表
describe 表名; -- 查看数据表的结构
create database 表名; -- 创建表
exit; -- 退出链接
```

## SQL功能分类

### 什么是SQL

> SQL是结构化查询语言（Structured Query Language）简称SQL
>
> SQl是专门为数据库而建立的操作命令集,是一种功能齐全的数据库语言
>
> 使用它时,只需要发出""做什么"的命令,"怎么做"是不用使用者考虑的

### SQL的功能分类

> SQL语言共分为四大类：数据定义语言DDL，数据操纵语言DML，数据查询语言DQL，数据控制语言DCL。
>
> 数据定义语言(DDL)，例如：CREATE、DROP、ALTER等语句。
>
> 数据操作语言(DML)，例如：INSERT（插入）、UPDATE（修改）、DELETE（删除）语句。
>
>  数据查询语言(DQL)，例如：SELECT 语句。（一般不会单独归于一类，因为只有一个语句）。
>
> 数据控制语言(DCL)，例如：GRANT、REVOKE等语句

### SQL的数据类型

> Mysql 中定义数据字段的类型对数据库的优化是非常重要的
>
> Mysql 支持所有标准SQL数值数据类型
>
> 在Mysql 中字符串类型和日期类型都要用单引号括起来 'YYYY-MM-DD'  '字符串'

#### 数值

| 类型      | 大小                                     | 范围（无符号）                                               |      用途       |
| :-------- | :--------------------------------------- | :----------------------------------------------------------- | :-------------: |
| TINYINT   | 1 byte                                   | (0，255)                                                     |    小整数值     |
| SMALLINT  | 2 bytes                                  | (0，65 535)                                                  |    大整数值     |
| MEDIUMINT | 3 bytes                                  | (0，16 777 215)                                              |    大整数值     |
| INT       | 4 bytes                                  | (0，4 294 967 295)                                           |    大整数值     |
| BIGINT    | 8 bytes                                  | (0，18 446 744 073 709 551 615)                              |   极大整数值    |
| FLOAT     | 4 bytes                                  | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE    | 8 bytes                                  | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL   | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               |     小数值      |

#### 字符串

| 类型       | 大小                  | 用途                            |
| ---------- | --------------------- | ------------------------------- |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

#### 时间

| 类型      | 大小 | 用途                | 用途                     |
| --------- | ---- | ------------------- | ------------------------ |
| DATE      | 3    | YYYY-MM-DD          | 日期值                   |
| TIME      | 3    | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1    | YYYY                | 年份值                   |
| DATETIME  | 8    | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4    | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

### 常用的数据类型

> int:大整数类型
>
> decimal:   小数类型,数值要计算精确的话用它 一般金融业务中使用
>
> double : 浮点型, 例如 double(5,2) 表示最多为5位,其中必须有2位小数,即最大值为999.99
>
> char: 固定长度字符串类型;  char(10) 'abc          '
>
> verchar: 可变长度字符串类型; varchar(10) 'abc'
>
> text: 文本类型
>
> blol: 二进制类型,一般用于存储图片
>
> date: 日期类型 'yyyy-MM-dd'
>
> time: 时间类型 'hh:mm:ss'
>
> datetime: 日期时间类型 'yyyy-MM-dd hh:mm:ss'
>
> timestamp: 时间戳类型

## SQL-数据定义语言DDL

### 创建数据库

```mysql
CREATE DATABASE [IF NOT EXISTS] 数据库名 character set utf8;
-- IF NOT EXISTS 是判断语句,可加可不加
-- 例子
create database if not exists mydb character set utf8;
```

### 修改数据库的字符集

```mysql
alter database 数据库名 character set gbk
-- 例子
alter database mydb character set gbk;
```

### 删除数据库

```mysql
DROP [IF EXISTS] 数据库名;
-- 例子
drop if exists mydb;
```

### 查看所有的数据库

```mysql
SHOW DATABASES;
```

### 使用数据库

```mysql
USE 数据库名;
-- 例子
use mydb;
```

### 创建表

```mysql
-- 先使用数据库
-- 输入创表命令
create table `表名` (
	`列名1` 列的类型 [约束],
    `列名2` 列的类型 [约束],
    ...
    `列名N` 列的类型 [约束]
);

-- 例子
use mydb;
create table student(
	stu_id bigint,
	stu_name varchar(50),
	stu_age int(3)
);

```

添加一列

```mysql
alter table 表名 add 列名 数据类型;
-- 例子
alter table student add stu_gender tinyint;
```

修改一个表的字段类型

```mysql
alter table 表名 modify 列名 数据类型;
-- 例子
alter table student modify stu_name varchar(30);
```

修改表名

```mysql
rename table 表名 to 新表名;
-- 例子
rename table student to newstu;
```

修改表的字符集为GBK

```mysql
alter table 表名 character set 字符集;
-- 例子
alter table student character set gbk;
```

修改表的列名和列类型

```mysql
-- 既可以修改列名也可以修改列列类型
alter table 表名 change 列名 新列名 数据类型;
-- 例子
alter table student change s_name stu_name varchar(30);
```

查看表的字段信息

```mysql
desc 表名;
-- 例子
desc student;
```

查看表的创建细节

```mysql
show create table 表名;
-- 例子
show create table student;
```

删除一列

```mysql
alter table 表名 drop 列名;
-- 例子
alter table student drop stu_gender;
```

### 删除表

```mysql
drop table 表名;
-- 例子
drop table student;
```

## SQL-数据操作语言DML

### 插入操作

> 插入操作的注意事项:
>
> 1. 列名与列值的类型,个数,顺序要一一对应
> 2. 值不要超过列定义的长度
> 3. 字符串类型和时间类型一样,都要用引号引起来 如: '字符串' , '日期'
>
> 插入小技巧
>
> 可以省略列名,只写value后面的列值,但是列值要与该表中的所有的列名要一一对应,并且值不要超过列定义的长度

```mysql
insert into 表名 (列名1,列名2,...N)  value (列值1, 列值2,...N);
-- 例子
insert into student (stu_id, stu_name, stu_age) value(1, 'zs', 10);
insert into student (stu_name, stu_age) value('ls', 20);
insert into student value(3, 'ww', 12);
```

批量插入

```mysql
insert into 表名 (列名1,列名2,...N)  values (列值1, 列值2,...N),(列值1, 列值2,...N), ...N;
-- 例子
insert into student (stu_id, stu_name, stu_age) values(4,'sdfa',22),(5,'df',33);
insert into student (stu_name, stu_age) values('sdf23a',21),('dddf',23);
insert into student values(6,'sdfgcvbfa',15),(7,'dfsdsa',31);
```

### 更新操作

```mysql
update 表名 set 列名1=列值1,列名2=列值2, ...N where 列名 = 值;
-- 把所有学生分数改为90分
update student set stu_score = 90;
-- 把姓名为zs的学生改为60分
update student set stu_score = 60 where stu_name = 'zs';
-- 把姓名为ls的年龄改为20岁和分数改为70分
update student set stu_age = 20, stu_score = 70 where stu_name = 'ls';
-- 把 ww的年龄在原来的基础上加1
update student set stu_age = stu_age + 1 where stu_name = 'ww';
```

修改数据库密码

```mysql
-- 修改Mysql数据库密码 8.0版本之前
use mysql;
-- 更改密码
update mysql.user set authentication_string = password('123') where user = 'root' and Host = 'localhost';
-- 刷新Mysql的系统权限相关表
flush privileges;

-- 更改Mysql数据库密码 8.0
ALTER USER'root'@'localhost' IDENTIFIED BY '新密码';
```

```bash
# 使用mysqladmin更改密码 8.0
mysqladmin -u root -p password 新密码
# 输入旧密码,然后修改成功
```

### 删除操作

```mysql
delete from 表名 where 列名 = 值,...N;
-- 删除操作,加where条件的
delete from student where stu_name = 'ww';
-- 不加where条件,是删除所有数据
delete from student;
```

> truncate 和 delete 删除的区别
>
> truncate 删除是删除表,然后重新新建表
>
> delete 删除是删除数据,保留表结构

```mysql
truncate table 表名;
-- 例子.前提条件:把表设置为主键自动增长,然后测试添加数据后,在删除数据后的结果对比
truncate from student;
delete from student;
```

## SQL-数据查询语言DQL

> 数据库执行DQL语句不会对数据进行改变,而是让数据库跟客户端发送了一个结果集
>
> 结果集:
>
> * 通过查询语句查询出来的数据以表的形式展示,我们称这个表为虚拟结果集, 存放在内存中
>
> * 查询返回的结果集是一张虚拟表

查询所有列的数据

```mysql
select * from 表名;
-- 例子
select * from student;
```

查询指定列的数据

```mysql
select 列名1,列名2,...N from 表名;
-- 例子
select stu_name, stu_age from student;
```

### 条件查询

> 条件查询就是在查询时给出where子句, 在where字句中可以使用一些运算符和关键字

条件查询运算符和关键字

| =           | 等于                       |
| ----------- | -------------------------- |
| <>, !=      | 不等于                     |
| >           | 大于                       |
| <           | 小于                       |
| <=          | 小于等于                   |
| >=          | 大于等于                   |
| BETWEEN     | 在两值之间                 |
| NOT BETWEEN | 不在两值之间               |
| IN          | 在集合中                   |
| NOT IN      | 不在集合中                 |
| <=>         | 严格比较两个NULL值是否相等 |
| IS NULL     | 为空                       |
| IS NOT NULL | 不为空                     |

使用例子:

```mysql
-- 查询性别为男,并且年龄为20的学生
select * from student where stu_age = 20 and stu_gender = 0;

-- 查询学号为1001或者名为zs的记录
select * from student where stu_id = 1001 or stu_name = 'zs';

-- 查询学号为1001,1002,1003的记录
select * from student where stu_id = 1001 or stu_id = 1002 or stu_id =1003;
select * from student where stu_id between 1001 and 1003;
select * from student where stu_id in(1001,1002,1003);

-- 查询年龄为null的记录
select * from student where stu_age is null;

-- 查询年龄在18-20之间的学生记录
select * from student where stu_age >=18 and stu_age <=20;
select * from student where stu_age between 18 and 20;

-- 查询性别非男的学生记录
select * from student where stu_gender != 0;

-- 查询姓名不为null的学生记录
select * from student where stu_name is not null;
```

### 模糊查询

> LIKE  模糊匹配
>
> _ 代表一个字符
>
> % 代表任意1 - N个字符

```mysql
-- 模糊查询
--  查询姓名有5个字母构成的学生记录
select * from student where stu_name like '_____';
-- 查询姓名有5个字母构成,并且第5个字母为s的学生记录
select * from student where stu_name like '____s';
-- 查询姓名为m开头的学生记录
select * from student where stu_name like 'm%';
-- 查询姓名第2个字母为s的学生记录
select * from student where stu_name like '_s%';
-- 查询姓名包含k字母的学生记录
select * from student where stu_name like '%k%';
```

### 字段控制查询

> 对列中的值去重 `distinct`
>
> 对字段进行运算和条件判断 (在mysql 中 NULL值 与任何值相加 返回结果均是NULL ) 建议加上条件判断语句给出默认值 `ifnull`
>
> 给列取别名 `as`

```mysql
-- 对列中的值去重
select  distinct stu_name from student;
-- 对字段进行运算和条件判断
select stu_age, stu_score, ifnull(stu_age,1) + ifnull(stu_score,0) from student;
-- 给列取别名, as可以省略,但是建议加上
select stu_age as age, stu_score as score, ifnull(stu_age,1) + ifnull(stu_score,0) as res from student;
```

### 排序

> 对查询结果进行排序
>
> 使用关键词 `order by` 
>
> 排序类型
>
> * 升序 asc (先小后大)
> * 降序 desc (先大后小)

```mysql
-- 对所有员工的薪水进行排序
select * from employee order by salary asc;
-- 对所有的学生记录,按年龄降序排序
select * from student order by stu_age desc;
-- 查询所有雇员,按月薪降序排序,如果月薪相同,按照编号升序排序
select * from employee order by salary desc, id asc;
```

### 聚合查询

> 对查询结果进行统计计算
>
> 常用聚合函数
>
> * COUNT() : 统计指定列不为null的记录行数
> * MAX() : 计算指定列的最大值,如果指定列是字符串类型,那么使用字符串排序运算
> * MIN() : 计算指定列的最小值,如果指定列是字符串类型,那么使用字符串排序运算
> * SUM() : 计算指定列的数值和,如果指定列不是数值类型,那么计算结构为0
> * AVG() : 计算指定列的平均值,如果指定列不是数值类型,那么计算结构为0

```mysql
-- count() 函数的使用
-- 查询employee表中记录数
select count(*) from employee;
-- 查询员工表中有绩效的人数
select count(performance) from employee;
-- 查询员工表中月薪大于2500的人数
select count(*) from employee where salary > 2500;
-- 统计月薪与绩效之和大于5000元的人数
select count(*) from employee where IFNULL(salary,0) + IFNULL(performance,0) > 5000;
-- 查询有绩效的人数和有管理费的人数
select count(performance), count(manage) from employee;
```

```mysql
-- SUM AVG 函数的使用
-- 查询所有雇员月薪和
select SUM(salary) from employee;
-- 查询所有雇员月薪,以及所有雇员绩效之和
select sum(salary),sum(performance) from employee;
-- 查询所有雇员月薪加绩效之和
select sum(IFNULL(salary,0) + IFNULL(performance,0)) from employee;
-- 查询所有员工平均工资
select AVG(salary) from employee;

-- MAX MIN 函数的使用
-- 查询最高工资和最低工资
select max(salary), min(salary) from employee;
```

### 分组查询

> **什么是分组查询?**
>
> 将查询结果按照1个或多个字段进行分组,字段值相同的为一组

#### **分组的使用**

```mysql
select gender from employee group by gender;
-- 根据性别来分组所以只有男女
-- 当 group by 单独使用时,只显示每组的第一条记录
-- 所以 group by 单独使用意义不大
--  注意事项: 在使用分组时,select 后面直接跟的字段一般出现在 group by 后
```

##### **group by + group_concat的使用**

```mysql
-- group_concat(字段值)可以作为一个输出字段来使用
-- 表示分组后,根据分组结果,使用group_concat()来放置每一组的某个字段的集合
select gender,group_concat(`name`) from employee group by gender;
```

##### **聚合函数分组和使用**

> 通过gourp_concat 的启发,我们既然可以统计出每个分组的某个字段的值的集合,那么我们也可以通过集合函数来对这个"值的集合"做一些操作

```mysql
-- 聚合函数配合分组查询
-- 查询每个部门的部门名称和每个部门的工资和
select department,group_concat(salary),sum(salary) from employee group by department;
-- 查询每个部门的部门名称和每个部门的人数
select department,group_concat(name),count(*) from employee group by department;
-- 查询每个部门的部门名称以及每个部门工资大于1500的人数
select department,COUNT(*),group_concat(salary) from employee where salary > 1500 group by department;
```

##### **group by  + having和使用**

> 用来分组查询后指定一些条件来输出查询结果
>
> having和wehere一样，但having 只能用于group by

##### **where 和 having的区别**

> * having 是在分组后对数据的过滤
>
> * where 是在分组前面对数据的过滤
>
> * having 后面可以使用分组函数（统计函数）
>
> * where 后面不可以使用分组函数
>
> * where 是对分组前记录的条件，如果某行没有满足where 字句，那么这行记录不会参加分组，而having 是对分组后数据的约束。
>
> * 
>
>   
>
>   

```mysql
-- group by  + having的使用
-- 查询工资和大于9000的部门名称以及工资和
select department,GROUP_CONCAT(salary),sum(salary) from employee 
GROUP BY department 
having sum(salary) > 9000;
-- 查询工资大于2000,工资总和大于6000的部门名称以及工资和
select department,GROUP_CONCAT(salary),sum(salary) from employee 
where salary > 2000 
GROUP BY department
HAVING sum(salary) > 6000
order by salary desc;
```

##### **书写顺序**

![image-20210509110535950](images/image-20210509110535950.png)

##### **执行顺序**

![image-20210509110613883](images/image-20210509110613883.png)

##### **分页查询Limit**

> **分页查询是什么?**
>
> 分页查询是从哪一行查,总共要查几行
>
> **limit参数**
>
> 1. limit参数1 : 从哪一行开始查 下标从0开始
> 2. limit 参数2 : 一共要查几条
>
> **格式:**
>
> select * from 表名 limit 参数1, 参数2;

**分页思路**

```mysql
int curPage = 1; -- 当前页
int pageSize = 3 -- 每页多少条数据

-- 当前页第一页 所以第一页从0开始 (1-1) * 3 = 0
-- 当前页第二页 所以从第二页从3开始 (2-1) * 3 = 3
-- 当前页第三页 所以从第三页从6开始 (3-1) * 3 = 6
-- 当前页第四页 所以从第四页从9开始 (4-1) * 3 = 9

select * from employee limit (curPage - 1) * pageSize, pageSize;
```

### SQL-数据控制语言DCL



## 数据完整性

> **什么是数据完整性?**
>
> 保证用户输入的数据保存到数据库中是正确的
>
> **如何添加数据完整性?**
>
> 在创建表时给表中添加约束
>
> **完整性分类**
>
> * 实体完整性
> * 域完整性
> * 引用完整性

### 实体完整性

> **什么是实体完整性?**
>
> 表中的一行(一条记录)代表一个实体
>
> **实体完整性的作用**
>
> 标识每一行数据不重复,行级约束
>
> **约束类型**
>
> 1. 主键约束 primary key
> 2. 唯一约束 unique
> 3. 自动增长列 auto_increment

#### 主键约束 primary key

> 特点:
>
> * 每个表中要有一个主键
> * 数据唯一,且不能为null

**使用方式和格式**

```mysql
-- 第一种格式
-- create table 表名(字段名 数据类型 primary key, 字段2 数据类型,...N);
create table proson(id bigint primary key, name varchar(50));
-- 第二种格式
-- create table 表名(字段名1 数据类型, 字段名2 数据类型, ...N, primary key(主键字段名));
create table proson(id bigint, name varchar(50), age int, primary key(id));
-- 第三种格式
-- 1. 先创建表 2. 再去修改表,添加主键
-- alter table 表名 add constraint primary key(主键字段名);
create table proson(id bigint, name varchar(50));
alter table proson add constraint primary key(id);
```

**联合主键**

> 特点: 
>
> * 两个字段数据相同时,才违反联合主键
> * 两个字段值有一方为null或者两个都为null,也违反联合主键

```mysql
-- 联合主键格式
-- create table 表名(字段名1 数据类型, 字段名2 数据类型, ...N, primary key(主键字段名1, 主键字段名2, ...N));
create table proson(id bigint, stu_number bigint, name varchar(50), age int, primary key(id, stu_number));
```

#### 唯一约束 unique

> 特点:
>
> * 指定列的数据不能重复
> * 数据可以为空值

**使用方式和格式**

```mysql
-- 唯一约束格式
-- create table 表名(字段名1 数据类型, 字段2 数据类型 unique,...N);
create table preson (id bigint primary key, name varchar(50) unique);
```

#### 自动增长列 auto_increment

> 特点: 
>
> * 指定列的数据自动增长
> * 即使数据删除或者插入失败,还是从删除或者插入失败后的序号继续增长

**使用方式和格式**

```mysql
-- 自动增长列格式
-- create table 表名(字段名1 数据类型 primary key auto_increment, 字段2 数据类型 unique,...N);
create table preson (
	id bigint primary key auto_increment,
	name varchar(50) unique
);
-- 注意 自动增长列必须配合主键搭配一起使用,如果不添加主键,即会报错
```

### 域完整性

> **什么是域完整性?**
>
> 限制此单元格的数据正确,不对照此列的其他单元格比较,域代表当前单元格
>
> **域完整性约束**
>
> * 数据类型 
> * 非空约束 not null
> * 默认值约束 default

**使用方法和格式**

```mysql
-- 域完整性约束格式
create table 表名(
	字段名1 数据类型 primary key auto_increment, 
	字段名2 数据类型 unique not null, 
	字段名3 数据类型 default '默认值'
);

create table person(
	id int primary key auto_increment,
	name varchar(20) unique not null,
	genger char(1) default '男'
);
```

### 引用完整性

> **什么是参照完整性?**
>
> 是指表与表之间的一种对应关系
>
> 通常情况下可以通过设置两表之间的主键,外键关系,或者编写两表的触发器来实现,通常设置表与表之间的关系后,对他们进行数据插入 更新 删除的过程中,系统都会参照另一张表进行对照,从而阻止一些不正确的数据操作
>
> **参照完整性的规则**
>
> 数据库的主键和外键类型一定要一致
>
> 两张表必须要是innoDB类型
>
> 设置参照完整性后, 外键中的内值,必须得是另一张表主键当中的内容

**使用方法和格式**

```mysql
-- 引用完整性的使用方法和格式
-- 注意: 一旦设置外键了,子表中外键的值只能从主键选择
-- 并且要删除两张表的话,需要先删除子表,在删除主表,否则删除不了主表
-- 1. 创建一张设置主键的表,该设置主键的表为主表
create table 主表名(字段名1 数据类型 primary key,...N);
-- 2. 创建表,设置外键,设置外键的为子表
create table 子表名(
	字段名1 数据类型 primary key,
	...N,
	外键字段名 数据类型,
	constraint 外键名 foreign key (外键字段名) references 主表(主表主键名)
)
-- 例子
create table stu(id int primary key, name varchar(50));

create table score(
	id int primary key, 
	score double(4,2),
	km varchar(50),
	s_id int,
	constraint sc_stu_fk foreign key  (s_id) REFERENCES stu(id)
);
-- 例子二就是添加外键或主键
alter table score add constraint sc_stu_id foreign key(s_id) references stu(id);
```

## 多表查询



## 子查询



## 常用函数



## 事务



## 权限管理



## 视图



## 存储过程



## 索引

