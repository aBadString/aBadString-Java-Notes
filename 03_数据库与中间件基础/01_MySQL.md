<h1 id="MySQL" align="center">MySQL</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. 关系型数据库基础](#1-关系型数据库基础)
  - [1.1. 函数依赖与数据库范式](#11-函数依赖与数据库范式)
- [2. MySQL 必知必会](#2-mysql-必知必会)
  - [2.1. MySQL 基础命令](#21-mysql-基础命令)
  - [2.2. 建表语句](#22-建表语句)
    - [2.2.1. 表字段数据类型](#221-表字段数据类型)
  - [2.3. 增改删语句](#23-增改删语句)
    - [2.3.1. insert](#231-insert)
    - [2.3.2. update](#232-update)
    - [2.3.3. delete](#233-delete)
  - [2.4. select 检索数据](#24-select-检索数据)
    - [2.4.1. limit：限制返回前几行数据](#241-limit限制返回前几行数据)
    - [2.4.2. order by 子句](#242-order-by-子句)
  - [2.5. where 过滤数据](#25-where-过滤数据)
  - [2.6. 函数](#26-函数)
  - [2.7. 聚集函数](#27-聚集函数)
  - [2.8. group by 分组数据](#28-group-by-分组数据)
    - [2.8.1. having 过滤分组](#281-having-过滤分组)
  - [2.9. join 表连接](#29-join-表连接)
    - [2.9.1. 自连接](#291-自连接)
    - [2.9.2. 自然连接](#292-自然连接)
    - [2.9.3. inner 内连接](#293-inner-内连接)
    - [2.9.4. outer 外连接](#294-outer-外连接)
  - [2.10. SQL 执行顺序](#210-sql-执行顺序)
    - [2.10.1. jion 子句](#2101-jion-子句)
      - [2.10.1.1. join 连接表，作笛卡尔积](#21011-join-连接表作笛卡尔积)
      - [2.10.1.2. on 筛选条件](#21012-on-筛选条件)
      - [2.10.1.3. outer 外连接](#21013-outer-外连接)
    - [2.10.2. where 子句](#2102-where-子句)
    - [2.10.3. group by 子句](#2103-group-by-子句)
      - [2.10.3.1. having](#21031-having)
    - [2.10.4. select 选择列](#2104-select-选择列)
      - [2.10.4.1. 计算 select 选择列的表达式](#21041-计算-select-选择列的表达式)
      - [2.10.4.2. distinct 执行去重](#21042-distinct-执行去重)
    - [2.10.5. order by 排序](#2105-order-by-排序)
    - [2.10.6. limit 子句](#2106-limit-子句)
  - [2.11. 触发器](#211-触发器)
- [3. MySQL 体系结构](#3-mysql-体系结构)
  - [3.1. MyISAM 与 InnoDB](#31-myisam-与-innodb)
  - [3.2. Innodb存储引擎缓冲池](#32-innodb存储引擎缓冲池)
- [4. 事务](#4-事务)
  - [4.1. 四大特性](#41-四大特性)
  - [4.2. 事务隔离级别](#42-事务隔离级别)
  - [4.3. 并发一致性问题](#43-并发一致性问题)
  - [4.4. 其他](#44-其他)
  - [4.5. MVCC 多版本并发控制](#45-mvcc-多版本并发控制)
- [5. 锁](#5-锁)
  - [5.1. 锁的分类](#51-锁的分类)
  - [5.2. 封锁协议](#52-封锁协议)
    - [5.2.1. 三级封锁协议](#521-三级封锁协议)
    - [5.2.2. 两段锁协议](#522-两段锁协议)
  - [5.3. 行锁的三种算法](#53-行锁的三种算法)
    - [5.3.1. Record Lock](#531-record-lock)
    - [5.3.2. Gap Lock](#532-gap-lock)
    - [5.3.3. Next-Key Lock](#533-next-key-lock)
  - [5.4. 阻塞](#54-阻塞)
  - [5.5. 死锁](#55-死锁)
  - [5.6. 锁升级](#56-锁升级)
- [6. 索引](#6-索引)
  - [6.1. 索引优点与使用场景](#61-索引优点与使用场景)
  - [6.2. MySQL 索引种类](#62-mysql-索引种类)
    - [6.2.1. B+ 树索引](#621-b-树索引)
      - [6.2.1.1. 聚集索引](#6211-聚集索引)
      - [6.2.1.2. 辅助索引 (非聚集索引)](#6212-辅助索引-非聚集索引)
      - [6.2.1.3. 联合索引](#6213-联合索引)
      - [6.2.1.4. 覆盖索引](#6214-覆盖索引)
    - [6.2.2. 全文索引](#622-全文索引)
    - [6.2.3. 哈希索引](#623-哈希索引)
    - [6.2.4. 空间数据索引(R-Tree)](#624-空间数据索引r-tree)
  - [6.3. 索引优化](#63-索引优化)
    - [6.3.1. 单列索引](#631-单列索引)
    - [6.3.2. 多列索引](#632-多列索引)
    - [6.3.3. 索引列的顺序](#633-索引列的顺序)
    - [6.3.4. 建立索引的常用技巧](#634-建立索引的常用技巧httpsjuejinimpost5a6873fbf265da3e393a97fa)
- [7. explain](#7-explain)
- [8. 附录：常用SQL语法](#8-附录常用sql语法)
- [9. 附录：MySQL 安装](#9-附录mysql-安装)
  - [9.1. 常见问题](#91-常见问题)

<!-- /code_chunk_output -->


# 1. 关系型数据库基础

**数据库**是保存有组织的数据的容器（通常是一个或者一组文件）。
**表**是某种特定类型数据的结构化清单。
**模式**是关于数据库和表的布局及特性的信息。
**列**是表中的一个字段。所有的表都是由一个或多个列组成的。
**数据类型**：所容许的数据的类型。每个表的每一列都有相应的数据类型。
**行**是表中的一个记录。
**主键**是一个或者一组列，它能够唯一地区分表中的每一行。（任意两行不能具有相同的主键值；每一行都要有一个主键值。）

**SQL 的优点：**
1. 几乎所有重要的 DBMS 都支持 SQL；
2. SQL 简单易学；
3. SQL 是强有力的语言，灵活使用可以进行非常复杂和高级的数据库操作。

**MySQL 的优点：**
1. 开源免费
2. 性能好，执行非常快
3. 可信赖
4. 简单，安装和使用都很容易

**数据完整性**
- 实体完整性：实体完整性在MySQL中的实现是通过主键约束和候选键约束实现的。
- 参照完整性：外键
- 约束完整性：列的约束

## 1.1. 函数依赖与数据库范式

**候选码：** 若关系中的某一属性组的值能唯一地标识一个元组，而其子集不能，则称该属性组为候选码。
**主码：**若一个关系中有多个候选码，则选定其中一个为主码。
**主属性：** 所有候选码的属性称为主属性。
**非主属性：**不包含在任何候选码中的属性称为非主属性。

**函数依赖**：对于两个集合 A, B，如果一个已经确定值的 A 可以唯一的确定 B 的值，则称 A 函数决定 B，B 函数依赖 A。记作 `A -> B`。
**部分函数依赖**：如果 `A -> B, X -> B` 且 $X \subsetneqq A$，则称 B 部分函数依赖于 A。
**传递函数依赖**：如果 `A -> X, X -> B` 且 `X !-> A`，则称 B 传递函数依赖于 A。

**数据库六大范式：**
- 第一范式：每个列都不可以再拆分。
- 第二范式：在第一范式的基础上，非主属性必须完全依赖于候选码（消除非主属性对主属性的部分依赖）
- 第三范式：在第二范式的基础上，任何非主属性不依赖于其它非主属性（消除非主属性对主属性的传递依赖）
- BC 范式：在第三范式的基础上，任何主属性不能对候选码子集依赖（消除主属性对于候选码的部分与传递函数依赖）
- 第四范式：在BC范式的基础上，限制关系模式的属性之间不允许有非平凡且非函数依赖的多值依赖。
- 第五范式：在第四范式的基础上，表必须可以分解为较小的表，除非那些表在逻辑上拥有与原始表相同的主键。

```text
学生表（学号，姓名，年龄，班主任，班主任办公室）
满足 2NF，不满足 3NF： 学号 -> 班主任 -> 班主任办公室 

修改：
学生表（学号，姓名，年龄，班主任)
班主任表 (班主任，班主任办公室)
```
|学号|姓名|年龄|班主任|班主任办公室|
|:-:|:-:|:-:|:-:|:-:|
|1|学生1|19|班主任1|办公室1|
|2|学生2|19|班主任1|办公室1|
|3|学生3|19|班主任1|办公室1|
|4|学生4|19|班主任2|办公室2|
|5|学生5|19|班主任2|办公室2|
|6|学生6|19|班主任3|办公室3|

不符合范式的关系，会产生很多异常，主要有以下四种异常：

- 冗余数据：班主任信息冗余。
- 修改异常：修改了一个记录中的信息，但是另一个记录中相同的信息却没有被修改。
- 删除异常：删除一个信息，那么也会丢失其它信息。例如删除了学生6，则对应的班主任3的信息也没有了。
插入异常：例如想要插入一个办公室的信息，如果它还没有班主任入住就无法插入。


# 2. MySQL 必知必会

## 2.1. MySQL 基础命令

登录 mysql：
```shell
mysql -u root -p
```
MySQL的连接和关闭：mysql -u -p -h -P
> -u：指定用户名    -p：指定密码    -h：主机    -P：端口

进入MySQL命令行后：G、c、q、s、h、d
> G：打印结果垂直显示
> c：取消当前MySQL命令
> q：退出MySQL连接
> s：显示服务器状态
> h：帮助信息
> d：改变执行符

查看数据库：
```sql
show databases;
```

使用某个数据库
```sql
use creshcourse;
```

查看数据库中的表
```sql
show tables;
```

查看某张表的列
```sql
show columns from customers;
desc member;
-- 这两命令是一样的效果
```

![image-20200331150351139](../images/image-20200331150351139.png)

## 2.2. 建表语句

```sql
-- 创建表
CREATE TABLE mytable (
  --  int 类型，不为空，自增
  id INT NOT NULL AUTO_INCREMENT,
  --  int 类型，不可为空，默认值为 1，不为空
  col1 INT NOT NULL DEFAULT 1,
  --  变长字符串类型，最长为 45 个字符，可以为空
  col2 VARCHAR(45) NULL,
  --  日期类型，可为空
  col3 DATE NULL,
  --  设置主键为 id
  PRIMARY KEY (`id`)
);

-- 添加列
ALTER TABLE mytable
ADD col CHAR(20);

-- 删除列
ALTER TABLE mytable
DROP COLUMN col;

-- 删除表
DROP TABLE mytable;
```

### 2.2.1. 表字段数据类型

**整型**
tinyint smallint mediumint int bigint
1       2        3         4   8 字节

**int(20)中20的含义**
是指显示字符的长度，不影响内部存储，只是当定义了ZEROFILL时，前面补多少个 0


**浮点型**
float double decimal

float 和 double 为浮点类型，decimal 为高精度小数类型。
CPU 原生支持浮点运算，但是不支持 decimal 类型的计算，因此 decimal 的计算比浮点类型需要更高的代价。
float, double 和 decimal 都可以指定列宽，例如 decimal(18, 9) 表示总共 18 位，取 9 位存储小数部分，剩下 9 位存储整数部分

**FLOAT和DOUBLE的区别是什么？**
- FLOAT类型数据可以存储至多8位十进制数，并在内存中占4字节。
- DOUBLE类型数据可以存储至多18位十进制数，并在内存中占8字节。


**字符串**
varchar char text blobvarchar

在进行存储和检索时，会保留 VARCHAR 末尾的空格，而会删除 CHAR 末尾的空格。

**varchar(50)中50的含义**
最多存放50个字符，varchar(50)和(200)存储hello所占空间一样，但后者在排序时会消耗更多内存，因为order by col采用fixed_length计算col长度(memory引擎也一样)。


**时间和日期**
datatime timestamp

datatime 能够保存从 1000 年到 9999 年的日期和时间，精度为秒，使用 8 字节的存储空间，它与时区无关。
timestamp 保存UNIX 时间戳，使用 4 个字节，它和时区有关，也就是说一个时间戳在不同的时区所代表的具体时间是不同的。

默认情况下，如果插入时没有指定 TIMESTAMP 列的值，会将这个值设置为当前时间。
应该尽量使用 TIMESTAMP，因为它比 DATETIME 空间效率更高。


## 2.3. 增改删语句

### 2.3.1. insert

```sql
INSERT INTO mytable(col1, col2)
  VALUES(val1, val2);

-- 插入检索出来的数据
INSERT INTO mytable1(col1, col2)
  SELECT col1, col2
  FROM mytable2;

-- 将一个表的内容插入到一个新表
-- 这个只会复制列和数据，不会复制索引和key
CREATE TABLE newtable AS
  SELECT * FROM mytable;
```

### 2.3.2. update

```sql
UPDATE mytable
  SET col1 = val1, col2 = val2
  WHERE id = 1;
```

### 2.3.3. delete

```sql
DELETE FROM mytable
  WHERE id = 1;
```
TRUNCATE TABLE 可以清空表，也就是删除所有行。
`TRUNCATE TABLE mytable;`

truncate 删除后 向表添加数据，id 标识列从1开始了。(体现了truncate删除是释放空间）
delete 删除后，然后添加，可以看到添加之后id标识接上一个id值之后。（说明delete删除不释放空间）

delete语句是DML语言,这个操作会放在rollback segement中,事物提交后才生效;如果有相应的触发器(trigger),执行的时候将被触发。
truncate、drop是DDL语言,操作后即 生效,原数据不会放到rollback中,不能回滚,操作不会触发trigger。

## 2.4. select 检索数据

**select：查询表中的一些列**
```sql
select user_name from t_user;
select user_name, user_sex from t_user;
select * from t_user;
```

**distinct：去除重复数据**
```sql
select distinct user_name from t_user;
```

**完全限定的表名**
```sql
select user_name from mybatis.t_user;
```
当没有执行 use mybatis时，可以使用完全限定的表名。

### 2.4.1. limit：限制返回前几行数据
```sql
-- 返回前 5 行数据
select user_name from t_user limit 5;
select user_name from t_user limit 0, 5;
-- 从行号 2 开始后面 5 行数据 （行数是 从 0 开始数的）
select user_name from t_user limit 2, 5;
```
当行数不够时，只会返回已有的那么多行；当行号越界时，返回 零行数据

### 2.4.2. order by 子句
```sql
select user_id, user_name from t_user order by user_name;
select * from t_user order by user_name, user_email;
```
order by 子句取一个或多个列来对输出进行排序。
```sql
select * from t_user order by user_id asc;
select * from t_user order by user_id desc;
select * from t_user order by user_name desc, user_id;
select * from t_user order by user_name, user_id desc;
select * from t_user order by user_name desc, user_id desc;
```
desc：降序；asc：升序，默认
desc 只作用于其前面的一列。

【寻找一列中最高或最低的值】
```sql
select * from t_user order by user_id desc limit 0, 1;
```


## 2.5. where 过滤数据

order by 子句应该位于 where 子句之后。

```sql
=             -- 等于
<>  !=        -- 不等于
<  <=  >  >=  -- 小于 小于等于 大于 大于等于
between and   -- 在指定两个值之间
is null  is not null

and -- and 优先级高于 or
or
in
not
-- 可以使用圆括号来改变 where 子句中语句优先级。
```

**in**
```sql
select name from t_user
    where age in (20, 21, 22);
```

**like**
通配符：
- %, 任何字符出现任何次数
- _, 任意单个字符
通配符最好不要用在搜索模式的左边，因为这样不能使用索引。

**regexp**
正则表达式匹配

看看下面 四条SQL 的输出
```sql
select username from user where username like 'aBadString';
-- aBadString
select username from user where username regexp 'aBadString';
-- aBadString
select username from user where username like 'BadStrin';
-- （无数据）匹配不到 BadStrin，like是匹配整个字符串
select username from user where username regexp 'BadStrin';
-- aBadString 正则表达式是匹配子串

select username from user where username like '%BadStrin%';
-- aBadString
select username from user where username regexp '^BadStrin$';
-- （无数据）^表示字符串开始，$表示字符串结束
```

## 2.6. 函数

**字符串函数**
Concat() 字符串拼接
LTrim() 去除字符串左侧多余空格
RTrim() 去除字符串右侧多余空格
Trim() 去除字符串左右两侧多余空格
Upper() 将字符串转为大写
Lower() 将字符串转为小写
Length() 计算字符串长度
SOUNDEX() 将文本串转换为描述其语音表示的字母数字模式的算法。（查询因为相同发音而写错的单词）

**日期函数**
CurDate() 返回当前日期
CurTime() 返回当前时间
Now() 返回当前日期和时间
Time() 返回一个日期时间的时间部分
Date() 返回日期时间的日期部分
```sql
select Now(), CurDate(), CurTime(), Date(Now()), Time(Now());
-- 2020-09-10 18:05:30,    2020-09-10,    18:05:30,    2020-09-10,    18:05:30
```

AddDate() 增加一个日期（天、周等）
AddTime() 增加一个时间（时、分等）
DateDiff() 计算两个日期之差
Date_Add() 高度灵活的日期运算函数
Date_Format() 返回一个格式化的日期或时间串

second() 返回一个时间的秒部分
Minute() 返回一个时间的分钟部分
Hour() 返回一个时间的小时部分
DayOfWeek() 对于一个日期，返回对应的星期几
Day() 返回一个日期的天数部分
Month() 返回一个日期的月份部分
Year() 返回一个日期的年份部分

**数值函数**
Abs() 返回一个数的绝对值
Cos() 返回一个角度的余弦
Exp() 返回一个数的指数值
Mod() 返回除操作的余数
Pi() 返回圆周率
Rand() 返回一个随机数
Sin() 返回一个角度的正弦
Sqrt() 返回一个数的平方根
Tan() 返回一个角度的正切


## 2.7. 聚集函数

COUNT() 返回某列的行数（COUNT(*)返回数据行数，COUNT(列名)会忽略值为 NULL 的行）
AVG() 返回某列的平均值（忽略值为 NULL 的行）
MAX() 返回某列的最大值（忽略值为 NULL 的行）
MIN() 返回某列的最小值（忽略值为 NULL 的行）
SUM() 返回某列值之和（忽略值为 NULL 的行）

```sql
--  计算填写了手机号的用户人数
select count(mobile) from account;
```


## 2.8. group by 分组数据

- group by 必须在 where 子句之后，order by 子句之前
- group by 的每个列都必须是检索列或者有效表达式，不能是聚集函数
- select 语句中的列只能是出现在 group by 子句中的列或者是聚集函数；如果 select使用表达式作为列，则 group by 子句必须出现相同的表达式，不能使用别名
- 分组列中的 NULL 值会被分为一组

### 2.8.1. having 过滤分组

having 支持所有 where 的操作。
having 用于过滤分组，可以使用聚集函数，而 where 过滤行，不能使用聚集函数。
where 在分组前过滤数据，having 在分组后过滤数据。

不要忘记 ORDER BY 一般在使用 GROUP BY 子句时，应该也给出 ORDER BY 子句。这是保证数据正确排序的唯一方法。千万不要仅依赖 GROUP BY 排序数据。


## 2.9. join 表连接

**外键**(foreign key)为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。

```sql
select *
from `member` m join `order` o
    on m.id=o.member_id;

select *
from `member` m join `order` o
    where m.id=o.member_id;
```

### 2.9.1. 自连接

|id| member_id| status| create_time|
|:-:|:-:|:-:|:-:|
|1|1|3|2020-09-11 00:00:00|
|2|2|1|2020-09-12 00:00:00|
|3|3|4|2020-09-09 00:00:00|
|4|4|0|2020-09-13 00:00:00|
|5|5|1|2020-09-11 00:00:00|
|6|6|2|2020-09-11 00:00:00|
|7| |0|2020-09-07 00:00:00|
查询和 id=7 属于同一个 status 的所有字段：
```sql
-- 方法一：子查询
select *
from `order`
where status in (
    select status
    from `order`
    where id = 7
);

-- 方法二：自连接
select o1.*
from `order` o1 join `order` o2 
    on o1.status = o2.status
where o2.id = 7;
```
|id| member_id| status| create_time|
|:-:|:-:|:-:|:-:|
|7| |0|2020-09-07 00:00:00|
|4|4|0|2020-09-13 00:00:00|

### 2.9.2. 自然连接

### 2.9.3. inner 内连接

### 2.9.4. outer 外连接

左外连接补全左表全部数据，补充的数据的右边使用 null 填充；
右外连接补全右表全部数据，补充的数据的左边使用 null 填充。


## 2.10. SQL 执行顺序

```sql
create table `member`
(
   `id` int auto_increment,
   `name` nvarchar(30) null,
   `phone` varchar(15) null,
   constraint member_pk
      primary key (id)
);
create table `order`
(
   `id` int auto_increment,
   `member_id` int,
   `status` int null,
   `create_time` datetime null,
   constraint order_pk
      primary key (id)
);
insert into `member`(id, name, phone) value (1, '张龙豪', '18501733702');
insert into `member`(id, name, phone) value (2, 'Jim', '15039512688');
insert into `member`(id, name, phone) value (3, 'Tom', '15139512854');
insert into `member`(id, name, phone) value (4, 'Lulu', '15687425583');
insert into `member`(id, name, phone) value (5, 'Jick', '13528567445');
insert into `order`(id, member_id, status, create_time) VALUE (1, 1, 3, '2020-9-11');
insert into `order`(id, member_id, status, create_time) VALUE (2, 2, 1, '2020-9-12');
insert into `order`(id, member_id, status, create_time) VALUE (3, 3, 4, '2020-9-9');
insert into `order`(id, member_id, status, create_time) VALUE (4, 4, 0, '2020-9-13');
insert into `order`(id, member_id, status, create_time) VALUE (5, 5, 1, '2020-9-11');
insert into `order`(id, member_id, status, create_time) VALUE (6, 6, 2, '2020-9-11');
insert into `order`(id, member_id, status, create_time) VALUE (7, NULL, 0, '2020-9-7');
```

先看看两张表的数据
```sql
select * from `member`;
select * from `order`;
```

|id|name|phone|
|:-:|:-:|:-:|
|1|张龙豪|18501733702|
|2|Jim|15039512688|
|3|Tom|15139512854|
|4|Lulu|15687425583|
|5|Jick|13528567445|

|id| member_id| status| create_time|
|:-:|:-:|:-:|:-:|
|1|1|3|2020-09-11 00:00:00|
|2|2|1|2020-09-12 00:00:00|
|3|3|4|2020-09-09 00:00:00|
|4|4|0|2020-09-13 00:00:00|
|5|5|1|2020-09-11 00:00:00|
|6|6|2|2020-09-11 00:00:00|
|7|null|0|2020-09-07 00:00:00|

看看这个 SQL 的执行
```sql
select status, max(m.id) as max_member_id
from `member` m right outer join `order` o
    on m.id=o.member_id
where m.id > 0
group by status
    having status>=0
order by max_member_id asc
limit 0, 4
```

### 2.10.1. jion 子句

#### 2.10.1.1. join 连接表，作笛卡尔积
```sql
select *
from `member` m join `order` o;
-- 还有
select *
from `order` o join  `member` m;
select *
from `member` m, `order` o;
select *
from `order` o, `member` m;
```

|m.id|name|phone|o.old|member_id|status|create_time|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|==1==|张龙豪|18501733702|1|==1==|3|2020-09-11 00:00:00|
|2|Jim|15039512688|1|1|3|2020-09-11 00:00:00|
|3|Tom|15139512854|1|1|3|2020-09-11 00:00:00|
|4|Lulu|15687425583|1|1|3|2020-09-11 00:00:00|
|5|Jick|13528567445|1|1|3|2020-09-11 00:00:00|
|1|张龙豪|18501733702|2|2|1|2020-09-12 00:00:00|
|==2==|Jim|15039512688|2|==2==|1|2020-09-12 00:00:00|
|3|Tom|15139512854|2|2|1|2020-09-12 00:00:00|
|4|Lulu|15687425583|2|2|1|2020-09-12 00:00:00|
|5|Jick|13528567445|2|2|1|2020-09-12 00:00:00|
|1|张龙豪|18501733702|3|3|4|2020-09-09 00:00:00|
|2|Jim|15039512688|3|3|4|2020-09-09 00:00:00|
|==3==|Tom|15139512854|3|==3==|4|2020-09-09 00:00:00|
|4|Lulu|15687425583|3|3|4|2020-09-09 00:00:00|
|5|Jick|13528567445|3|3|4|2020-09-09 00:00:00|
|1|张龙豪|18501733702|4|4|0|2020-09-13 00:00:00|
|2|Jim|15039512688|4|4|0|2020-09-13 00:00:00|
|3|Tom|15139512854|4|4|0|2020-09-13 00:00:00|
|==4==|Lulu|15687425583|4|==4==|0|2020-09-13 00:00:00|
|5|Jick|13528567445|4|4|0|2020-09-13 00:00:00|
|1|张龙豪|18501733702|5|5|1|2020-09-11 00:00:00|
|2|Jim|15039512688|5|5|1|2020-09-11 00:00:00|
|3|Tom|15139512854|5|5|1|2020-09-11 00:00:00|
|4|Lulu|15687425583|5|5|1|2020-09-11 00:00:00|
|==5==|Jick|13528567445|5|==5==|1|2020-09-11 00:00:00|
|1|张龙豪|18501733702|6|6|2|2020-09-11 00:00:00|
|2|Jim|15039512688|6|6|2|2020-09-11 00:00:00|
|3|Tom|15139512854|6|6|2|2020-09-11 00:00:00|
|4|Lulu|15687425583|6|6|2|2020-09-11 00:00:00|
|5|Jick|13528567445|6|6|2|2020-09-11 00:00:00|
|1|张龙豪|18501733702|7||0|2020-09-07 00:00:00|
|2|Jim|15039512688|7||0|2020-09-07 00:00:00|
|3|Tom|15139512854|7||0|2020-09-07 00:00:00|
|4|Lulu|15687425583|7||0|2020-09-07 00:00:00|
|5|Jick|13528567445|7||0|2020-09-07 00:00:00|

#### 2.10.1.2. on 筛选条件
```sql
select *
from `member` m join `order` o
    on m.id=o.member_id;
```
先执行 on 条件筛选数据，在把右表 order 的数据加上来

|m.id|name|phone|o.old|member_id|status|create_time|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|1|张龙豪|18501733702|1|1|3|2020-09-11 00:00:00|
|2|Jim|15039512688|2|2|1|2020-09-12 00:00:00|
|3|Tom|15139512854|3|3|4|2020-09-09 00:00:00|
|4|Lulu|15687425583|4|4|0|2020-09-13 00:00:00|
|5|Jick|13528567445|5|5|1|2020-09-11 00:00:00|

#### 2.10.1.3. outer 外连接

左外连接补全左表全部数据，补充的数据的右边使用 null 填充；
右外连接补全右表全部数据，补充的数据的左边使用 null 填充。
```sql
select *
from `member` m right outer join `order` o
    on m.id=o.member_id

select *
from `order` o left outer join `member` m
    on m.id=o.member_id
```

|m.id|name|phone|o.old|member_id|status|create_time|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|1|张龙豪|18501733702|1|1|3|2020-09-11 00:00:00|
|2|Jim|15039512688|2|2|1|2020-09-12 00:00:00|
|3|Tom|15139512854|3|3|4|2020-09-09 00:00:00|
|4|Lulu|15687425583|4|4|0|2020-09-13 00:00:00|
|5|Jick|13528567445|5|5|1|2020-09-11 00:00:00|
|==null==|==null==|==null==|6|6|2|2020-09-11 00:00:00|
|==null==|==null==|==null==|7|null|0|2020-09-07 00:00:00|

### 2.10.2. where 子句

注意：where 的删除是永久的；on 的删除是可以在外连接时加入进来的。
```sql
select *
from `member` m right outer join `order` o
    on m.id=o.member_id
where m.id > 0;
```
m.id > 0 会删掉 m.id 小于等于 0 以及 m.id 为 NULL 的 行

|m.id|name|phone|o.old|member_id|status|create_time|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|1|张龙豪|18501733702|1|1|3|2020-09-11 00:00:00|
|2|Jim|15039512688|2|2|1|2020-09-12 00:00:00|
|3|Tom|15139512854|3|3|4|2020-09-09 00:00:00|
|4|Lulu|15687425583|4|4|0|2020-09-13 00:00:00|
|5|Jick|13528567445|5|5|1|2020-09-11 00:00:00|

### 2.10.3. group by 子句

```sql
select status, count(*)
from `member` m right outer join `order` o
    on m.id=o.member_id
where m.id > 0
group by status;
```
1、执行完 where 后，执行 group by，将数据分为下面 4 组
|group|m.id|name|phone|o.old|member_id|status|create_time|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
||||||||||
|A|1|张龙豪|18501733702|1|1|3|2020-09-11 00:00:00|
|||||||||
|B|2|Jim|15039512688|2|2|1|2020-09-12 00:00:00|
|B|5|Jick|13528567445|5|5|1|2020-09-11 00:00:00|
|||||||||
|C|3|Tom|15139512854|3|3|4|2020-09-09 00:00:00|
|||||||||
|D|4|Lulu|15687425583|4|4|0|2020-09-13 00:00:00|

2、再执行 select 选择列，count(*) 分别计算每组的数据行数
|status|count(*)|
|:-:|:-:|
|3|1|
|1|2|
|4|1|
|0|1|

#### 2.10.3.1. having

having count(*) <= 1; 保留数据行数小于等于 1 的组
```sql
select status, count(*)
from `member` m right outer join `order` o
    on m.id=o.member_id
where m.id > 0
group by status
    having count(*) <= 1;
```

执行完 having 后，再执行 select 语句
|status|count(*)|
|:-:|:-:|
|3|1|
|4|1|
|0|1|

### 2.10.4. select 选择列

#### 2.10.4.1. 计算 select 选择列的表达式

#### 2.10.4.2. distinct 执行去重


### 2.10.5. order by 排序

asc 升序，默认
desc 降序

```sql
select status, count(*)
from `member` m right outer join `order` o
    on m.id=o.member_id
where m.id > 0
group by status
    having count(*) <= 1
order by status;
```
|status|count(*)|
|:-:|:-:|
|0|1|
|3|1|
|4|1|


### 2.10.6. limit 子句

```sql
select status, count(*)
from `member` m right outer join `order` o
    on m.id=o.member_id
where m.id > 0
group by status
    having count(*) <= 1
order by status
limit 1,2;
```
从行号 2 的行（包括）开始取 2 行数据。（行号从 0 开始）
|status|count(*)|
|:-:|:-:|
|3|1|
|4|1|

## 2.11. 触发器

在MySQL数据库中有如下六种触发器：
- 1、Before Insert
- 2、After Insert
- 3、Before Update
- 4、After Update
- 5、Before Delete
- 6、After Delete

# 3. MySQL 体系结构

数据库 database：物理操作系统文件或其他形式文件类型的集合。
实例 instance：MySQL 数据库由后台线程以及一个共享内存区组成。
通常情况下，数据库与实例一一对应。集群的情况下，存在一个数据库对应多个实例。
MySQL 是一个单进程多线程架构的数据库。MySQL 数据库实例在系统上的表现就是一个进程。

MySQL 的组成：
- 连接池组件
- 管理服务和工具组件
- SQL 接口组件
- 查询分析器组件
- 优化器组件
- 缓冲组件 Cache
- **插件式存储引擎**
- 物理文件

存储引擎是基于表的，而不是数据库。

![image-20200328145525223](../images/image-20200328145525223.png)


**日志类型：**

1. 二进制日志
   存储数据库中表修改的全部动作，可以用于数据库的主从复制

2. 调试日志
   调试模式下生成的日志

3. 错误日志
   当mysqld启动和停止时，以及服务器在运行过程中发生任何严重错误时的相关信息

4. 查询日志
   记录所有的查询操作

5. 慢查询日志
   记录超时的查询操作，指定时间由 long_query_time 设置

6. 更新日志
   更新日志提供查询信息，但只有修改数据库内容的查询


## 3.1. MyISAM 与 InnoDB

**1、InnoDB存储引擎**
- 默认事务型引擎，最重要最广泛的存储引擎，性能非常优秀。
- 数据存储在共享表空间，可以通过配置分开。也就是多个表和索引都存储在一个表空间中，可以通过配置文件改变此配置。
- 对主键查询的性能高于其他类型的存储引擎。
- 内部做了很多优化，从磁盘读取数据时会自动构建hash索引，插入数据时自动构建插入缓冲区。
- 通过一些机制和工具支持真正的热备份。
- 支持崩溃后的安全恢复。
- 支持行级锁。
- 支持外键。
- 支持事务。

**2、MyISAM存储引擎**
- 拥有全文索引、压缩、空间函数。
- 不支持事务和行级锁、不支持崩溃后的安全恢复。
- 表存储在两个文件，MYD和MYI。
- 设计简单，某些场景下性能很好，例如获取整个表有多少条数据，性能很高。
- 全文索引不是很常用，不如使用外部的ElasticSearch或Lucene。


**请说明InnoDB和MyISAM的区别**
- InnoDB支持事务，MyISAM不支持；
- InnoDB支持外键，MyISAM不支持；
- InnoDB支持行级锁，MyISAM只支持表锁；

- InnoDB不支持全文索引，MyISAM支持全文索引；
- InnoDB表必须有主键（用户没有指定的话会自己找或生产一个主键），MyISAM可以没有
- InnoDB不保存表的具体行数，执行select count(*) from table时需要全表扫描。而MyISAM用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快（注意不能加有任何WHERE条件）

- InnoDB支持崩溃后的恢复，MyISAM不支持；
- InnoDB数据存储在共享表空间，MyISAM数据存储在文件中；
- Innodb存储文件有frm、ibd，而MyISAM是frm、MYD、MYI
  Innodb：frm是表定义文件，ibd是数据文件
  Myisam：frm是表定义文件，myd是数据文件，myi是索引文件


**innodb引擎的特性**
- 插入缓冲（insert buffer)
- 二次写(double write)
- 自适应哈希索引(ahi)
- 预读(read ahead)

## 3.2. Innodb存储引擎缓冲池

在 Innodb 中需要用到的数据页都是从缓冲池中分配出来的。
**AWE**：地址窗口化拓展。
**Frame**：帧。16K的虚拟地址空间， 在缓冲池的管理上，整个缓冲区是以大小为16k的 frame 为单位来进行的，frame是innodb中页的大小。
**Page**: 页。16K的物理内存空间， page上存的是需要保存到磁盘上的数据， 这些数据可能是数据记录信息， 也可以是索引信息或其他的元数据等


内存缓冲池：
首先将从磁盘读取到的页放在缓冲池中，这个过程叫做：将页 fix 在缓冲池中。下一次访问时，如果数据在缓冲池中，则该页被命中；若不在，则从磁盘读取页。
修改操作时，先修改缓冲池中的页，然后在定时地更新到磁盘。

缓冲池 中数据页的类型有：
索引页、数据页、undo 页、插入缓冲、自适应哈希索引、InnoDB存储的锁信息、数据字典信息。

MySQL自增的最大值为：2147483647。超出后会报错。


# 4. 事务

事务是区别数据库和文件系统的重要特性之一。
事务会把数据库从一种一致状态转换为另一种一致状态。在数据库提交工作时，可以确保要么所有修改都已经保存了，要么所有修改都不保存。

## 4.1. 四大特性

InnoDB 存储引擎中的事务符合：
- 原子性：整个事务是不可分割的工作单位。要么都完成，要么都不完成。
- 一致性：事务将数据库从一种一致状态转变为另一种一致状态。（在一致性状态下，所有事务对同一个数据的读取结果都是相同的）
- 隔离性(并发控制、可串行化、锁)：一个事务提交之前对其他事务是不可见的。
- 持久性：事务一旦提交，其结果是永久性的。持久性保证事务系统的高可靠性。

事务是由一条或者一组 SQL 语句组成的，事务是访问并更新数据库中各种数据项的一个程序执行单元。

**ACID 靠什么保证的呢？**
原子性：由undo log日志保证，它记录了需要回滚的日志信息，事务回滚时撤销已经执行成功的sql。
一致性：一般由代码层面来保证。另一说法：原子性和隔离性保证了一致性。
隔离性：由 MVCC 来保证。
持久性：由 内存 + redo log来保证，mysql修改数据同时在内存和redo log记录这次操作，事务提交的时候通过redo log刷盘，宕机的时候可以从redo log恢复。

## 4.2. 事务隔离级别

读未提交 Read uncommitted：如果一个事务读取到了另一个未提交的事务修改过的数据。
读已提交 Read committed：一个事务能读到另一个已经提交的事务修改过的数据。
可重复读 Repeatable read：一个事务只会读取该事务开始时的记录快照版本。
串行化 serializable：不允许 读-写、写-读 的并发操作。

## 4.3. 并发一致性问题

**丢失修改**：指一个事务的更新操作被另外一个事务的更新操作替换
**脏读**：当前事务可以读到另一个事务还未提交的数据
**不可重复读**：同一个事务内，两只执行同一条语句查询的结果不一致。
**幻影读**：同一个事务内，两次使用 count 计数的结果不一致。

**不可重复读和幻读的区别：**
避免不可重复读需要锁行就行；避免幻影读则需要锁表。
不可重复读重点在于update；而幻读的重点在于insert

||脏读|不可重复读|幻影读|
|:-:|:-:|:-:|:-:|
|**读未提交**|Y|Y|Y|
|**读已提交**| |Y|Y|
|**可重复读**| | |Y|
|**串行化**  | | | |

## 4.4. 其他

**事务的分类**

- 扁平事务
  在扁平事务中，所有的操作都处于一个层次，由 begin work 开始，commit work 或者 rollback work 结束，期间操作是原子的，要么都执行，要么都回滚。
  ![image-20200328200611471](../images/image-20200328200611471.png)

- 带有保存点的扁平事务
  支持扁平事务的一切操作，还新加了保存点，允许事务回滚到保存点。保存点用 save work 函数来创建。

- 链事务
  在提交一个事务时，释放不需要的数据对象，将必要的处理上下文隐式地传给下一个要开始的事务（提交事务和开始下一个事务 合并为一个原子操作）。
  ![image-20200328201451756](../images/image-20200328201451756.png)

- 嵌套事务
  由一个顶层事务控制着各个层次的事务。
  ![image-20200328201545774](../images/image-20200328201545774.png)
  ![image-20200328201758345](../images/image-20200328201758345.png)

- 分布式事务
  通常是在一个分布式环境下允许的扁平事务，需要根据数据所在位置访问网络中 的不同节点。

**redo log 与 undo log**

redo log：重做日志，用来实现事务的持久性。该日志文件由 重做日志缓冲(redo log buffer) 和 重做日志(redo log)组成，前者存在内存中，后者存在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中。
- 特点 mysql 为了提升性能不会把每次的修改都实时同步到磁盘，而是会先存到Buffer Pool(缓冲池)里头，把这个当作缓存来用。然后使用后台线程去做缓冲池和磁盘之间的同步。
- 问题 引入了redo log来记录已成功提交事务的修改信息，并且会把redo log持久化到磁盘，系统重启之后在读取redo log恢复最新数据。
- 作用 redo log是用来恢复数据的 用于保障已提交事务的持久化特性。

undo log：回滚日志，用于记录数据被修改前的信息。他正好跟前面所说的重做日志所记录的相反，重做日志记录数据被修改后的信息。undo log主要记录的是数据的逻辑变化，为了在发生错误时回滚之前的操作，需要将之前的操作都记录下来，然后在发生错误时才可以回滚。
- 特点 每次写入数据或者修改数据之前都会把修改前的信息记录到 undo log。
- 问题 undo log 记录事务修改之前版本的数据信息，因此假如由于系统错误或者rollback操作而回滚的话可以根据undo log的信息来进行回滚到没被修改前的状态。
- 作用 undo log是用来回滚数据的用于保障未提交事务的原子性

## 4.5. MVCC 多版本并发控制

多版本并发控制（Multi-Version Concurrency Control, MVCC）实际上就是保存了数据在某个时间节点的快照。
MVCC 是 MySQL 的 InnoDB 存储引擎实现隔离级别的一种具体方式。
- 用于实现提交读和可重复读这两种隔离级别。
- 而未提交读隔离级别总是读取最新的数据行，要求很低，无需使用 MVCC。
- 可串行化隔离级别需要对所有读取的行都加锁，单纯使用 MVCC 无法实现。

**基本思路**
与 CopyOnWrite 的思想类似，写操作不会在原来的数据上修改，而是会将修改后版本的数据新增在表中。读操作依然可以去读老版本的数据。不同事务隔离级别的区别就是读取的数据版本不同（提交读总是读取最新版本数据，可重复读则只读取事务开启时的版本数据）。

**版本号**
- 系统版本号 SYS_ID：是一个递增的数字，每开始一个新的事务，系统版本号就会自动递增。
- 事务版本号 TRX_ID ：事务开始时的系统版本号。

**具体机制**
数据表中的每行数据实际上隐藏了两列，创建时间版本号，过期(删除)时间版本号，每开始一个新的事务，版本号都会自动递增。

假设目前表中数据如下：
|id|name|create_version|delete_version|
|:-:|:-:|:-:|:-:|
|1|张三|1||
|2|李四|2||

执行一条查询语句，current_version = 3
```sql
select * from tb_user;
-- 结果
-- |id|name|
-- |1 |张三|
-- |2 |李四|
```

然后有个事务 A 执行以下操作，current_version = 4
```sql
update user set name='张六' where id = 1;
```
数据库中的数据变成这样了，current_version = 4
|id|name|create_version|delete_version|
|:-:|:-:|:-:|:-:|
|1|张三|1||
|2|李四|2||
|1|张六|4||

又有另一个事务 B 执行删除操作，current_version = 5
```sql
delete from user where id = 2;
```
数据库中的数据变成这样了，current_version = 5
|id|name|create_version|delete_version|
|:-:|:-:|:-:|:-:|
|1|张三|1||
|2|李四|2|5|
|1|张六|4||

再执行第一个查询语句
（MVCC的原理是查找创建版本小于或等于当前事务版本，删除版本为空或者大于当前事务版本）
```sql
select * from tb_user;
-- 相当于
select * from user where create_version <= 3 and (delete_version > 3 or delete_version is null);
-- 结果
-- |id|name|
-- |1 |张三|
-- |2 |李四|
```


# 5. 锁

lock 的对象是事务，用来锁定数据库中的对象。如：表、页、行。一般 lock 的对象仅在事务 commit 或 rollback 后释放。有死锁检测机制。
latch 称为闩锁（轻量级的锁），它要求锁定时间必须非常短，否则性能会很差。latch 分为 mutex (互斥锁) 和 rwlock (读写锁)。没有死锁检测机制。
![image-20200328151857549](../images/image-20200328151857549.png)

## 5.1. 锁的分类

**读写锁**
- 共享锁 (S Lock)：允许事务读一行数据。
- 排它锁 (X Lock)：允许事务删除或更新一行数据。
X 锁与任何锁都不兼容，S 锁之间兼容。S 和 X 都是行锁。
![image-20200328173003024](../images/image-20200328173003024.png)

**意向锁**
- 意向共享锁 (IS Lock)：事务想要获得一张表中某几行的共享锁。
- 意向排它锁 (IX Lock)：事务想要获得一张表中某几行的排它锁 。
意向锁之间相互兼容，IS 与 S 兼容
![image-20200328173149700](../images/image-20200328173149700.png)


**一致性非锁定读**
如果读取的行正在被修改或删除， InnoDB 存储引擎回去读取行的一个之前版本的快照数据（undo段）。
- Read Committed 事务隔离级别：总是读取行的最新版本（每次读取数据前都生成一个 ReadView）。
- Repeatable Read  事务隔离级别：总是读取事务开始时的版本（只在第一次读取数据时生成一个 ReadView）。

**一致性锁定读**
- select ... for update：加 X 锁，其他事务不能对该行加任何锁。
- select ... lock in share mode：加 S 锁，其他事务只能加 S 锁，加 X 锁会被阻塞。

**自增长与锁**
-INC Locking 锁是一种特殊的表锁，锁是在完成了自增长值的插入后被释放。

**外键和锁**
对于一个外键列，如果没有显示的给这个列加索引，InnoDB 引擎会自动加一个索引。这样做可以避免表锁。

## 5.2. 封锁协议

### 5.2.1. 三级封锁协议

**一级封锁协议**
事务 T 要修改数据 A 时必须加 X 锁，直到 T 结束才释放锁。可以解决丢失修改问题。

**二级封锁协议**
在一级的基础上，要求读取数据 A 时必须加 S 锁，读取完马上释放 S 锁。可以解决读脏数据问题。

**三级封锁协议**
在一级的基础上，要求读取数据 A 时必须加 S 锁，直到事务结束了才能释放 S 锁。可以解决不可重复读的问题。

||丢失修改|脏读|不可重复读|
|:-:|:-:|:-:|:-:|
|**一级封锁协议**| |Y|Y|
|**一级封锁协议**| | |Y|
|**三级封锁协议**| | | |

### 5.2.2. 两段锁协议

加锁和解锁分为两个阶段进行。当执行完最后一个加锁操作后，才能开始执行解锁操作。

事务遵循两段锁协议是保证可串行化调度的充分不必要条件。满足两段锁协议一定可以可串行化调度；可串行化调度不一定满足两段锁协议。

## 5.3. 行锁的三种算法

> 《MySQL 技术内幕 InnoDB 存储引擎》-> 6.4 锁的算法
> MySQL的锁机制 - 记录锁、间隙锁、临键锁·知乎 https://zhuanlan.zhihu.com/p/48269420

**Phantom Problem 幻想问题**
Phantom Problem 是指在同一事务中，连续执行两次同样的 SQL 语句可能导致不同的结果，第二次的 SQL 语句可能会返回之前不存在的行。

### 5.3.1. Record Lock
单个行记录上的锁。
Record Lock总是会去锁住索引记录，如果InnoDB存储引擎表在建立的时候没有设置任何一个索引，那么这时InnoDB存储引擎会使用隐式的主键来进行锁定。
> A record lock is a lock on an index record. Record locks always lock index records, even if a table is defined with no indexes. For such cases, InnoDB creates a hidden clustered index and uses this index for record locking.

### 5.3.2. Gap Lock
间隙锁，锁定一个范围，但不包括记录本身。

显示地关闭 Gap Lock:
1. 将事务的隔离级别设置为 Read Committed 
2. 将参数 innodb_locks_unsafe_for_binlog 设置为 1

### 5.3.3. Next-Key Lock
Record Lock + Gap Lock，锁定一个范围，并且锁定记录本身。

Next-Key Locks 是 MySQL 的 InnoDB 存储引擎的一种锁实现。
MVCC 不能解决幻影读问题，Next-Key Locks 就是为了解决这个问题而存在的。在可重复读（REPEATABLE READ）隔离级别下，使用 MVCC + Next-Key Locks 可以解决幻读问题。

**查询列是唯一索引时，Next-Key Lock 降级为 Record Lock。**

## 5.4. 阻塞

阻塞：有时一个事务的锁需要等待另一个事务的锁释放它所占用的资源。

InnoDB 存储引擎中参数 innodb_lock_wait_timeout 用来控制等待时间（默认 50 秒）；参数 innodb_rollbck_on_timeout 用来设定是否在 等待超时时对进行中的事务进行回滚操作（默认 OFF 不回滚）。

innodb_lock_wait_timeout 是动态的，可以在数据库运行时调整；innodb_rollbck_on_timeout 是静态的，不可以在启动是修改。

必须在事务抛出异常时，进行 commit 或者 rollback，否则会十分危险。

## 5.5. 死锁

死锁：两个或者两个以上的事务在执行过程中，因为争夺锁而造成的一种相互等待的现象。若无外力作用，事务将无法推进下去。
线程死锁：两个或者两个以上的线程相互等待对方无法释放的资源，这种等待在无外力的作用下不可接触。

解决死锁：
1. 超时法：当等待时间超过某个值时，一个事务回滚，另一个事务继续执行。
2. 等待图法 wait-for graph：
   数据库需要保存以下两种信息：
   - 锁的信息链表
   - 事务等待链表
   通过上述链表可以构成一张图，若图中存在回路，就代表存在死锁。

## 5.6. 锁升级

将多个粒度较小的锁升级为粒度更大的锁。
![image-20200329231635090](../images/image-20200329231635090.png)

InnoDB 存储引擎不存在锁升级的问题。
![image-20200328195006465](../images/image-20200328195006465.png)


# 6. 索引

## 6.1. 索引优点与使用场景

**索引的优点：**
1. 索引大大减少了服务器需要扫描的数据量
2. 索引可以帮助服务器避免排序和临时表
3. 索引可以将随机 I/O 变为顺序 I/O

**索引的使用场景：**
- 对于非常小的表，大部分情况下全表扫描效率更高。
- 中到大型表，索引非常有效。
- 特大型的表，建立和使用索引的代价会随之增大，可以使用分区技术来解决。

**索引的类型：**索引很多种类型，是在MySQL的存储引擎实现的。
- 普通索引：最基本的索引，没有任何约束限制。
- 唯一索引：和普通索引类似，但是具有唯一性约束，可以允许有空值。
- 主键索引：特殊的唯一索引，不允许有空值。

**索引的区别：**
- 一个表只能有一个主键索引，但是可以有多个唯一索引。
- 主键索引一定是唯一索引，唯一索引不是主键索引。
- 主键可以与外键构成参照完整性约束，防止数据不一致。
- 联合索引：将多个列组合在一起创建索引，可以覆盖多个列。（也叫复合索引，组合索引）
- 外键索引：只有InnoDB类型的表才可以使用外键索引，保证数据的一致性、完整性、和实现级联操作（基本不用）。
- 全文索引：MySQL自带的全文索引只能用于MyISAM，并且只能对英文进行全文检索 （基本不用）

![image-20200426183030831](../images/image-20200426183030831.png)

**MyISAM索引与InnoDB索引的区别？**
- InnoDB索引是聚簇索引，MyISAM索引是非聚簇索引。
- InnoDB的主键索引的叶子节点存储着行数据，因此主键索引非常高效。
- MyISAM索引的叶子节点存储的是行数据地址，需要再寻址一次才能得到数据。
- InnoDB非主键索引的叶子节点存储的是主键和其他带索引的列数据，因此查询时做到覆盖索引会非常高效。
![image-20200426130649578](../images/image-20200426130649578.png)
![image-20200426130711071](../images/image-20200426130711071.png)

## 6.2. MySQL 索引种类

### 6.2.1. B+ 树索引

**二分查找法**
- 将一组数据先升序排序，然后每次比较中点位置的值；若大于则在右半区间查找，否则在左半区间查找。

**二叉查找树**
- 二叉查找树中，左孩子的值总是小于父亲的值，右孩子的值总是大于父亲的值。
- 二叉查找树可能会不平衡，变成链表。

**平衡二叉树**
- 符合二叉查找树
- 必须满足任何节点的两个子树的高度之差小于等于1
维护一棵平衡二叉树，需要经常性的旋转操作，开销较大。


**B+ 树**
由 B 树和索引顺序访问方法 (ISAM) 演化而来。
最下面一层用于存放数，叫做 Leaf Page；上面几层存放索引，叫做 Index Page。 Leaf Page 之间通过双向链表连接起来。
![image-20200328222016449](../images/image-20200328222016449.png)

**插入操作** 平衡扩张

1. Leaf 未满，Index 未满：
   直接将记录插入到叶子节点的相应位置中。

2. Leaf 满了，Index 未满：
   需要拆分 Leaf Page。将目标页的中间节点放入 Index 中。小于中间节点的放左边 Page；大于等于的放入右边 Page。

3. Leaf 满了，Index 满了：
   需要拆分 Leaf Page 和 Index Page。先按 2 操作，然后再防止 中间节点。将Index Page 的中间节点放入上一层的 Index Page。小于中间节点的放左边 Page；大于等于的放入右边 Page。

![image-20200328214513454](../images/image-20200328214513454.png)

201页，edge的框上输入。

**旋转操作**
当前Leaf Page满了，而其**左右**兄弟 Leaf Page 未满时，进行旋转操作。
将记录移动到其兄弟节点上，并重新更新 Index Page。

**删除操作**
使用填充因子来控制删除操作，填充因子最小值是 50%。
填充因子 = 有数的节点数目 / 总节点数目。

1. Leaf 不小于 填充因子，Index 不小于 填充因子
   1.1 要删除的数据不是第一个，直接删除
   1.2 要删除的数据是第一个，删除后，更新 Index Page
2. ...... 没看懂
![image-20200328220614492](../images/image-20200328220614492.png)
204页，edge的框上输入。

**在数据库中 B+ 树的高度一般在 2 — 4 层。**

#### 6.2.1.1. 聚集索引

聚集索引就是按照每张表的主键构造一棵 B+ 树，叶子节点中存放的是整张表的行记录数据。也将聚集索引的叶子节点叫做数据页。
每张表只有一个聚集索引。聚集索引并不是物理上连续的，而是逻辑上连续的。

**优点**
1. 减少磁盘IO
2. 数据访问更快
3. 使用覆盖索引扫描的查询可以直接使用页节点中的主键值
4. 对于主键的范围查找和排序查找速度非常快。

**缺点**
1. 插入速度依赖与插入顺序
2. 更新索引的代价很高
3. 页分裂问题
4. 二级索引变大，因为与主索引有关
5. 二级索引查找的次数不只一次

#### 6.2.1.2. 辅助索引 (非聚集索引)

辅助索引的叶子节点不包括行记录的全部数据，包含一个键值和书签。
当使用辅助索引来查询数据时，会先通过辅助索引找到指向聚集索引的主键，然后再通过聚集索引来找到完整的记录。

#### 6.2.1.3. 联合索引

索引匹配的最左原则具体是说，假如索引列分别为A，B，C，顺序也是A，B，C：
- 那么查询的时候，如果查询【A】【A，B】 【A，B，C】，那么可以通过索引查询
- 如果查询的时候，采用【A，C】，那么C这个虽然是索引，但是由于中间缺失了B，因此C这个索引是用不到的，只能用到A索引
- 如果查询的时候，采用【B】 【B，C】 【C】，由于没有用到第一列索引，不是最左前缀，那么后面的索引也是用不到了
- 如果查询的时候，采用范围查询，并且是最左前缀，也就是第一列索引，那么可以用到索引，但是范围后面的列无法用到索引

**MySQL联合索引，为什么范围查找不能使用索引**
![](../images/范围查找不能使用索引.png)
![](../images/索引使用总结.jpg)

#### 6.2.1.4. 覆盖索引

如果在一次查询中，如果一个索引包含或者说覆盖所有需要查询的字段的值，则不再需要回表查询。我们就称之为覆盖索引。
要确定一个查询是否是覆盖索引，通过 explain 命令查看 Extra 的结果是否为 Using index。

### 6.2.2. 全文索引

MyISAM 存储引擎支持全文索引，用于查找文本中的关键词，而不是直接比较是否相等。
查找条件使用 MATCH AGAINST，而不是普通的 WHERE。
全文索引使用倒排索引实现，它记录着关键词到其所在文档的映射。

InnoDB 存储引擎在 MySQL 5.6.4 版本中也开始支持全文索引。

### 6.2.3. 哈希索引

哈希索引查找复杂度为 $O(1)$
哈希索引是自适应的，InnoDB 会根据需要添加，不能人为干预。

### 6.2.4. 空间数据索引(R-Tree)

MyISAM表支持空间索引，可以用作地理数据存储。


## 6.3. 索引优化

### 6.3.1. 单列索引

主要要注意索引列不能作为表达式的一部分参与运算，或者做为函数的参数，否则不会走索引。**只要列涉及到运算和函数，MySQL就不会使用索引**。

例如：
有如下数据表和单列索引：
```sql
create table `order`
(
   `id` int auto_increment,
   `member_id` int,
   `status` int null,
   `create_time` datetime null,
   constraint order_pk
      primary key (id)
);

create index order_create_time_index
  on `order` (create_time);

insert into `order`(id, member_id, status, create_time) VALUE (1, 1, 3, '2020-9-11');
insert into `order`(id, member_id, status, create_time) VALUE (2, 2, 1, '2020-9-12');
insert into `order`(id, member_id, status, create_time) VALUE (3, 3, 4, '2020-9-9');
insert into `order`(id, member_id, status, create_time) VALUE (4, 4, 0, '2020-9-13');
insert into `order`(id, member_id, status, create_time) VALUE (5, 5, 1, '2020-9-11');
insert into `order`(id, member_id, status, create_time) VALUE (6, 6, 2, '2020-9-11');
insert into `order`(id, member_id, status, create_time) VALUE (7, NULL, 0, '2020-9-7');
```
如下两条 SQL
```sql
-- 走索引 order_create_time_index
explain select * from `order` where create_time = '2020-9-11';
-- 不能走索引
explain select * from `order` where year(create_time) = 2020;
```

### 6.3.2. 多列索引

注意最左前缀原则。
让选择性最强的索引列放在前面。选择性强是指，数据重复性低的。如：id 属于选择性强，性别选择性弱。

```sql
create index order_status_member_id_create_time_index
  on `order` (`status`, member_id, create_time);

-- 走索引 order_status_member_id_create_time_index    Extra: Using where
explain select * from `order` where status = 2 and member_id = 6 and create_time = '2020-9-13';
-- 走索引 order_create_time_index    Extra: Using index
explain select * from `order` where status = 2 and member_id = 6 and create_time = '2020-9-11';

-- 走索引 order_status_member_id_create_time_index    Extra: Using where; Using index
explain select * from `order` where status = 2 and member_id >= 6 and create_time = '2020-9-11';
-- 走索引 order_status_member_id_create_time_index    Extra: Using where; Using index
explain select * from `order` where status > 2 and member_id = 6 and create_time = '2020-9-11';
```

### 6.3.3. 索引列的顺序

让选择性最强的索引列放在前面。
索引的选择性是指：不重复的索引值和记录总数的比值。最大值为 1，此时每个记录都有唯一的索引与其对应。选择性越高，每个记录的区分度越高，查询效率也越高。

### 6.3.4. [建立索引的常用技巧](https://juejin.im/post/5a6873fbf265da3e393a97fa)

1. 最左前缀匹配原则，非常重要的原则，mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配，比如a = 1 and b = 2 and c > 3 and d = 4 如果建立(a,b,c,d)顺序的索引，d是用不到索引的，如果建立(a,b,d,c)的索引则都可以用到，a,b,d的顺序可以任意调整。
2. =和in可以乱序，比如a = 1 and b = 2 and c = 3 建立(a,b,c)索引可以任意顺序，mysql的查询优化器会帮你优化成索引可以识别的形式
3. 尽量选择区分度高的列作为索引,区分度的公式是count(distinct col)/count(*)，表示字段不重复的比例，比例越大我们扫描的记录数越少，唯一键的区分度是1，而一些状态、性别字段可能在大数据面前区分度就是0，那可能有人会问，这个比例有什么经验值吗？使用场景不同，这个值也很难确定，一般需要join的字段我们都要求是0.1以上，即平均1条扫描10条记录
4. 索引列不能参与计算，保持列“干净”，比如from_unixtime(create_time) = ’2014-05-29’就不能使用到索引，原因很简单，b+树中存的都是数据表中的字段值，但进行检索时，需要把所有元素都应用函数才能比较，显然成本太大。所以语句应该写成create_time = unix_timestamp(’2014-05-29’);
5. 尽量的扩展索引，不要新建索引。比如表中已经有a的索引，现在要加(a,b)的索引，那么只需要修改原来的索引即可，当然要考虑原有数据和线上使用情况。

```sql
KEY(a,b,c)

WHERE a = 1 AND b = 2 AND c = 3
WHERE a = 1 AND b = 2
WHERE a = 1
#以上SQL语句可以用到索引

WHERE b = 2 AND c = 3
WHERE a = 1 AND c = 3
#以上SQL语句用不到索引
```

**以下三条sql 如何建索引，只建一条怎么建？**
```sql
WHERE a=1 AND b=1
WHERE b=1
WHERE b=1 ORDER BY time DESC
```
以顺序b,a,time建立联合索引，
```sql
CREATE INDEX table1_b_a_time ON index_test01(b,a,time)
```
因为最新MySQL版本会优化WHERE子句后面的列顺序，以匹配联合索引顺序。

# 7. explain

1. **id**：SELECT 识别符，sql 语句执行的顺序
2. **select_type**：
   - simple：简单查询
   - primary：在有子查询的语句中，最外面的select查询就是primary
   - union：union 语句的后面那一个
   - dependent union
   - union result
3. **table**：输出的行所用的表
4. **type**：连接类型
   - system：表仅有一行数据， const 的特例
   - const：表最多有一个匹配行
   - eq_ref
   - ref 
   - ref_or_null 
   - index_merge 
   - unique_subquery 
   - index_subquery
   - range 
   - index   
   - ALL 


# 8. 附录：常用SQL语法
```sql
[] 可选 0次或1次
{} 0次或多次
| 或者


-- SQL模式/架构的创建
create schema 
{
	<架构名> 
  | authorization <用户名>
  | <架构名> authorization <用户名>
}
[{
	表定义语句
  |	视图定义语句
  |	授权语句
  |	拒绝授权语句
}]
-- SQL模式/架构的撤销
drop schema <模式名> [ cascade | restrict ]
-- cascade: 删除架构的同时删除该架构中的所有对象
-- restrict: 如果被删除的架构中包含有架构对象，则拒绝删除此架构

create database <数据库名>
drop database <数据库名> 

-- 建表
create table [<架构名>.]<表名>
(
	{ <列名> <数据类型> [列级完整性约束] [, ...n] }
	[, 表级完整性约束] [, ...n]
);

-- 数据类型
integer / int	-- 长整数
smallint		-- 短整数
tinyint
real			-- 浮点数
double precision -- 双精度浮点数
float(n)		-- 浮点数, 精度至少为n位
numeric(p,d) / decimal(p,d)	-- 定点数,有p位数字(不包括符号,小数点),小数点后面有d位数字
char(n)			-- 长度为n的定长字符串 n : 1~8000
varchar(n)		-- 最大长度为n的变长字符串 n : 1~4000
bit(n)			-- 长度为n的定长二进制位串
bit varying(n)	-- 最大长度为n的变长二进制位串
date			-- 年月日 YYYY-MM-DD
time			-- 时分秒 hh:mm:ss[.nnnnnnn]。精确到100ns
datetime		-- YYYY-MM-DD?hh:mm:ss.nnn。精确到0.00333s
SmallDatatime	-- YYYY-MM-DD?hh:mm:00。精确到分钟。

-- 约束
not null -- 非空
unique -- 唯一值约束
default -- 默认值约束
check -- 取值范围约束
primary key -- 主键
foreign key -- 外键
-- 主键约束
primary key[(<列名> [, ...n])]
PIID int primary key
primary key(PID, MID)
-- 外键约束
[foreign key (<列名>)] references <外表名>(<外表列名>)
MID smallint not null references domiMembers (MID)
foreign key (PID) references payment(PID)
-- 唯一值约束
unique [(<列名> [, ...n])]
-- 默认值约束
default 常量表达式			--	建表时
default 常量表达式 for 列名	--  修改表
-- DEFAULT约束只能作为列级完整性约束
payDT  datetime default getdate()
-- 取值范围约束
check(逻辑表达式)


-- 修改表
alter table [<架构名>.]<表名>
{
	alter column <列名> <新数据类型>	-- 修改列定义
  | add <列名> <新数据类型> [约束]		-- 添加新列
  | drop column <列名>					-- 删除列
  | add [constraint <约束名>] 约束定义	-- 添加约束
  | drop <约束名>						-- 删除约束
}

-- 删除表
drop table <表名> {, <表名>}


-- 查询表
select [distinct] <目标列名序列>
	from <表名> [join <表名> on <连接条件>]	-- [inner] left/right/full [outer] join 
	[where <行选择条件>]
	[group by <分组依据列>] [having <组选择条件>]
	[order by <排序依据列>]
distinct -- 去除重复元组

WHERE?Age?BETWEEN?20?AND?30
-- 子句，查找的Age范围是多少？?
-- 答：Age的范围是，大于等于20，小于等于30.?


is null | is not null
like 
_	-- 匹配任意一个字符
%	-- 匹配0到多个字符
[]	-- 匹配括号中任意一个字符 [abc]:匹配a或b或c [a-z]:匹配a到z
[^] -- 不匹配括号中的字符

order by <列名> [asc | desc] [, ...n]		-- 默认asc(升序)

-- 聚合函数
count(*)	-- 统计表中元组个数
count([distinct] <列名>)	-- 统计本列的列值个数  distinct:去重
sum(<列名>)
avg(<列名>)
max(<列名>)
min(<列名>)
-- 以上除了count(*)外,均忽略NULL值

-- 查询结果保存到表中
select 查询列表序列 into <新表名>
	from 数据源
	...				-- 其他条件子句，分组子句等

#<表名>  -- 局部临时表
##<表名>  -- 全局临时表

where [not] exists (子查询)


-- 插入数据
insert [into] <表名> [(列名)] values(值列表)	-- 单行插入
insert [into] <表名> [(列名)] select语句		-- 多行插入

-- 更新数据
update <表名> set <列名> = {表达式 | default | null} [, ...n]
	[from <条件表名> [, ...n]]
	[where <更新条件>]

-- 删除数据
delete [from] <表名>
	[from <条件表名> [, ...n]]
	[where <删除条件>]

-- 创建索引
create [unique] | [clustered] | [nonclustered]
	index <索引名> on <表名>(<列名> [, ...n])

unique -- 唯一索引
clustered -- 聚集索引 一张表只有一个
nonclustered -- 非聚集索引 默认

desc -- 降序
asc  -- 升序

drop index <表名>.<索引名>


-- 视图
create view <视图名> [(<列名> [, ...n])]
as
	select语句

-- 修改视图
alter view <视图名> [(<列名> [, ...n])]
as
	select语句

-- 删除视图
drop view <视图名>


-- T-SQL
-- 全局变量
@@<other>
@@connections	-- 返回当前到本服务器的连接的数目。
@@rowcount		-- 返回上一条T-SQL语句影响的数据行数。 
@@error			-- 返回上一条T-SQL语句执行后的错误号。 
@@procid		-- 返回当前存储过程的ID号 
@@remserver		-- 返回登录记录中远程服务器的名字。 
@@spid			-- 返回当前服务器进程的ID标识。 
@@version		-- 返回当前SQL Server服务器的版本和处理器类型。 
@@language		-- 返回当前SQL Server服务器的语言

-- 局部变量
@<other>
DECLARE  @local_variable data_type [, @local_variable data_type] ... -- 定义局部变量
declare @PID int;	
declare @PID int, @MID int;
-- 赋值
SET @local_variable = expression
SELECT @variable_name=expression [，@variable_name=expression]
	[FROM list of tables]
	[WHERE expression]
	.....

-- 函数
CAST ( 表达式 AS  数据类型[(长度)] )  -- 数据类型转换
cast(IID as varchar(20))
getdate()	-- 获取当前时间
year()  month()  day()

-- 函数定义
create function <函数名>(<参数列表>)
	return <数据类型>
as
	begin
		<函数体>
		return <返回值表达式>
	end

create function CubicVolume
(
	@CubeLength numeric(4,1),
	@CubeWidth numeric(4,1),
	@CubeHeight numeric(4,1),
)
	return numeric(12,3)
as
	begin
		return (@CubeLength * @CubeWidth * @CubeHeight)
	end

-- 流程控制语句
-- 块语句
begin
	Sql_statement1
	Sql_statement2
	...
	Sql_statementn
end
-- 条件分支语句
if <布尔表达式>
	<SQL语句或语句块>
else
	<SQL语句或语句块>
-- case表达式
case
	{when <条件表达式0> then <结果表达式0>} [, ...n]
	[else <结果表达式n+1>]
end
-- 循环语句
while <布尔表达式>
begin
	<命令行或程序块>
	break
	continue
end


-- 存储过程
create procedure <存储过程名>
[ {@<参数名> <数据类型>} [=default] [output] ] [, ...n]
[ with {recompile | encryption | recompile , encryption} ]
as
	SQL语句

execute <存储过程名> [<实参> [output]] [, ...n]

-- 游标
declare <游标名> cursor for select语句	-- 定义游标
open <游标名>	-- 打开游标
@@fetch_status	-- 游标状态 为0时数据可操作
fetch <游标名> [into <变量1>, <变量2>, ...]
close <游标名>	-- 关闭游标
deallocate <游标名>	-- 释放游标


-- 触发器
create trigger <触发器名> on {<表名> | <视图名>}
{ for | after | instead of }	-- 触发时机
{ [insert] [,] [delete] [,] [update] }	-- 触发事件
as
	SQL语句

inserted	-- 新插入的数据
deleted		-- 被删除的数据

drop trigger <触发器名>

update()	-- 只有在insert或update触发器中可用。在触发器里，有时候我们要判断更新的是不是某列，这个时候就可以使用 UPDATE()。

测试
-- 例：在S表上定义触发器，阻止SNO字段值被修改 
create trigger sno_upd on S for update
as
	if update(sno)
		rollback transaction
	return


-- 事务
begin transaction <事务名>
rollback
commit
save transaction p1;
rollback transaction p1


-- 语句权限
create database
create table
create procedure
create view
-- 授权语句
grant <语句权限名> [, ...n] to <用户名> [, ...n]
	[with grant option]		-- 允许将其权限授给其他用户
-- 收权语句
revoke <语句权限名> [, ...n] from <用户名> [, ...n]
-- 拒绝权限语句
deny <语句权限名> [, ...n] to <用户名> [, ...n]

-- 对象权限
delete insert update select	-- 表 | 视图
execute	-- 存储过程
references
-- 授权语句
grant <对象权限名> [, ...n] on {表名 | 视图名 | 存储过程名}
	to <用户名> [, ...n]
	[with grant option]		-- 允许将其权限授给其他用户
-- 收权语句
revoke <对象权限名> [, ...n] on {表名 | 视图名 | 存储过程名}
	from <用户名> [, ...n]
-- 拒绝权限语句
deny <对象权限名> [, ...n] on {表名 | 视图名 | 存储过程名}
	to <用户名> [, ...n]

grant select(pno,pname,tm) on tourist to user1

-- 常用系统函数
-- 日期函数
getdate()	-- 返回当前系统时间 datetime
select getdate();

dateadd(datepart, number, date)	-- 对给定日期date的datepart部分加上一段时间number
select dateadd(day, 100, getdate());

datediff(datepart, stardate, enddate) -- 返回两个日期之间所差的日期
select datediff(year ,'1999/9/13', getdate());

datepart(datepart, date) -- 返回date的指定部分
select datepart(year, getdate())

year()  month()  day()

-- 字符串函数
left(chars, n)	-- 从左边开始指定个数的子串
select left('abcde', 2);	-- ab

right(chars, n)	-- 从右边开始指定个数的子串
select right('abcde', 2);	-- de

len(chars)	-- 计算字符串中的字符个数
select len('abcde');	-- 5

ltrim() -- 删除字符串左边的空格
rtrim() -- 删除字符串右边的空格
select ltrim(rtrim('   sdfas  asds   '));	-- sdfas  asds

substring(chars, star, length)	-- 从第star个字符开始截取lenght个字符 （从1开始计数）
select substring('abcdefght', 3, 6);	-- cdefgh

-- 类型转换
cast(<表达式> as <数据类型>[(大小)])
select cast(189.231784 as numeric(5,2));


+ -- 字符串连接符
-- [列名]


-- 单词表
schema -- 架构
authorization -- 授权（创建架构时指定的用户）
numeric(p,d) -- 定点数
unique -- 唯一值约束
primary key -- 主键
foreign key  references  -- 外键
default -- 默认值约束
check -- 取值范围约束
alter -- 修改表
column -- 列
constraint -- 约束名
distinct -- 去除重复元组
percent -- 百分比
exists -- 存在
clustered -- 聚集索引 一张表只有一个
nonclustered -- 非聚集索引 默认
desc -- 降序
asc  -- 升序
table -- 表
index -- 索引
view -- 视图
function -- 函数
procedure -- 存储过程
execute -- 执行
declare -- 定义变量
deallocate -- 释放游标
fetch -- 遍历游标
trigger -- 触发器
inserted	-- 新插入的数据
deleted		-- 被删除的数据
rollback -- 回滚
commit -- 提交
with grant option -- 允许将其权限授给其他用户
revoke -- 收权
deny -- 拒绝

@@connections	-- 返回当前到本服务器的连接的数目。
@@rowcount		-- 返回上一条T-SQL语句影响的数据行数。 
@@error			-- 返回上一条T-SQL语句执行后的错误号。 
@@procid		-- 返回当前存储过程的ID号 
@@remserver		-- 返回登录记录中远程服务器的名字。 
@@spid			-- 返回当前服务器进程的ID标识。 
@@version		-- 返回当前SQL Server服务器的版本和处理器类型。 
@@language		-- 返回当前SQL Server服务器的语言
@@fetch_status	-- 游标状态 为0时数据可操作


-- 5.查询在所有窗口消费过的一卡通信息，包括一卡通卡号、学号、开卡日期、余额。		shangji05.sql
select cno, sno, carddate, remainingsum from schoolcard
where cno in (
	select cno from (select cno, count(*) as num from (select cno, servicewno from foodconsume group by cno,servicewno) S group by cno) C
		where num  =  (select count(*) from servicewindows)
);
--  ·查询选了全部课程的学生的姓名和所在班级		shangji03.sql
select snm, clsnm from stu, class where exists (
	select * from study group by sno having count(cno)=(select count(cno) from course)
	and class.clsno=stu.clsno
);
-- 查询选修了所有课程的学生
select sno,snm from stu where not exists(
	select * from course where not exists(
		select * from study where sno = stu.sno and cno = course.cno
	)
);
-- 查询选修了学号为“8002117171”的学生所选全部课程的学生姓名
select snm from stu where not exists(
	select * from study s1 where sno='8002117171' and not exists(
		select * from study s2 where s2.sno=stu.sno and s1.cno=s2.cno
	)
);
-- 一、事务应用 Save tran
-- 用超级用户连接SQL Server
--   ⑴定义一个事务：
--     ①将所有的的消费记录增加5元
--     ②设置事务存储点
--     ③增加一个学生信息（12345，张三，'Java165班'）
--     ④如果出现错误，则回滚到事务存储点
--     ⑤如果回滚后还出现错误，则本事务全部回滚；如果回滚后没有错误，则提交本事务
--   ⑵将⑴再执行一遍，这样会产生学生信息插入错误，看save tran是否发生了作用
begin tran
	begin try
		update foodConsume set amount = amount + 5;
		save tran p1;
		insert into stuInfo(sno,sname,sclass) values(12345,'张三','Java165班');
		if @@ERROR > 0
		begin
			rollback tran p1;
		end 
		commit tran
	end try
	begin catch
		rollback tran
	end catch


数据库系统 = 数据库 + 数据库管理系统 + 应用程序 + 数据库管理员

候选键: 如果一个属性或属性集的值能够唯一标识一个关系的元组而又不包含多余的属性。(侯选关键字、候选码)
主键: 当一个关系中有多个候选键时，可以从中选择一个做为主键。(主码、主关键字)
主属性: 任意候选键中的属性
非主属性: 不包含在任何候选键中的属性

-- 关系规范化理论
1NF: 不包含非原子项属性
2NF: 1NF + 每个非主属性都完全函数依赖于主键
3NF: 2NF + 每个非主属性都不传递函数依赖于主键
BCNF: 3NF + 每个函数依赖的决定因子都是候选键
	可能不满足BC的：(1)关系中包含两个(或多个)复合候选键  (2)候选键的属性有重叠
	

架构（schema，也称为模式）是数据库下的一个逻辑命名空间，可以存放表、视图等数据库对象，它是一个数据库对象的容器。
```

# 9. 附录：MySQL 安装

在该MySQL根目录下创建 **my.ini** 配置文件
```xml
[client]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
[mysqld]
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=C:\\Program Files\\JavaDevelopmentTools\\mysql-8.0.15-winx64
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
# datadir=
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

cmd 进入MySQL根目录
```
cd C:\Program Files\JavaDevelopmentTools\mysql-8.0.15-winx64
cd bin
```

初始化数据库：
```
mysqld --initialize --console
```

执行完成后，会输出 root 用户的初始默认密码
```
2020-03-12T12:56:28.384344Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: ifwtm%))p7qZ
```

输入以下安装命令：
```
mysqld install
```

启动输入以下命令即可：
```
net start mysql
```

登录MySQL：当 MySQL 服务已经运行时, 我们可以通过 MySQL 自带的客户端工具登录到 MySQL 数据库中, 首先打开命令提示符, 输入以下格式的命名:
```
mysql -h 主机名 -u 用户名 -p
```
一般输入这个即可：
```
mysql -u root -p
```

修改密码为 whoami
```
alter user user() identified by "whoami";
```
输入```quit;```可以退出。

查看数据库文件存储位置：
```
show variables like 'datadir';
```

添加用户
```
create user abadstring@localhost identified by 'abadstring';
```

这样就创建了一个在本地，名为 abadbtring，密码为 abadstring的用户。
*注意：此处的"localhost"，是指该用户只能在本地登录，不能在另外一台机器上远程登录。如果想远程登录的话，将"localhost"改为"%"，表示在任何一台电脑上都可以登录。也可以指定某台机器可以远程登录。*

查看所有用户
```
select User, Host from mysql.user;
```

给用户授权
```
grant all privileges on mybatis.* to "abadstring"@"localhost";
```

上面命令授予用户 abadstring 在本地(localhost) 对于数据库 mybatis 的所有表(*) 的全部权限(all privileges)。
```
GRANT privileges ON databasename.tablename TO 'username'@'host';
```
privileges – 用户的操作权限,如SELECT , INSERT , UPDATE  等(详细列表见该文最后面).如果要授予所 的权限则使用ALL说明: 
databasename –  数据库名
tablename-表名,如果要授予该用户对所有数据库和表的相应操作权限则可用* 表示, 如*.*

## 9.1. 常见问题

**Connection error! ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client**
```sql
use mysql;
alter user 'root'@'localhost' identified with mysql_native_password by '123456';
flush privileges;
```