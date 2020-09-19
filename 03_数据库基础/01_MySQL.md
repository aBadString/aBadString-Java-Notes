<h1 id="MySQL" align="center">MySQL</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. 数据库基础和SQL语言](#1-数据库基础和sql语言)
  - [1.1. show 命令](#11-show-命令)
  - [1.2. select 检索数据](#12-select-检索数据)
    - [1.2.1. order by 子句](#121-order-by-子句)
  - [1.3. where 过滤数据](#13-where-过滤数据)
  - [1.4. 函数](#14-函数)
  - [1.5. 聚集函数](#15-聚集函数)
  - [1.6. group by 分组数据](#16-group-by-分组数据)
    - [1.6.1. having 过滤分组](#161-having-过滤分组)
  - [1.7. SQL 执行顺序](#17-sql-执行顺序)
    - [1.7.1. jion 子句](#171-jion-子句)
      - [1.7.1.1. join 连接表，作笛卡尔积](#1711-join-连接表作笛卡尔积)
      - [1.7.1.2. on 筛选条件](#1712-on-筛选条件)
      - [1.7.1.3. outer 外连接](#1713-outer-外连接)
    - [1.7.2. where 子句](#172-where-子句)
    - [1.7.3. group by 子句](#173-group-by-子句)
      - [1.7.3.1. having](#1731-having)
    - [1.7.4. select 选择列](#174-select-选择列)
      - [1.7.4.1. 计算 select 选择列的表达式](#1741-计算-select-选择列的表达式)
      - [1.7.4.2. distinct 执行去重](#1742-distinct-执行去重)
    - [1.7.5. order by 排序](#175-order-by-排序)
    - [1.7.6. limit 子句](#176-limit-子句)
- [2. MySQL 体系结构](#2-mysql-体系结构)
  - [2.1. Innodb存储引擎缓冲池](#21-innodb存储引擎缓冲池)
- [3. 锁](#3-锁)
  - [3.1. InnoDB 存储引擎中的锁](#31-innodb-存储引擎中的锁)
  - [3.2. 锁的算法](#32-锁的算法)
  - [3.3. 锁问题](#33-锁问题)
  - [3.4. 阻塞](#34-阻塞)
  - [3.5. 死锁](#35-死锁)
  - [3.6. 锁升级](#36-锁升级)
- [4. 事务](#4-事务)
  - [4.1. 四大特性](#41-四大特性)
  - [4.2. 事务的分类](#42-事务的分类)
  - [4.3. 事务隔离级别](#43-事务隔离级别)
- [5. MySQL 索引](#5-mysql-索引)
  - [5.1. B+ 树索引](#51-b-树索引)
    - [5.1.1. 聚集索引](#511-聚集索引)
    - [5.1.2. 辅助索引 (非聚集索引)](#512-辅助索引-非聚集索引)
    - [5.1.3. 联合索引](#513-联合索引)
      - [5.1.3.1. MySQL联合索引，为什么范围查找不能使用索引](#5131-mysql联合索引为什么范围查找不能使用索引)
  - [5.2. 全文索引](#52-全文索引)
  - [5.3. 哈希索引](#53-哈希索引)
- [6. 索引优化](#6-索引优化)
  - [6.1. 单列索引](#61-单列索引)
  - [6.2. 多列索引](#62-多列索引)
- [7. 其他](#7-其他)

<!-- /code_chunk_output -->

# 1. 数据库基础和SQL语言

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


**候选码：** 若关系中的某一属性组的值能唯一地标识一个元组，而其子集不能，则称该属性组为候选码。
**主码：**若一个关系中有多个候选码，则选定其中一个为主码。
**主属性：** 所有候选码的属性称为主属性。
**非主属性：**不包含在任何候选码中的属性称为非主属性。

**数据库六大范式：**
- 第一范式：每个列都不可以再拆分。
- 第二范式：在第一范式的基础上，非主属性必须完全依赖于候选码（消除非主属性对主属性的部分依赖）
- 第三范式：在第二范式的基础上，任何非主属性不依赖于其它非主属性（消除传递依赖）
- BC 范式：在第三范式的基础上，任何主属性不能对候选码子集依赖（消除主属性对于候选码的部分与传递函数依赖）
- 第四范式：在BC范式的基础上，限制关系模式的属性之间不允许有非平凡且非函数依赖的多值依赖。
- 第五范式：在第四范式的基础上，表必须可以分解为较小的表，除非那些表在逻辑上拥有与原始表相同的主键。


学生表（学号，姓名，年龄，班主任，班主任办公室）
满足 2NF，不满足 3NF： 学号 -> 班主任 -> 班主任办公室 

修改：
学生表（学号，姓名，年龄，班主任)
班主任表 (班主任，班主任办公室)


## 1.1. show 命令

登录 mysql：
```shell
mysql -u root -p
```

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

![image-20200331150351139](/images/image-20200331150351139.png)


## 1.2. select 检索数据

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

**limit：限制返回前几行数据**
```sql
-- 返回前 5 行数据
select user_name from t_user limit 5;
select user_name from t_user limit 0, 5;
-- 从行号 2 开始后面 5 行数据 （行数是 从 0 开始数的）
select user_name from t_user limit 2, 5;
```
当行数不够时，只会返回已有的那么多行；当行号越界时，返回 零行数据

**完全限定的表名**
```sql
select user_name from mybatis.t_user;
```
当没有执行 use mybatis时，可以使用完全限定的表名。


### 1.2.1. order by 子句
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


## 1.3. where 过滤数据

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

## 1.4. 函数

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


## 1.5. 聚集函数

COUNT() 返回某列的行数（COUNT(*)返回数据行数，COUNT(列名)会忽略值为 NULL 的行）
AVG() 返回某列的平均值（忽略值为 NULL 的行）
MAX() 返回某列的最大值（忽略值为 NULL 的行）
MIN() 返回某列的最小值（忽略值为 NULL 的行）
SUM() 返回某列值之和（忽略值为 NULL 的行）

```sql
--  计算填写了手机号的用户人数
select count(mobile) from account;
```


## 1.6. group by 分组数据

- group by 必须在 where 子句之后，order by 子句之前
- group by 的每个列都必须是检索列或者有效表达式，不能是聚集函数
- select 语句中的列只能是出现在 group by 子句中的列或者是聚集函数；如果 select使用表达式作为列，则 group by 子句必须出现相同的表达式，不能使用别名
- 分组列中的 NULL 值会被分为一组

### 1.6.1. having 过滤分组

having 支持所有 where 的操作。
having 用于过滤分组，可以使用聚集函数，而 where 过滤行，不能使用聚集函数。
where 在分组前过滤数据，having 在分组后过滤数据。

不要忘记 ORDER BY 一般在使用 GROUP BY 子句时，应该也给出 ORDER BY 子句。这是保证数据正确排序的唯一方法。千万不要仅依赖 GROUP BY 排序数据。


## 1.7. SQL 执行顺序

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

### 1.7.1. jion 子句

#### 1.7.1.1. join 连接表，作笛卡尔积
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

#### 1.7.1.2. on 筛选条件
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

#### 1.7.1.3. outer 外连接

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

### 1.7.2. where 子句

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

### 1.7.3. group by 子句

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

#### 1.7.3.1. having

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

### 1.7.4. select 选择列

#### 1.7.4.1. 计算 select 选择列的表达式

#### 1.7.4.2. distinct 执行去重


### 1.7.5. order by 排序

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


### 1.7.6. limit 子句

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

# 2. MySQL 体系结构

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

![image-20200328145525223](/images/image-20200328145525223.png)


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


## 2.1. Innodb存储引擎缓冲池

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


# 3. 锁

lock 的对象是事务，用来锁定数据库中的对象。如：表、页、行。一般 lock 的对象仅在事务 commit 或 rollback 后释放。有死锁检测机制。
latch 称为闩锁（轻量级的锁），它要求锁定时间必须非常短，否则性能会很差。latch 分为 mutex (互斥锁) 和 rwlock (读写锁)。没有死锁检测机制。
![image-20200328151857549](/images/image-20200328151857549.png)

## 3.1. InnoDB 存储引擎中的锁

- 共享锁 (S Lock)：允许事务读一行数据。
- 排它锁 (X Lock)：允许事务删除或更新一行数据。

X 锁与任何锁都不兼容，S 锁之间兼容。S 和 X 都是行锁。

![image-20200328173003024](/images/image-20200328173003024.png)

- 意向共享锁 (IS Lock)：事务想要获得一张表中某几行的共享锁。
- 意向排它锁 (IX Lock)：事务想要获得一张表中某几行的排它锁 。

意向锁之间相互兼容，IS 与 S 兼容

![image-20200328173149700](/images/image-20200328173149700.png)


**一致性非锁定读**
如果读取的行正在被修改或删除， InnoDB 存储引擎回去读取行的一个之前版本的快照数据（undo段）。
- Read Committed 事务隔离级别：总是读取行的最新版本。
- Repeatable Read  事务隔离级别：总是读取事务开始时的版本。

**一致性锁定读**
- select ... for update：加 X 锁，其他事务不能对该行加任何锁。
- select ... lock in share mode：加 S 锁，其他事务只能加 S 锁，加 X 锁会被阻塞。

**自增长与锁**
-INC Locking 锁是一种特殊的表锁，锁是在完成了自增长值的插入后被释放。

**外键和锁**
对于一个外键列，如果没有显示的给这个列加索引，InnoDB 引擎会自动加一个索引。这样做可以避免表锁。

## 3.2. 锁的算法

**行锁的三种算法**
- Record Lock：单个行记录上的锁。
- Gap Lock：间隙锁，锁定一个范围，但不包括当前行。
- Next-Key Lock： Record Lock + Gap Lock，锁定一个范围，包括当前行。

显示地关闭 Gap Lock:
1. 将事务的隔离级别设置为 Read Committed 
2. 将参数 innodb_locks_unsafe_for_binlog 设置为 1

**Phantom Problem 幻想问题**
Phantom Problem 是指在同一事务中，连续执行两次同样的 SQL 语句可能导致不同的结果，第二次的 SQL 语句可能会返回之前不存在的行。

## 3.3. 锁问题

**脏读**
脏数据：事务对缓冲池中行记录的修改，还没有被提交。
脏读：在不同事务下，当前事务可以读取到另一个事务未提交的数据。发生在事务隔离级别为 Read Uncommitted 时。

**不可重复读**
在一个事务中，两次相同的操作所读取到的数据不一样。发生在事务隔离级别为 Read Committed 时。

**丢失更新**
一个事务的更新操作会被另一个事务的更新操作覆盖掉。

## 3.4. 阻塞

阻塞：有时一个事务的锁需要等待另一个事务的锁释放它所占用的资源。

InnoDB 存储引擎中参数 innodb_lock_wait_timeout 用来控制等待时间（默认 50 秒）；参数 innodb_rollbck_on_timeout 用来设定是否在 等待超时时对进行中的事务进行回滚操作（默认 OFF 不回滚）。

innodb_lock_wait_timeout 是动态的，可以在数据库运行时调整；innodb_rollbck_on_timeout 是静态的，不可以在启动是修改。

必须在事务抛出异常时，进行 commit 或者 rollback，否则会十分危险。

## 3.5. 死锁

死锁：两个或者两个以上的事务在执行过程中，因为争夺锁而造成的一种相互等待的现象。若无外力作用，事务将无法推进下去。
线程死锁：两个或者两个以上的线程相互等待对方无法释放的资源，这种等待在无外力的作用下不可接触。

解决死锁：
1. 超时法：当等待时间超过某个值时，一个事务回滚，另一个事务继续执行。
2. 等待图法 wait-for graph：
   数据库需要保存以下两种信息：
   - 锁的信息链表
   - 事务等待链表
   通过上述链表可以构成一张图，若图中存在回路，就代表存在死锁。

## 3.6. 锁升级

将多个粒度较小的锁升级为粒度更大的锁。
![image-20200329231635090](/images/image-20200329231635090.png)

InnoDB 存储引擎不存在锁升级的问题。
![image-20200328195006465](/images/image-20200328195006465.png)


# 4. 事务

事务是区别数据库和文件系统的重要特性之一。
事务会把数据库从一种一致状态转换为另一种一致状态。在数据库提交工作时，可以确保要么所有修改都已经保存了，要么所有修改都不保存。

## 4.1. 四大特性

InnoDB 存储引擎中的事务符合：
- 原子性：整个事务是不可分割的工作单位。要么都完成，要么都不完成。
- 一致性：事务将数据库从一种一致状态转变为另一种一致状态。
- 隔离性 (并发控制、可串行化、锁)：一个事务提交之前对其他事务是不可见的。
- 持久性：事务一旦提交，其结果是永久性的。持久性保证事务系统的高可靠性。

事务是由一条或者一组 SQL 语句组成的，事务是访问并更新数据库中各种数据项的一个程序执行单元。

## 4.2. 事务的分类

- 扁平事务
  在扁平事务中，所有的操作都处于一个层次，由 begin work 开始，commit work 或者 rollback work 结束，期间操作是原子的，要么都执行，要么都回滚。
  ![image-20200328200611471](/images/image-20200328200611471.png)

- 带有保存点的扁平事务
  支持扁平事务的一切操作，还新加了保存点，允许事务回滚到保存点。保存点用 save work 函数来创建。

- 链事务
  在提交一个事务时，释放不需要的数据对象，将必要的处理上下文隐式地传给下一个要开始的事务（提交事务和开始下一个事务 合并为一个原子操作）。
  ![image-20200328201451756](/images/image-20200328201451756.png)

- 嵌套事务
  由一个顶层事务控制着各个层次的事务。
  ![image-20200328201545774](/images/image-20200328201545774.png)
  ![image-20200328201758345](/images/image-20200328201758345.png)

- 分布式事务
  通常是在一个分布式环境下允许的扁平事务，需要根据数据所在位置访问网络中 的不同节点。

## 4.3. 事务隔离级别

未提交读 Read uncommitted：如果一个事务读取到了另一个未提交的事务修改过的数据。
已提交读 Read committed：一个事务能读到另一个已经提交的事务修改过的数据。
可重复读 Repeatable read：一个事务只会读取该事务开始时的记录快照版本。
串行化 serializable：不允许 读-写、写-读 的并发操作。
 Read committed 每次读取数据前都生成一个ReadView
 Repeatable read 在第一次读取数据时生成一个ReadView

**redo log 与 undo log**

redo log：重做日志，用来实现事务的持久性。该日志文件由 重做日志缓冲(redo log buffer) 和 重做日志(redo log)组成，前者存在内存中，后者存在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中。
- 特点 mysql 为了提升性能不会把每次的修改都实时同步到磁盘，而是会先存到Buffer Pool(缓冲池)里头，把这个当作缓存来用。然后使用后台线程去做缓冲池和磁盘之间的同步。
- 问题 引入了redo log来记录已成功提交事务的修改信息，并且会把redo log持久化到磁盘，系统重启之后在读取redo log恢复最新数据。
- 作用 redo log是用来恢复数据的 用于保障已提交事务的持久化特性。

undo log：回滚日志，用于记录数据被修改前的信息。他正好跟前面所说的重做日志所记录的相反，重做日志记录数据被修改后的信息。undo log主要记录的是数据的逻辑变化，为了在发生错误时回滚之前的操作，需要将之前的操作都记录下来，然后在发生错误时才可以回滚。
- 特点 每次写入数据或者修改数据之前都会把修改前的信息记录到 undo log。
- 问题 undo log 记录事务修改之前版本的数据信息，因此假如由于系统错误或者rollback操作而回滚的话可以根据undo log的信息来进行回滚到没被修改前的状态。
- 作用 undo log是用来回滚数据的用于保障未提交事务的原子性

# 5. MySQL 索引

索引的优点：
1. 索引大大减少了服务器需要扫描的数据量
2. 索引可以帮助服务器避免排序和临时表
3. 索引可以将随机 I/O 变为顺序 I/O

## 5.1. B+ 树索引

**二分查找法**
将一组数据先升序排序，然后每次比较中点位置的值；若大于则在右半区间查找，否则在左半区间查找。

**二叉查找树**
- 二叉查找树中，左孩子的值总是小于父亲的值，右孩子的值总是大于父亲的值。
二叉查找树可能会不平衡，变成链表。

**平衡二叉树**
- 符合二叉查找树
- 必须满足任何节点的两个子树的高度之差小于等于1
维护一棵平衡二叉树，需要经常性的旋转操作，开销较大。

**B+ 树**

由 B 树和索引顺序访问方法 (ISAM) 演化而来。
最下面一层用于存放数，叫做 Leaf Page；上面几层存放索引，叫做 Index Page。 Leaf Page 之间通过双向链表连接起来。
![image-20200328222016449](/images/image-20200328222016449.png)

**插入操作：**平衡扩张

1. Leaf 未满，Index 未满：
   直接将记录插入到叶子节点的相应位置中。

2. Leaf 满了，Index 未满：
   需要拆分 Leaf Page。将目标页的中间节点放入 Index 中。小于中间节点的放左边 Page；大于等于的放入右边 Page。

3. Leaf 满了，Index 满了：
   需要拆分 Leaf Page 和 Index Page。先按 2 操作，然后再防止 中间节点。将Index Page 的中间节点放入上一层的 Index Page。小于中间节点的放左边 Page；大于等于的放入右边 Page。

![image-20200328214513454](/images/image-20200328214513454.png)

201页，edge的框上输入。

**旋转操作：**
当前Leaf Page满了，而其**左右**兄弟 Leaf Page 未满时，进行旋转操作。
将记录移动到其兄弟节点上，并重新更新 Index Page。

**删除操作：**
使用填充因子来控制删除操作，填充因子最小值是 50%。
填充因子 = 有数的节点数目 / 总节点数目。

1. Leaf 不小于 填充因子，Index 不小于 填充因子
   1.1 要删除的数据不是第一个，直接删除
   1.2 要删除的数据是第一个，删除后，更新 Index Page
2. ...... 没看懂
![image-20200328220614492](/images/image-20200328220614492.png)
204页，edge的框上输入。

**InnoDB 和 MyISAM 的索引**

在数据库中 B+ 树的高度一般在 2 — 4 层。
MyISAM的索引方式也叫做“非聚集”的，之所以这么称呼是为了与InnoDB的聚集索引区分。
![image-20200426130649578](/images/image-20200426130649578.png)
![image-20200426130711071](/images/image-20200426130711071.png)

### 5.1.1. 聚集索引

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

### 5.1.2. 辅助索引 (非聚集索引)

辅助索引的叶子节点不包括行记录的全部数据，包含一个键值和书签。
当使用辅助索引来查询数据时，会先通过辅助索引找到指向聚集索引的主键，然后再通过聚集索引来找到完整的记录。

### 5.1.3. 联合索引

索引匹配的最左原则具体是说，假如索引列分别为A，B，C，顺序也是A，B，C：
- 那么查询的时候，如果查询【A】【A，B】 【A，B，C】，那么可以通过索引查询
- 如果查询的时候，采用【A，C】，那么C这个虽然是索引，但是由于中间缺失了B，因此C这个索引是用不到的，只能用到A索引
- 如果查询的时候，采用【B】 【B，C】 【C】，由于没有用到第一列索引，不是最左前缀，那么后面的索引也是用不到了
- 如果查询的时候，采用范围查询，并且是最左前缀，也就是第一列索引，那么可以用到索引，但是范围后面的列无法用到索引

**[建立索引的常用技巧](https://juejin.im/post/5a6873fbf265da3e393a97fa)**
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

**只要列涉及到运算和函数，MySQL就不会使用索引**

```sql
SELECT FROM users WHERE YEAR(adddate) < 2007
```

#### 5.1.3.1. MySQL联合索引，为什么范围查找不能使用索引
![](/images/范围查找不能使用索引.png)
![](/images/索引使用总结.jpg)


## 5.2. 全文索引

MyISAM 存储引擎支持全文索引，用于查找文本中的关键词，而不是直接比较是否相等。
查找条件使用 MATCH AGAINST，而不是普通的 WHERE。
全文索引使用倒排索引实现，它记录着关键词到其所在文档的映射。

InnoDB 存储引擎在 MySQL 5.6.4 版本中也开始支持全文索引。

## 5.3. 哈希索引

哈希索引查找复杂度为 $O(1)$
哈希索引是自适应的，InnoDB 会根据需要添加，不能人为干预。

## 5.4. 空间数据索引(R-Tree)

MyISAM表支持空间索引，可以用作地理数据存储。


# 6. 索引优化

## 6.1. 单列索引

主要要注意索引列不能作为表达式的一部分参与运算，或者做为函数的参数，否则不会走索引。

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

## 6.2. 多列索引

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



# 7. 其他

**触发器**

在MySQL数据库中有如下六种触发器：
- 1、Before Insert
- 2、After Insert
- 3、Before Update
- 4、After Update
- 5、Before Delete
- 6、After Delete

**FLOAT和DOUBLE的区别是什么？**
- FLOAT类型数据可以存储至多8位十进制数，并在内存中占4字节。
- DOUBLE类型数据可以存储至多18位十进制数，并在内存中占8字节。

INT
tinyint smallint mediumint int bigint
1       2        3         4   8 字节

float double decimal
varchar char text blobvarchar

**varchar(50)中50的含义**
最多存放50个字符，varchar(50)和(200)存储hello所占空间一样，但后者在排序时会消耗更多内存，因为order by col采用fixed_length计算col长度(memory引擎也一样)。

**int(20)中20的含义**
是指显示字符的长度，不影响内部存储，只是当定义了ZEROFILL时，前面补多少个 0


**MySQL的连接和关闭：mysql -u -p -h -P**

> -u：指定用户名    -p：指定密码    -h：主机    -P：端口

**进入MySQL命令行后：G、c、q、s、h、d**

> G：打印结果垂直显示
>
> c：取消当前MySQL命令
>
> q：退出MySQL连接
>
> s：显示服务器状态
>
> h：帮助信息
>
> d：改变执行符


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
- InnoDB数据存储在共享表空间，MyISAM数据存储在文件中；
- InnoDB支持行级锁，MyISAM只支持表锁；
- InnoDB支持崩溃后的恢复，MyISAM不支持；
- InnoDB支持外键，MyISAM不支持；
- InnoDB不支持全文索引，MyISAM支持全文索引；

**innodb引擎的特性**
- 插入缓冲（insert buffer)
- 二次写(double write)
- 自适应哈希索引(ahi)
- 预读(read ahead)


**索引的使用场景：**
- 对于非常小的表，大部分情况下全表扫描效率更高。
- 中到大型表，索引非常有效。
- 特大型的表，建立和使用索引的代价会随之增大，可以使用分区技术来解决。

**索引的类型：**索引很多种类型，是在MySQL的存储引擎实现的。
- 普通索引：最基本的索引，没有任何约束限制。
- 唯一索引：和普通索引类似，但是具有唯一性约束。
- 主键索引：特殊的唯一索引，不允许有空值。

**索引的区别：**-一个表只能有一个主键索引，但是可以有多个唯一索引。
- 主键索引一定是唯一索引，唯一索引不是主键索引。
- 主键可以与外键构成参照完整性约束，防止数据不一致。
- 联合索引：将多个列组合在一起创建索引，可以覆盖多个列。（也叫复合索引，组合索引）
- 外键索引：只有InnoDB类型的表才可以使用外键索引，保证数据的一致性、完整性、和实现级联操作（基本不用）。
- 全文索引：MySQL自带的全文索引只能用于MyISAM，并且只能对英文进行全文检索 （基本不用）

![image-20200426183030831](/images/image-20200426183030831.png)


**数据完整性**
- 实体完整性：实体完整性在MySQL中的实现是通过主键约束和候选键约束实现的。
- 参照完整性：外键
- 约束完整性：列的约束

**MyISAM索引与InnoDB索引的区别？**
- InnoDB索引是聚簇索引，MyISAM索引是非聚簇索引。
- InnoDB的主键索引的叶子节点存储着行数据，因此主键索引非常高效。
- MyISAM索引的叶子节点存储的是行数据地址，需要再寻址一次才能得到数据。
- InnoDB非主键索引的叶子节点存储的是主键和其他带索引的列数据，因此查询时做到覆盖索引会非常高效。

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


**有A(id,sex,par,c1,c2),B(id,age,c1,c2)两张表，其中A.id与B.id关联，现在要求写出一条SQL语句，将B中age>50的记录的c1,c2更新到A表中同一记录中的c1,c2字段中** **考点分析**

```sql
UPDATE A,B SET A.c1 = B.c1, A.c2 = B.c2 WHERE A.id = B.id and B.age > 50;
UPDATE A INNER JOIN B ON A.id = B.id SET A.c1 = B.c1,A.c2 = B.c2 WHERE B.age > 50
```

https://juejin.im/entry/5b57ec015188251aa8292a69
