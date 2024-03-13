---
title: "MySQLTutorial"
date: 2021-08-04T18:34:25+08:00
lastmod: 2021-08-24T22:54:22+08:00
draft: false
categories:
- 数据库
- 技术
tags:
- MySQL
- Getting Started
---

# MySQL Tutorial

MySQL 是最流行的关系型数据库管理系统，在 WEB 应用方面 MySQL 是最好的 RDBMS(Relational Database Management System：关系数据库管理系统)应用软件之一。

所谓的关系型数据库，是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据。

RDBMS 即关系数据库管理系统(Relational Database Management System)的特点：

- 1.数据以表格的形式出现
- 2.每行为各种记录名称
- 3.每列为记录名称所对应的数据域
- 4.许多的行和列组成一张表单
- 5.若干的表单组成database

### RDBMS 术语

在我们开始学习MySQL 数据库前，让我们先了解下RDBMS的一些术语：

- **数据库:** 数据库是一些关联表的集合。
- **数据表:** 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
- **列:** 一列(数据元素) 包含了相同类型的数据, 例如邮政编码的数据。
- **行：**一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
- **冗余**：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
- **主键**：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
- **外键：**外键用于关联两个表。
- **复合键**：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
- **索引：**使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
- **参照完整性:** 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

MySQL 为关系型数据库(Relational Database Management System), 这种所谓的"关系型"可以理解为"表格"的概念, 一个关系型数据库由一个或数个表格组成, 如图所示的一个表格:

![img](https://www.runoob.com/wp-content/uploads/2014/03/0921_1.jpg)

- 表头(header): 每一列的名称;
- 列(col): 具有相同数据类型的数据的集合;
- 行(row): 每一行用来描述某条记录的具体信息;
- 值(value): 行的具体信息, 每个值必须与该列的数据类型相同;
- **键(key)**: 键的值在当前列中具有唯一性。







启动

`systemctl start mysql`

关闭

`systemctl stop mysql`

设置开机启动

`systemctl enable mysql`

```
sudo mysql -u root -p

# no password

service mysql restart

service mysql stop

service mysql start
```



查看状态

`systemctl status mysql`

进入mysql

`sudo mysql`或`sudo mysql -u root -p`

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

退出mysql

`exit`

查看用户

`select user();`



### 验证安装查看版本

在成功安装 MySQL 后，一些基础表会表初始化，在服务器启动后，你可以通过简单的测试来验证 MySQL 是否工作正常。

使用 mysqladmin 工具来获取服务器状态：

使用 mysqladmin 命令来检查服务器的版本, 在 linux 上该二进制文件位于 /usr/bin 目录，在 Windows 上该二进制文件位于C:\mysql\bin 。

`mysqladmin --version`

本机：mysqladmin  Ver 8.42 Distrib 5.7.34, for Linux on x86_64

检验

```shell
sudo netstat -tap | grep mysql

```

通过上述命令检查之后，如果看到有mysql 的socket处于 listen 状态则表示安装成功。

### 密码设置与破解

Mysql安装成功后，默认的root用户密码为空，你可以使用以下命令来创建root用户的密码：

```
sudo mysqladmin -u root password "xxxxxxx";
```

现在你可以通过以下命令来连接到Mysql服务器：

```
sudo mysql -u root -p
Enter password:*******
```

**注意：**在输入密码时，密码是不会显示了，你正确输入即可。



连接到workbench报错[无法在 127.0.0.1:3306 连接到 mysql，用户 'root'@'localhost' 的用户 root 访问被拒绝（使用密码：YES）](https://stackoverflow.com/questions/25777943/failed-to-connect-to-mysql-at-127-0-0-13306-with-user-root-access-denied-for-us)

无法在 127.0.0.1:3306 连接到 mysql，用户 'root'@'localhost' 的用户 root 访问被拒绝（使用密码：YES）

我在 Ubuntu 上安装了新的 mysql，但我将 mysql 密码留空，结果我无法以任何方式连接到 mysql。
最近我透露有一个用户表，其中包含名称、主机、密码和一些插件。因此，对于我的用户 root@localhost mysql 在安装时分配了一个名为**auth_socket**的插件，它让 Unix 用户“root”以没有密码的 mysql 用户“root”身份登录，但不允许以另一个 Unix 用户身份登录。所以要解决这个问题，你应该关闭这个插件并设置通常的身份验证：

1. 打开 Linux 终端
2. 输入“ **sudo mysql** ”，
   您将看到“mysql >”，这意味着您已以“root”Unix 用户身份连接到 mysql，并且可以键入 SQL 查询。
3. 输入 SQL 查询以更改登录方式：
   **ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_new_password';**
   其中 'mysql_native_password' 表示 - 关闭 auth_socket 插件。

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'xxxxxxx';`



2.5mysql忘记密码：

1.停止mysql服务 `net stop mysql`或者相应的进程`pkill -9 mysql`

2.进入mysql下bin目录： `mysqld --skip-grant-tables` （Windows）//跳过验证登录  `mysqld_safe --skip-grant-tables`（Linux）

3.另外窗口打开： `mysql` //直接可用进入系统

4.更改密码

 `use mysql;`

`` update user set password=password('123456') where user='root' and host='localhost';`（Windows）

`update mysql.user set password=password('123456') where user='root' and host='localhost';`（Linux）

刷新下

`flush privileges;`//立即生效

5.注销系统，再进入，开MySQL，使用用户名root和刚才设置的新密码123456登陆

2.6修改密码：

1.你的root用户现在没有密码，你希望的密码修改为123456，那么命令是： `mysqladmin -u root password 123456`

2.如果你的root现在有密码了（123456），那么修改密码为abcdef的命令是： `mysqladmin -u root -p123456 password abcdef`

2.7添加用户： `grant select,insert,update,delete,create,drop on stud.* to user1@localhost identified by "user1"; `//添加用户名user1密码为user1具有插入，更新，删除，创建，删除对于数据库所有表` GRANT ALL PRIVILEGES ON *.* TO 'backlion'@'%' IDENTIFIED BY 'backlion123' WITH GRANT OPTION; grant all privileges on *.* to test@loclhost identified by "test"; `创建主键： `Alter table test add primary key(code,curlum); `//用code和curlum作为一组联合主键来约束









连接远程帐户

`sudo mysql -uroot -p123  -h 127.0.0.1 -P 3306`

#### 解决 MySQL 的 ERROR 1698 (28000): Access denied for user 'root'@'localhost'

```mysql
use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SELECT User, Host, plugin FROM mysql.user;
+------------------+-----------+-----------------------+
| User             | Host      | plugin                |
+------------------+-----------+-----------------------+
| root             | localhost | auth_socket           |
| mysql.session    | localhost | mysql_native_password |
| mysql.sys        | localhost | mysql_native_password |
| debian-sys-maint | localhost | mysql_native_password |
+------------------+-----------+-----------------------+
4 rows in set (0.00 sec)

```



就像你在查询语句中看到的那样，root用户在使用auth_socket插件。1. 你可以设置你的root用户使用mysql_native_password插件 2. 你可以创建一个与你的系统用户一致的新的数据库用户（推荐）（笔者注：方法2即满足auth_socket插件的要求）

报错原因：安装数据库，在安装的过程中未设置密码。在连接数据库，由于没有设置密码，所以在需要输入密码的时候，直接按了Enter键，导致该错误的出现。

解决方法1：使用sudo权限（不推荐）
sudo mysql -u root -p

解决方法2：设置密码（推荐）
步骤一：sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf


保存并重启mysql

步骤二：修改密码
步骤三：再次修改 mysqld.cnf 文件参见步骤一，修改如下：注释掉 skip-grant-tables

## 登录 MySQL



当 MySQL 服务已经运行时, 我们可以通过 MySQL 自带的客户端工具登录到 MySQL 数据库中, 首先打开命令提示符, 输入以下格式的命名:

```
mysql -h 主机名 -u 用户名 -p
```

参数说明：

- **-h** : 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0.1)该参数可以省略;
- **-u** : 登录的用户名;
- **-p** : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。

如果我们要登录本机的 MySQL 数据库，只需要输入以下命令即可：

```
mysql -u root -p
```

按回车确认, 如果安装正确且 MySQL 正在运行, 会得到以下响应:

```
Enter password:
```

若密码存在, 输入密码登录, 不存在则直接按回车登录。登录成功后你将会看到 Welcome to the MySQL monitor... 的提示语。

然后命令提示符会一直以 **mysq>** 加一个闪烁的光标等待命令的输入, 输入 **exit** 或 **quit**或\q 退出登录。

## 启动及关闭 MySQL 服务器

在 Windows 系统下，打开命令窗口(cmd)，进入 MySQL 安装目录的 bin 目录。

启动：

```
cd c:/mysql/bin
mysqld --console
```

关闭：

```
cd c:/mysql/bin
mysqladmin -uroot shutdown
```

Linux 系统下

首先，我们需要通过以下命令来检查MySQL服务器是否启动：

```
ps -ef | grep mysqld
```

如果MySql已经启动，以上命令将输出mysql进程列表， 如果mysql未启动，你可以使用以下命令来启动mysql服务器:

```
root@host# cd /usr/bin
./mysqld_safe &
```

如果你想关闭目前运行的 MySQL 服务器, 你可以执行以下命令:

```
root@host# cd /usr/bin
./mysqladmin -u root -p shutdown
Enter password: ******
```

## MySQL 用户设置

如果你需要添加 MySQL 用户，你只需要在 mysql 数据库中的 user 表添加新用户即可。

以下为添加用户的的实例，用户名为guest，密码为guest123，并授权用户可进行 SELECT, INSERT 和 UPDATE操作权限：

```
root@host# mysql -u root -p
Enter password:*******
mysql> use mysql;
Database changed

mysql> INSERT INTO user 
          (host, user, password, 
           select_priv, insert_priv, update_priv) 
           VALUES ('localhost', 'guest', 
           PASSWORD('guest123'), 'Y', 'Y', 'Y');
Query OK, 1 row affected (0.20 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 1 row affected (0.01 sec)

mysql> SELECT host, user, password FROM user WHERE user = 'guest';
+-----------+---------+------------------+
| host      | user    | password         |
+-----------+---------+------------------+
| localhost | guest | 6f8c114b58f2ce9e |
+-----------+---------+------------------+
1 row in set (0.00 sec)
```

在添加用户时，请注意使用MySQL提供的 PASSWORD() 函数来对密码进行加密。 你可以在以上实例看到用户密码加密后为： 6f8c114b58f2ce9e.

**注意：**在 MySQL5.7 中 user 表的 password 已换成了**authentication_string**。

**注意：**password() 加密函数已经在 8.0.11 中移除了，可以使用 MD5() 函数代替。

**注意：**在注意需要执行 **FLUSH PRIVILEGES** 语句。 这个命令执行后会重新载入授权表。

如果你不使用该命令，你就无法使用新创建的用户来连接mysql服务器，除非你重启mysql服务器。

你可以在创建用户时，为用户指定权限，在对应的权限列中，在插入语句中设置为 'Y' 即可，用户权限列表如下：

- Select_priv
- Insert_priv
- Update_priv
- Delete_priv
- Create_priv
- Drop_priv
- Reload_priv
- Shutdown_priv
- Process_priv
- File_priv
- Grant_priv
- References_priv
- Index_priv
- Alter_priv

另外一种添加用户的方法为通过SQL的 GRANT 命令，以下命令会给指定数据库TUTORIALS添加用户 zara ，密码为 zara123 。

```
root@host# mysql -u root -p
Enter password:*******
mysql> use mysql;
Database changed

mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP
    -> ON TUTORIALS.*
    -> TO 'zara'@'localhost'
    -> IDENTIFIED BY 'zara123';
```

以上命令会在mysql数据库中的user表创建一条用户信息记录。

**注意:** MySQL 的SQL语句以分号 (;) 作为结束标识。

------

## /etc/my.cnf 文件配置

一般情况下，你不需要修改该配置文件，该文件默认配置如下：

```
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

[mysql.server]
user=mysql
basedir=/var/lib

[safe_mysqld]
err-log=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

在配置文件中，你可以指定不同的错误日志文件存放的目录，一般你不需要改动这些配置。



## 管理MySQL的命令

以下列出了使用Mysql数据库过程中常用的命令：

- **USE \*数据库名\*** :
  选择要操作的Mysql数据库，使用该命令后所有Mysql命令都只针对该数据库。

  ```
  mysql> use RUNOOB;
  Database changed
  ```

- **SHOW DATABASES:**
  列出 MySQL 数据库管理系统的数据库列表。

  ```
  mysql> SHOW DATABASES;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | RUNOOB             |
  | cdcol              |
  | mysql              |
  | onethink           |
  | performance_schema |
  | phpmyadmin         |
  | test               |
  | wecenter           |
  | wordpress          |
  +--------------------+
  10 rows in set (0.02 sec)
  ```

- **SHOW TABLES:**
  显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库。

  ```
  mysql> use RUNOOB;
  Database changed
  mysql> SHOW TABLES;
  +------------------+
  | Tables_in_runoob |
  +------------------+
  | employee_tbl     |
  | runoob_tbl       |
  | tcount_tbl       |
  +------------------+
  3 rows in set (0.00 sec)
  ```

- **SHOW COLUMNS FROM \*数据表\*:**
  显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。

  ```
  mysql> SHOW COLUMNS FROM runoob_tbl;
  +-----------------+--------------+------+-----+---------+-------+
  | Field           | Type         | Null | Key | Default | Extra |
  +-----------------+--------------+------+-----+---------+-------+
  | runoob_id       | int(11)      | NO   | PRI | NULL    |       |
  | runoob_title    | varchar(255) | YES  |     | NULL    |       |
  | runoob_author   | varchar(255) | YES  |     | NULL    |       |
  | submission_date | date         | YES  |     | NULL    |       |
  +-----------------+--------------+------+-----+---------+-------+
  4 rows in set (0.01 sec)
  ```

- **SHOW INDEX FROM \*数据表\*:**
  显示数据表的详细索引信息，包括PRIMARY KEY（主键）。

  ```
  mysql> SHOW INDEX FROM runoob_tbl;
  +------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | Table      | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
  +------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | runoob_tbl |          0 | PRIMARY  |            1 | runoob_id   | A         |           2 |     NULL | NULL   |      | BTREE      |         |               |
  +------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  1 row in set (0.00 sec)
  ```

- **SHOW TABLE STATUS [FROM db_name] [LIKE 'pattern'] \G:**
  该命令将输出Mysql数据库管理系统的性能及统计信息。

  ```
  mysql> SHOW TABLE STATUS  FROM RUNOOB;   # 显示数据库 RUNOOB 中所有表的信息
  
  mysql> SHOW TABLE STATUS from RUNOOB LIKE 'runoob%';     # 表名以runoob开头的表的信息
  mysql> SHOW TABLE STATUS from RUNOOB LIKE 'runoob%'\G;   # 加上 \G，查询结果按列打印
  ```

Gif 图演示：

![img](https://www.runoob.com/wp-content/uploads/2014/03/mysql-admin.gif)



# MySQL 创建数据库

------

我们可以在登陆 MySQL 服务后，使用 **create** 命令创建数据库，语法如下:

```
CREATE DATABASE 数据库名;
```

以下命令简单的演示了创建数据库的过程，数据名为 RUNOOB:

```
[root@host]# mysql -u root -p   
Enter password:******  # 登录后进入终端

mysql> create DATABASE RUNOOB;
```

### 使用 mysqladmin 创建数据库

使用普通用户，你可能需要特定的权限来创建或者删除 MySQL 数据库。

所以我们这边使用root用户登录，root用户拥有最高权限，可以使用 mysql **mysqladmin** 命令来创建数据库。

以下命令简单的演示了创建数据库的过程，数据名为 RUNOOB:

```
[root@host]# mysqladmin -u root -p create RUNOOB
Enter password:******
```

以上命令执行成功后会创建 MySQL 数据库 RUNOOB。

## 数据库操作：

3.1显示数据库 show databases;

3.2选择数据库 use examples;

3.3创建数据库并设置编码utf8 多语言 create database bk default character set utf8 collate utf8_general_ci;

使用root登录后，可以使用

```
CREATE DATABASE IF NOT EXISTS RUNOOB DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

创建数据库，该命令的作用：

-  \1. 如果数据库不存在则创建，存在则不创建。
-  \2. 创建RUNOOB数据库，并设定编码集为utf8

3.4修改数据库字符集为utf8 use mysql; alter database character set utf8;

3.5删除数据库 drop database bk;

3.6查看数据库状态： status;



### 数据表的操作：

4.1显示数据表 show tables;

4.2查看数据表的结构属性（字段，类型） describe test; desc test;

4.3复制表结构（里面没有数据，结构一样） create table newtest like oldtest; //创建新表newtest和旧表oldtest数据表结构一样

4.4复制表中的数据 insert into nettest select * from oldtest; //将旧表oldtest的数据复制到新表newtest里面

4.6重命名表名 alter table old_name rename new_name

4.7显示当前mysql版本和当前日期 select version(),current_date;

4.8创建表： create table bk( id int(10) unsigned zerofill not null auto_increment, email varchar(40) not null, ip varchar(15) not null, state int(10) not null default '-1', primary key (id) ); /*

1.常用的数据类型为int,varchar,date,text这4个数据类型

2.字段（行）的属性有：数据类型（数据长度） 是否为空 是否为主键 是否为自动增加 默认值 如：int(25) not null primary key auto_increment default 12

3.每个字段之间用逗号分开，最后那个字段不需要用逗号

4.以分号结束

5.可以设置简单数据表结构如： 字段名数据类型 数据长度 是否为空 是否主键是否自动增加 默认值 id int 12 NOT NULL primary key auto_increment name varchar 30NOT NULL password varchar30 NOT NULL time date 30 NOT NULL jianyi text 400 */

4.9删除数据表： drop table bk; //包括结构和数据都删除

10.数据库字段的操作：

11.1添加表字段 alter table test add bk varchar(32) not null; //向表test中添加bk字段（列）

11.2修改表字段 alter table test change id id1 varchar(10) not null; //将表test中字段（列）id更改为id1

11.3删除字段（列） alter table test drop cn;

11.4插入表数据 insert into test (id11,email,ip,state,bk)value(2,'[601462930@qq.com](mailto:601462930@qq.com)','10.192.16.12',1314,567);
//如果是字符型对应的值需要用单引号引起来，数字型不需要

11.5删除数据 delete from test //删除整个表test的数据，结构保留 delete from test where id11=2; 删除数据表来自某个主键字段。就等于删除整条数据

11.6修改表字段数据信息(数据） Update table_name set 字段名=’新值’ [, 字段2 =’新值’ , …..][where id=id_num] [order by 字段 顺序] update test set email='[895098355@qq.com](mailto:895098355@qq.com)' where id11=2;

11.7修改字段的属性（数据类型） alter table test modify class varchar(30) not null; //修改表test中字段class的属性为varchar(30)

### 12.查询数据：



12.1查询所有数据： select * form test;

12.2查询两个字段的数据 select id,number from test;

12.3查询前2行数据： select * from test limit 0,2;

12.4按增序排列查询 select * from test order by id11 asc; select * from test order by id11 //默认为增序查询

12.5按降序排列查询 select * from test order by id11 desc;

12.6模糊查询 select * from test where email '%qq%'; //查询test表中，条件是email的数据中包含qq的数据

12.7查询某个字段下面的数据 select email as emaildata from test ; //选择email字段作为emalidata统计显示出的数据

12.8.条件查询 select * form test where id=12;

### 13.多表查询：



13.1用法一：where条件联合查询 select 表1.字段 [as 别名],表n.字段 from 表1 [别名],表n where 条件; select testA.username as username,testB.id from testA,testB where testA.uid=testB.uid

13.2用法二：inner join on 条件联合查询 select 表1.字段 [as 别名],表n.字段 from 表1 INNER JOIN 表n on 条件; select testA.username as uername ,testB.id from testA inner join testB on testA.uid=testB.uid

13.3记录联合: select语句1 union[all] select语句2 select * from testA union select id from testB;









