# MySQL数据库管理系统

## 0、引言：

### 0.1、学什么

1. SQL语言
2. 事务
3. 存储引擎
4. 索引
5. SQL优化
6. 锁
7. 日志
8. 主从复制
9. 读写分离
10. 分库分表

### 0.2、学习路线

1. 基础篇
   - MySQL概述
   - SQL
   - 函数
   - 约束
   - 多表查询
   - 事务
2. 进阶篇
   - 储存引擎
   - 索引
   - SQL优化
   - 视图/存储过程/触发器
   - 锁
   - InnoDB核心
   - MySQL管理
3. 运维篇
   - 日志
   - 主从分离
   - 分库分表
   - 读写分离

## 1、基础篇

### 1.1、MySQL概述

- 数据库相关概念

  | 名称           | 全称                                                         | 简介                             |
  | -------------- | ------------------------------------------------------------ | -------------------------------- |
  | 数据库         | 存储数据的仓库，数据是有组织的进行存储                       | DataBase(DB)                     |
  | 数据库管理系统 | 操纵和管理数据库的大型软件                                   | DataBase Management System(DBMS) |
  | SQL            | 操作关系型数据库的编程语言，定义了一套操作关系型数据库的统一标准 | Structured Query Language(SQL)   |

- MySQL数据库

   下载：

   下载地址：https://dev.mysql.com/downloads/mysql/
   
   ![](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260922930.png)
   
   数据模型：
   
   关系型数据库(RDBMS)
   
   概念：建立在关系型模型基础上，由多张表相互连接的二维表组成的数据库



### 1.2、SQL

#### 1.2.1、SQL通用语法

1. SQL语句可以单行或者多行书写，以分号结尾。
2. SQL语句可以使用空格缩进来增强语句的可读性。
3. MySQL数据库SQL语句不区分大小写，关键字建议使用大写。
4. 注释：
   - 单行注释：-- 注释内容 或 # 注释内容
   - 多行注释：/* 注释内容 */

#### 1.2.2、SQL分类

| 分类 | 全称                     | 说明                                                     |
| ---- | ------------------------ | -------------------------------------------------------- |
| DDL  | Data Definition Language | 数据定义语言，用来定义数据库对象(数据库，表，字段)       |
| DML  | Data Manipulate Language | 数据库操作语言，用来对数据库表中的数据进行增删改         |
| DQL  | Data Query Language      | 数据库查询语言，用来查询数据库中表的记录                 |
| DCL  | Data Control Language    | 数据库控制语言，用来创建数据库用户，控制数据库的访问权限 |

#### 1.2.3、DDL

##### 1、DDL-操作数据库

1. 查询

   查询所有数据库

   ```mysql
   show database;
   ```

   查询当前处于哪个数据库之中

   ```mysql
   select database();
   ```

2. 创建

   ```mysql
   create database [if not exists] 数据库名称 [default charset 字符集][collate 排序规则]
   ```

3. 删除

   ```mysql
   drop database [if exists] 数据库名称;
   ```

4. 使用

   ```mysql
   use 数据库名称;
   ```



##### 2、DDL-操作表

1. 查询当前数据库中所有表

   ```sql
   show tables;
   ```

2. 查询表结构

   ```sql
   desc 表名;
   ```

3. 查询指定表的建表语句

   ```sql
   show create table 表名;
   ```

4. 创建表

   ```sql
   create table 表名(
   	字段1 字段1类型[comment 字段1注释],
     字段2 字段2类型[comment 字段2注释],
     字段3 字段3类型[comment 字段3注释],
     ...
     字段n 字段n类型[comment 字段n注释]
   )[comment 表注释];
   ```

##### 3、数据类型

![](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260923055.png)

(一个字节对应八个二进制位)

##### 4、DDL-修改-删除

alter--使什么改变

1. 添加字段

   ```sql
   alter table 表名 add 字段名 类型(长度) [comment 注释][约束];
   -- 为emp表添加一个新的字段：名称为‘nickname’，类型为varchar(20)
   alter table emp add nick_name varchar(20) comment '昵称';
   ```

2. 修改字段

   修改数据类型(modify--调整)

   ```sql
   alter table 表名 modify 字段名 新数据类型(长度);
   ```

   修改字段名和字段类型(change--改变)

   ```sql
   alter table 表名 change 旧字段名 新字段名 类型(长度) [comment 注释] [约束];
   ```

   ```sql
   # 将emp表的nickname字段修改为username,类型为varchar(30)
   alter table emp change nickname username varchar(30) comment '用户名';
   ```

3. 删除字段

   ```sql
   alter table 表名 drop 字段名;
   # 删除emp表中的username字段
   alter table emp drop username;
   ```

4. 修改表名

   ```sql
   alter table 表名 rename to 新表名;
   # 将emp表的表名修改为employee;
   alter table emp rename to employee;
   ```

5. 删除表

   ```sql
   drop table [if exists] 表名;
   # 删除employee表
   drop table if exists employee;
   ```

   删除指定表，并重新创建该表(清除表中的所有数据)

   ```sql
   truncate table 表名;
   ```

   

#### 1.2.4、DML

1. 添加数据(INSERT)
2. 修改数据(UPDATE)
3. 删除数据(DELETE)

##### 1、DML-添加数据

1. 给指定字段添加数据

   ```sql
   insert into 表名(字段1,字段2...) values (值1,值2...)
   ```

2. 给所有字段赋值

   ```sql
   insert into 表名 values (值1,值2...)
   ```

3. 批量添加数据

   ```sql
   insert into 表名(字段1,字段2...) values (值1,值2...),(值1,值2...),(值1,值2...);
   
   insert into 表名 values (值1,值2...),(值1,值2...),(值1,值2...);
   ```

   ```sql
   #创建employee表
   create table employee(
       id int comment '编号',
       name varchar(10) comment '姓名',
       gender char(1) comment '性别',
       age tinyint comment '年龄',
       id_card char(18) comment '二代身份证18位',
       entrydate date comment '日期'
   );
   
   #把年龄字段修改为无符号数
   alter table employee change age age tinyint unsigned comment '年龄';
   
   #添加数据
   insert into employee (id, name, gender, age, id_card, entrydate)
   values (1,'张三','男',18,'123456789012345678','2022-01-01');
   #查询数据
   select *
   from employee;
   
   #测试年龄为负数可不可以插入
   # insert into employee (id, name, gender, age, id_card, entrydate)
   # values (2,'ww','男',-1,'123456789012345678','2022-01-01');
   #不指定字段名
   insert into employee values (2,'李四','男',18,'123456789442345678','2022-01-03');
   
   #批量添加数据(不指定字段名)
   insert into employee
   values (3,'王五','男',18,'123456789332345678','2022-01-03'),(4,'red','男',8,'123456780042345678','2022-03-03'),(5,'andy','男',8,'123456789442300678','2022-03-03');
   
   
   ```

   

##### 2、DML-修改数据

```sql
update 表名 set 字段名1 = 值1,字段名2 = 值2,...[where 条件];
```

```sql
#修改数据
#修改id为1的名称为itheima
update employee set name = 'itheima' where id = 1;
#修改id为1的数据，name为jack，gender为女
update employee set name = 'jack', gender = '女' where id = 1;
#将所有员工入职日期修改为2022-01-01
update employee set entrydate = '2022-01-01';
```



3、DML-删除数据

```sql
delete from 表名 [where 条件];
```

注意:

- delete语句的条件可有可无，如果没有条件则会删除整张表的所有数据
- delete语句不能删除某一个字段的值(可以使用update)(比如我们删除一个数据的某个字段比如年龄 我们就使用update语句把这个字段修改为null)

```sql
#删除数据
#删除gender为女的数据
delete from employee where gender='女';
-- 删除所有员工
delete
from employee;
```



#### 1.2.5、DQL

- 关键字select

- 语法结构

  ```sql
  select
  	字段列表
  from
  	列表名
  where
  	条件列表
  group by
  	分组字段列表
  having
  	分组后条件列表
  order by
  	排序字段列表
  limit
  	分页参数
  ```

1. 基本查询(select)
2. 条件查询(where)
3. 聚合函数(count、max、min、avg、sum)
4. 分组查询(group by)
5. 排序查询(order by)
6. 分页查询(limit)

##### 1、DQL-基本查询

1. 查询多个字段

   ```sql
   select 字段1,字段2,字段3 ... from 表名;
   select * from 表名;
   
   ```

   <img src="https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260923707.png" style="zoom:50%;" />

   ![](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260923461.png)

2. 设置别名

   ```sql
   select 字段1[as 别名1],字段2[as 别名2] ... from 表名;
   #别名关键字as或者空一格
   select id as '编号',name '姓名' from employee;
   ```

3. 去除重复记录

   ```sql
   select distinct 字段列表 from 表名;
   #去重关键字 distinct
   select distinct age from employee;
   ```

##### 2、DQL-条件查询

1. 语法

   ```sql
   select 字段列表 from 表名 where 条件列表;
   ```

2. 条件

   |    比较运算符    |                 功能                  |
   | :--------------: | :-----------------------------------: |
   |        >         |                 大于                  |
   |        >=        |               大于等于                |
   |        <         |                 小于                  |
   |        <=        |               小于等于                |
   |      <>或!=      |                不等于                 |
   | between...and... |    在某个范围之内(含最小、最大值)     |
   |     in(...)      |     在in之后的列表中的值，多选一      |
   |   like 占位符    | 模糊匹配(_匹配单个字符,%匹配任意字符) |
   |     is null      |                是null                 |

   | 逻辑运算符 |   功能   |
   | :--------: | :------: |
   | and 或 &&  |   并且   |
   | or 或 \|\| |   或者   |
   |  not 或 !  | 非，不是 |

3. 案例

   ```sql
   #查询年龄等于18的员工
   select * from employee where age = 18;
   #查询年龄小于20的员工信息
   select *
   from employee where employee.age < 18;
   #查询没有身份证号的员工信息
   select *
   from employee where id_card is null ;
   #查询有身份证号信息的员工
   select *
   from employee where id_card is not null ;
   #查询年龄不等于88的员工
   select *
   from employee where employee.age <> 88;
   #查询年龄在15到20之间的
   select *
   from employee where employee.age between 15 and 20;
   #查询性别为女且年龄小于25
   select *
   from employee where gender = '男' and employee.age<15;
   #查询年龄等于18或20或40
   select *
   from employee where employee.age = 18 or employee.age = 20 or employee.age = 40;
   
   select *
   from employee where employee.age in (18,20,40);
   #查询姓名为两个字的员工
   select *
   from employee where name like '__';
   #查询姓张的
   select *
   from employee where name like '张%';
   #查询身份证最后一位是X
   select *
   from employee where id_card like '%X';
   ```

##### 3、DQL-聚合函数

1. 介绍

   将一列数据作为整体进行纵向计算

2. 常见的聚合函数

   | 函数  |   功能   |
   | :---: | :------: |
   | count | 统计数量 |
   |  max  |  最大值  |
   |  min  |  最小值  |
   |  avg  |  平均值  |
   |  sum  |   求和   |

3. 语法

   ```sql
   select 聚合函数(字段) from 表名;
   ```

注意：使用聚合函数的时候所有的null值不参与运算

```sql
#求总记录数
select count(*)
from employee;

select count(employee.id) from employee;

#统计平均年龄
select avg(age) as '平均年龄' from employee;

#求最大年龄
select max(age) '最大年龄' from employee;
#求最小年龄
select min(age) '最小年龄' from employee;
#统计入职日期为2022-01-03员工的年龄之和
select sum(age) '2022-01-03的年龄之和' from employee where entrydate = '2022-01-03';

```



##### 4、DQL-分组查询

1. 语法

   ```sql
   select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
   ```

2. where和having区别

   - 执行时机不同，where是在分组前进行过滤(不满足where条件的不参与分组)，而having是在分组后的结果进行过滤
   - 判断条件不同：where不能对聚合函数进行判断，而having可以

```sql
#根据性别分组，统计男性员工和女员工的数量
select gender,count(*) from employee group by gender;
#根据性别分组，统计男和女的平均年龄
select gender, avg(age) from employee group by gender;
#查询年龄小于等于20的员工，并根据入职日期分组，获取员工数量大于等于2的入职日期
select entrydate,count(*) from employee where age<=20 group by entrydate having count(entrydate)>=2
```

注意：

- 执行顺序:where -> 聚合函数 -> having
- **分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义!**



##### 5、DQL-排序查询

1. 语法

   ```sql
   select 字段列表 from 表名 order by 字段1 排序方式1,字段2 排序方式2;
   ```

2. 排序方式

   - ASC升序(默认)
   - DESC降序

注意：如果是多字段排序，当第一个字段值相同时，才会按照第二个字段进行排序

```sql
#根据年龄进行升序排序
select name,age,entrydate from employee order by age;
#根据年龄进行降序排序
select name,age,entrydate from employee order by age desc ;
#根据年龄进行升序排序 当年龄相同时 根据入职日期进行降序排序
select name,age,entrydate from employee order by age,entrydate desc ;
```





##### 6、DQL-分页查询

1. 语句

   ```sql
   select
   	字段列表
   from
   	表名列表
   where
   	条件列表
   group by
   	分组字段列表
   having
   	分组后条件列表
   order by
   	排序字段列表
   limit
   	分页参数
   ```

2. 语法

   ```sql
   select 字段列表 from 表名 limit 起始索引,查询记录;
   ```

3. 注意

   - 起始索引从0开始，起始索引 = (查询页码-1)*每页显示记录数
   - 分页查询是数据库的方言，不同数据库有不同的实现，MySQL中是limit
   - 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10

```sql
#-----------limit------------
select name from employee limit 0,3;
select name from employee limit 3;
select name from employee limit 3,3
```



##### 7、DQL-练习

```sql
#-----------练习------------
#查询年龄分别为8，18，22
select *
from employee where age in (8,18,22);
#查询性别为男，并且年龄在10-40岁以内的姓名为两个字的
select *
from employee where gender = '男' and age between 10 and 40 and name like '__';
#统计员工表中，年龄10-20岁的，男女员工人数
select gender,count(*) from employee where age between 10 and  20 group by gender;
#查询所有年龄小于等于18岁的员工姓名和年龄，并对查询结果进行按照年龄升序排序，如果年龄相同就按照入职日期降序排序
select name,age from employee where age <= 18 order by age,entrydate desc ;
#查询性别为男，且年龄在10-20以内的前2个员工信息，并对查询结果进行按照年龄升序排序，如果年龄相同就按照入职日期降序排序
select *
from employee where gender = '男' and age between 10 and 20 order by age,entrydate desc limit 0,2;

```



##### 8、DQL-执行顺序

编写顺序：

```sql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表
limit
	分页参数
```

执行顺序:

```sql
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
select
	字段列表
order by
	排序字段列表
limit
	分页参数
```





#### 1.2.6、DCL

##### 1、DCL-管理用户

1. 查询用户

   ```sql
   use mysql;
   select * from user;
   ```

2. 创建用户

   ```sql
   create user '用户名'@'主机名' identified by '密码';
   ```

3. 修改用户

   ```sql
   alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';
   ```

4. 删除用户

   ```sql
   drop user '用户名'@'主机名';
   ```

##### 2、DCL-权限控制

1. 查询权限

   ```sql
   show grants for '用户名'@'主机名';
   ```

2. 授予权限

   ```sql
   grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
   ```

3. 撤销权限

   ```sql
   revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
   ```





### 1.3、函数

**函数**是指一段可以直接被另一段程序调用的程序或代码

#### 1.3.1、字符串函数

|           函数           |                    功能                    |
| :----------------------: | :----------------------------------------: |
|    concat(s1,s2...sn)    | 字符串拼接，将是s1,s2...sn拼接成一个字符串 |
|        lower(str)        |         将字符串str全部转换为小写          |
|        upper(str)        |         将字符串str全部转换为大写          |
|     lpad(str,n,pad)      |  左填充，用pad对左侧进行填充，使长度达到n  |
|     rpad(str,n,pad)      |  右填充，用pad对右侧进行填充，使长度达到n  |
|        trim(str)         |        把str的开头和结尾的空格去掉         |
| substring(str,start,len) |   返回str从start开始的len的长度的字符串    |

```sql
select 函数(参数);

-- 案例
-- 由于企业变更原因，在工号前面需要补零，让工号长度为5
update employee set work_number = lpad(work_number,5,'0');
```

#### 1.3.2、数值函数

|    函数    |                功能                |
| :--------: | :--------------------------------: |
|  ceil(x)   |              向上取整              |
|  floor(x)  |              向下取整              |
|  mod(x,y)  |            返回x/y的模             |
|   rand()   |         返回0~1内的随机值          |
| round(x,y) | 返回参数x的四舍五入值，保留y位小数 |

```sql
-- -------------数值函数-------------
/**
|  ceil(x)   |              向上取整              |
| :--------: | :--------------------------------: |
|  floor(x)  |              向下取整              |
|  mod(x,y)  |            返回x/y的模             |
|   rand()   |         返回0~1内的随机值          |
| round(x,y) | 返回参数x的四舍五入值，保留y位小数 |
 */
-- ceil
select ceil(1.5);
-- floor
select floor(1.5);
-- mod(x,y)
select mod(3,4);
select mod(7,4);
-- rand
select rand();
-- round(x,y)
select round(2.345,2);
-- 通过数据库的函数生成一个随机6位的验证码
select lpad(round(rand()*1000000,0),6,0)
```



#### 1.3.3、日期函数

|               函数                |                        功能                         |
| :-------------------------------: | :-------------------------------------------------: |
|             curdate()             |            返回当前日期(current-当前的)             |
|             curtime()             |                    返回当前时间                     |
|               now()               |                 返回当前日期和时间                  |
|            year(date)             |                 返回指定的date的年                  |
|            month(date)            |                 返回指定的date的月                  |
|             day(date)             |                 返回指定的date的日                  |
| date_add(date,interval expr typr) | 返回一个日期/时间值上加上一个时间间隔expr后的时间值 |
|       datediff(date1,date2)       |    返回起始时间date1 和 结束时间date2之间的天数     |

```sql
-- ---------------------date----------------------
/**
|             curdate()             |            返回当前日期(current-当前的)             |
| :-------------------------------: | :-------------------------------------------------: |
|             curtime()             |                    返回当前时间                     |
|               now()               |                 返回当前日期和时间                  |
|            year(date)             |                 返回指定的date的年                  |
|            month(date)            |                 返回指定的date的月                  |
|             day(date)             |                 返回指定的date的日                  |
| date_add(date,interval expr typr) | 返回一个日期/时间值上加上一个时间间隔expr后的时间值 |
|       datediff(date1,date2)       |    返回起始时间date1 和 结束时间date2之间的天数     |
 */
-- curdate
select curdate();
-- curtime
select curtime();
-- now()
select now();
-- year month day
select year(now()),month(now()),day(now());
-- date_add
select date_add(now(),interval 70 day );
select date_add(now(),interval 70 month );
select date_add(now(),interval 70 year );
-- datediff (date1,date2) date1-date2
select datediff('2022-6-1','2022-5-30');

-- 查询员工的入职的天数，并且倒序排序
select name as '姓名',datediff(CURDATE(),entrydate) as '入职时长' from employee order by 入职时长 desc ;
```

#### 1.3.4、流程函数

|                           函数                            |                           功能                           |
| :-------------------------------------------------------: | :------------------------------------------------------: |
|                       if(vaule,t,f)                       |             如果value为true则返回t,否则返回f             |
|                   ifnull(value1,value2)                   |      如果value不为null，返回value1，否则返回value2       |
|    case when [val1] then [res1]... else [default] end     |    如果val1为true，返回res1...，否则返回default默认值    |
| case [expr] when [val1] then [res1]... else [default] end | 如果expr的值等于val1，返回res1...，否则返回default默认值 |

```sql
-- 流程控制函数
-- if
select if(true,'success','error');
-- ifnull
select ifnull('ok','default');
select ifnull('','default');
select ifnull(null,'default');
-- case when then else end
select name,case entrydate when '2022-01-01' then '元旦' when '2022-01-02' then '元旦下一天' else '其他' end  as '身份信息'
from employee;

select name,score,
       case when score >= 85 then '优秀' when score >= 60 then '及格' else '不及格' end as '成绩' from employee order by score desc ;
```



### 1.4、约束

#### 1.4.1、概念

1. 概念：约束是作用于表中字段上的规则，用于限制存储在表中的数据。

2. 目的：保证数据库中数据的正确、有效和完整性

3. 分类：

   |           约束           |                           描述                           |   关键字    |
   | :----------------------: | :------------------------------------------------------: | :---------: |
   |         非空约束         |                限制该字段的数据不能为null                |  not null   |
   |         唯一约束         |         保证该字段的所有数据都是唯一的、不重复的         |   unique    |
   |         主键约束         |         主键是一行数据的唯一标识、要求非空且唯一         | primary key |
   |         默认约束         |      保存数据时，如果未指定该字段的值，则采用默认值      |   default   |
   | 检查约束(8.0.16版本之后) |                保证字段的值满足某一个条件                |    check    |
   |         外键约束         | 用来让两张表的数据之间建立联系，保证数据的一致性和完整性 | foreign key |



#### 1.4.2、约束演示

![image-20220601124722431](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260924576.png)

```sql
-- -------------------------约束演示------------------------------
-- 删除表
drop table if exists user;
-- 创建表
create table user(
    id int primary key auto_increment comment 'id',
    name varchar(10) not null unique comment '姓名',
    age int check ( age between 0 and 120) comment '年龄',
    status char(1) default 1 comment '状态',
    gender char(1) comment '性别'

) comment '用户表';
-- 修改字段
# alter table user change status status char(1) default 1 comment '状态';
-- 插入数据
insert into user (name, age, status, gender) values ('Red',59,'1','男'),('Andy',29,'0','男');
insert into user (name, age, status, gender) values ('Blue',120,'1','男');
insert into user (name, age,  gender) values ('Black3',20,'女');
-- 查询数据
select *
from user;
```



#### 1.4.3、外键约束

创建两张表--员工表和部门表

sql如下：

```sql
-- -------------------约束(外键)----------------------
-- 准备数据--创建表
-- 部门表
create table dept(
    id int primary key auto_increment comment 'ID',
    name varchar(50) not null comment '部门名称'
)comment '部门表';

insert into dept (id, name)
values (1,'研发部'),(2,'市场部'),(3,'财务部'),(4,'销售部'),(5,'总经办');

select *
from dept;

-- 员工表
create table emp(
    id int auto_increment comment 'ID' primary key ,
    name varchar(50) not null comment '姓名',
    age int comment '年龄',
    job varchar(20) comment '职位',
    salary int comment '薪水',
    entry_date date comment '入职时间',
    manager_id int comment '直属领导ID',
    dept_id int comment '部门ID'
)comment '员工表';

insert into emp (name, age, job, salary, entry_date, manager_id, dept_id)
values ('金庸',66,'总裁',20000,'2000-01-01',null,5),
       ('张无忌',50,'项目经理',12500,'2005-02-03',1,1),
       ('杨戬',22,'开发',15000,'2022-02-03',2,1),
       ('二郎神',10,'开发',2500,'2022-04-03',3,1),
       ('孙悟空',66,'开发',12500,'2005-02-03',3,1),
       ('猪八戒',70,'运维',12500,'2005-02-03',3,1);

select *
from emp;
```

1. 语法：

   添加外键

   ```sql
   create table 表名(
   		字段名 数据类型,
     	...
     	[constraint] [外键名称] foreign key(外键字段名) references 主表(主表列名)
   )comment 'xx表';
   ```

   ```sql
   alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名);
   alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);
   ```

   删除外键

   ```sql
   alter table 表名 drop foreign key 外键名称;
   alter table emp drop foreign key fk_emp_dept_id;
   ```

   删除/更新行为

   |    行为     |                             说明                             |
   | :---------: | :----------------------------------------------------------: |
   |  no action  | 当在父表中删除/更新对应的记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。(与restrict一致) |
   |  restrict   | 当在父表中删除/更新对应的记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。(与no action一致) |
   |   cascade   | 当在父表中删除/更新对应的记录时，首先检查该记录是否有对应外键，如果有，则也删除/更新外键在字表中的记录 |
   |  set null   | 当在父表中删除对应的记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null(这就要求外键允许为null) |
   | set default |          父表有变更时，字表将外键列设置成一个默认值          |

   ```sql
   alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名) 
   on update cascade on delete cascade;
   -- 外键更新/删除行为
   alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id)
       on update cascade on delete cascade ;
   ```



### 1.5、多表查询

#### 1.5.1、多表关系

1. 概述

   在项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所以**各个表结构之间也存在着各种联系**，基本上分为三种：

   - 一对多(多对一)
   - 多对多
   - 一对一

2. 一对多(多对一)：

   - 案例：部门 与 员工的关系
   - 关系：一个部门对应多个员工，多个员工对应一个部门
   - 实现：在**多**的一方建立**外键**，指向**一**的一方的**主键**

   ![](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260924635.png)
   
3. 多对多

   - 案例：学生 与 课程 的关系
   - 关系：一个学生可以选修多门课程，一门课程可以供多个学生选择
   - 实现：**建立第三张中间表，中间表至少包含两个外键，分别关联两方的主键**

   ![](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260924270.png)

   ```sql
   -- ------------多对多------------
   -- 学生表
   create table student(
       id int primary key auto_increment comment '主键ID',
       name varchar(10) comment '姓名',
       number varchar(10) comment '学号'
   )comment '学生表';
   
   insert into student (id, name, number)
   values (null,'孙悟空','10001'),(null,'猪八戒','10002'),(null,'沙僧','10003'),(null,'白龙马','10004'),(null,'唐僧','10005'),(null,'如来','10006');
   
   -- 课程表
   create table course(
       id int primary key auto_increment comment '主键ID',
       name varchar(10) comment '课程名称'
   )comment '课程表';
   insert into course (id, name)
   values (null,'Java'),(null,'Python'),(null,'MySQL'),(null,'JavaScript');
   
   -- 中间表
   create table student_course(
       id int auto_increment primary key comment '主键',
       student_id int not null comment '学生ID',
       course_id int not null comment '课程ID',
       constraint fk_course_id foreign key (course_id) references course (id),
       constraint fk_student_id foreign key (student_id) references student (id)
   )comment '学生课程中间表';
   
   insert into student_course (id, student_id, course_id)
   values (null,1,1),(null,1,2),(null,1,3),(null,2,1),(null,3,1),(null,4,1),(null,5,1),(null,6,1);
   
   select *
   from student;
   select *
   from course;
   select *
   from student_course;
   
   ```

   ![](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260924737.png)

4. 一对一

   - 案例：用户 与 用户详情的关系

   - 关系：一对一关系，用于单表拆分。将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率

   - 实现：在任意一方加入外键，关联另一方的主键，并且设置外键为唯一的(unique)

     ![image-20220601144716512](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260925792.png)

     ```sql
     -- -------------一对一-------------
     create table tb_user(
         id int primary key auto_increment comment '主键ID',
         name varchar(10) comment '姓名',
         age int comment '年龄',
         gender char(1) comment '1:男,2:女',
         phone char(11) comment '手机号'
     )comment '用户基本信息';
     
     create table tb_user_edu(
         id int primary key auto_increment comment '主键ID',
         degree varchar(20) comment '学历',
         major varchar(50) comment '专业',
         primaryschool varchar(50) comment '小学',
         middleschool varchar(50) comment '中学',
         university varchar(50) comment '大学',
         userid int unique comment '用户ID',
         constraint fk_userid foreign key (userid) references tb_user (id)
     
     )comment '用户教育信息';
     
     
     insert into tb_user (id, name, age, gender, phone)
     values (null,'1',20,'男','18888888888'),
            (null,'2',21,'男','18888888877'),
            (null,'3',22,'女','18888888866'),
            (null,'4',23,'男','18888888855');
     insert into tb_user_edu (degree, major, primaryschool, middleschool, university, userid)
     values ('本科','舞蹈','北京第一小学','北京第一中学','北京舞蹈学院',1),
            ('本科','舞蹈','上海第一小学','上海第一中学','北京舞蹈学院',2),
            ('本科','舞蹈','江苏第一小学','江苏第一中学','北京舞蹈学院',3),
            ('本科','舞蹈','杭州第一小学','杭州第一中学','北京舞蹈学院',4);
     
     select *
     from tb_user_edu;
     ```

     

#### 1.5.2、多表查询概述

1. 概述：

   指从多张表中查询数据

2. 笛卡尔积:

   指的是在数学中，两个集合A集合和B集合的所有组合情况(在多表查询的时候，要消除无效的笛卡尔积)

   ![image-20220603154004088](https://raw.githubusercontent.com/redyouzi/images-for-blog/main/img02/202208260925058.png)

3. 多表查询分类

   - 连接查询
     - 内连接：相当于查询A、B交集部分数据
     - 外连接：
       - 左外连接：查询**左表**所有数据，以及两张表交集部分数据
       - 右外连接：查询**右表**所有数据，以及两张表交集部分数据
     - 自连接：查询表与自身的连接查询，自连接必须使用表别名



#### 1.5.3、内连接

1. 内连接语法

   - 隐式内连接

     ```sql
     select 字段列表 from 表1,表2 where 条件...;
     -- 隐式内连接
     select emp.name,dept.name from emp,dept where dept_id = dept.id;
     ```

   - 显式内连接

     ```sql
     select 字段列表 from 表1 [inner] join 表2 on 连接条件...;
     -- 显式内连接
     select e.name,d.name from emp e join dept d on e.dept_id = d.id;
     ```

   内连接查询的是两张表交集的部分



#### 1.5.4、外连接













#### 1.5.5、自连接













#### 1.5.6、子查询











#### 1.5.7、多表查询案例







