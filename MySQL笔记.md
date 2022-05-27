# MySQL数据库

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
   
   ![](https://xiaohualiyuntuchuang.oss-cn-hangzhou.aliyuncs.com/img/202205272017368.png)
   
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

![](https://xiaohualiyuntuchuang.oss-cn-hangzhou.aliyuncs.com/img/202205272057199.png)

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







#### 1.2.5、DQL









#### 1.2.6、DCL

