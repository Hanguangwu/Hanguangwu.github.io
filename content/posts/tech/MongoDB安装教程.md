---
title: "MongoDB安装教程"
date: 2021-08-04T20:34:25+08:00
lastmod: 2021-08-24T22:54:22+08:00
draft: false
categories:
- 数据库
- 技术
tags:
- MongoDB
- Getting Started
---

# MongoDB安装教程

MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。

在高负载的情况下，添加更多的节点，可以保证服务器性能。

MongoDB 旨在为WEB应用提供可扩展的高性能数据存储解决方案。

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

![img](https://www.runoob.com/wp-content/uploads/2013/10/crud-annotated-document.png)

[现在开始学习 MongoDB！](https://www.runoob.com/mongodb/nosql.html)



### Ubuntu18.04安装MongoDB Community Edition

[MongoDB 源码下载地址](https://www.mongodb.com/download-center#community)

通过apt包管理工具安装MongoDB

`sudo apt install mongodb`

查看版本

`mongod -version`

本机：

```
db version v3.6.3
git version: 9586e557d54ef70f9ca4b43c26892cd55257e1a5
OpenSSL version: OpenSSL 1.1.1  11 Sep 2018
allocator: tcmalloc
modules: none
build environment:
    distarch: x86_64
    target_arch: x86_64
```

本机版本

启动`sudo service mongodb start`

进入shell `mongo`

退出shell `exit`

关闭`sudo service mongodb stop`

查看状态`sudu service mongodb status`

help

```
	db.help()                    help on db methods
	db.mycoll.help()             help on collection methods
	sh.help()                    sharding helpers
	rs.help()                    replica set helpers
	help admin                   administrative help
	help connect                 connecting to a db help
	help keys                    key shortcuts
	help misc                    misc things to know
	help mr                      mapreduce

	show dbs                     show database names
	show collections             show collections in current database
	show users                   show users in current database
	show profile                 show most recent system.profile entries with time >= 1ms
	show logs                    show the accessible logger names
	show log [name]              prints out the last segment of log in memory, 'global' is default
	use <db_name>                set current database
	db.foo.find()                list objects in collection foo
	db.foo.find( { a : 1 } )     list objects in foo where a == 1
	it                           result of the last line evaluated; use to further iterate
	DBQuery.shellBatchSize = x   set default number of items to display on shell
	exit                         quit the mongo shell

```



### 1.安装MongoDB

#### 第一步：导入public key，包管理系统会使用到

```shell
$ wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -

  
 
```

这个执行后的结果，应该要返回OK，否则就是导入失败。

#### 第二步：为MongoDB创建一个列表文件

Ubuntu 18.04(Bionic):

```shell
$ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list

  
 
```

#### 第三步：重新加载本地的包数据库

```shell
$ sudo apt-get update

  
 
```

#### 第四步：安装MongoDB包

```shell
$ sudo apt-get install -y mongodb-org

  
 
```

执行完上面4步就安装好MongoDB了。

### 2.运行MongoDB Coｍmunity Edition

通过包管理工具（apt）安装的MongoDB的数据目录是 **/var/lib/mongodb** ，日志目录是 **/var/log/mongodb**，配置文件是 **/ect/mongod.conf**，如果修改了配置文件，那么MongoDB实例必须重启，否则无效。

大多数的类Unix操作系统都会限制一个会话对系统资源的使用，这个限制将会对发挥MongoDB的操作性能产生影响，因此可以参考类UNix系统无限制设置，提高MongoDB的性能。

默认情况下，MongoDB使用mongodb用户账号运行。如果你改变用户运行MongoDB，那么你必须修改新用户的权限，让其可以访问数据目录和日志目录。

#### 第一步：初始化系统

运行和管理mongod进程，我们将使用操作系统内置初始化系统。最新的linux版本，一般都用systemd（使用的是systemctl命令）。Systemd 是 Linux 系统工具，用来启动守护进程，已成为大多数发行版的标准配置。关于Systemd可以参考《Systemd与initd》。

如果你不确定你的平台使用哪个初始化系统，可以运行下面的命令确定：

```shell
$ ps --no-headers -o comm 1

  # systemd
 
```



启动MongoDB：

```shell
$ sudo systemctl start mongod

  
 
```

如果提示`Failed to start mongod.service: Unit mongod.service not found.`可以执行下面的命令：

```shell
$ sudo systemctl daemon-reload

  
 
```

#### 第二步：验证MongoDB是否启动成功

```shell
$ sudo systemctl daemon-reload

  
 
```

#### 第三步：停止MongoDB

```shell
$ sudo systemctl stop mongod

  
 
```

#### 第四步：重启MongoDB

```shell
$ sudo systemctl restart mongod

  
 
```

### 3.开始使用MognoDB

打开一个mongo shell，它会连接到mongod实例，默认端口是27017：

```shell
$ mongo

  
 
```

更多mongo shell设置可以参考这个网址：https://docs.mongodb.com/manual/mongo/。

接下来就可以使用mongoDB了。





**如何修复子进程 /usr/bin/dpkg 在 Ubuntu 中返回错误代码 (1)**

错误消息**“子进程 /usr/bin/dpkg 返回错误代码 (1)”**表示包安装程序存在问题。在软件安装失败或安装程序损坏后，这可能会在 Ubuntu 中发生。

此错误中的关键短语是**/usr/bin/dpkg。**这是指 Linux 的 dpkg 软件包安装程序。软件包安装程序是一种跟踪软件、更新和依赖项的应用程序。如果它已损坏，任何新的软件安装都会导致此错误消息。

After trying

```
sudo dpkg --configure -a
```

and

```
sudo apt-get install -f
```

the problem of a broken package still exist the solution is to edit the dpkg status file manually.

```
sudo nano /var/lib/dpkg/status  
```

(you can use vim instead of nano)

Locate the corrupt package, and remove the whole block of information about it and save the file.

















#### 什么是NoSQL?

NoSQL，指的是非关系型的数据库。NoSQL有时也称作Not Only SQL的缩写，是对不同于传统的关系型数据库的数据库管理系统的统称。

NoSQL用于超大规模数据的存储。（例如谷歌或Facebook每天为他们的用户收集万亿比特的数据）。这些类型的数据存储不需要固定的模式，无需多余操作就可以横向扩展。

#### 为什么使用NoSQL ?

今天我们可以通过第三方平台（如：Google,Facebook等）可以很容易的访问和抓取数据。用户的个人信息，社交网络，地理位置，用户生成的数据和用户操作日志已经成倍的增加。我们如果要对这些用户数据进行挖掘，那SQL数据库已经不适合这些应用了, NoSQL 数据库的发展却能很好的处理这些大的数据。

#### 关系型数据库遵循ACID规则

事务在英文中是transaction，和现实世界中的交易很类似，它有如下四个特性：



**1、A (Atomicity) 原子性**

原子性很容易理解，也就是说事务里的所有操作要么全部做完，要么都不做，事务成功的条件是事务里的所有操作都成功，只要有一个操作失败，整个事务就失败，需要回滚。



比如银行转账，从A账户转100元至B账户，分为两个步骤：1）从A账户取100元；2）存入100元至B账户。这两步要么一起完成，要么一起不完成，如果只完成第一步，第二步失败，钱会莫名其妙少了100元。



**2、C (Consistency) 一致性**

一致性也比较容易理解，也就是说数据库要一直处于一致的状态，事务的运行不会改变数据库原本的一致性约束。



例如现有完整性约束a+b=10，如果一个事务改变了a，那么必须得改变b，使得事务结束后依然满足a+b=10，否则事务失败。

**3、I (Isolation) 独立性**

所谓的独立性是指并发的事务之间不会互相影响，如果一个事务要访问的数据正在被另外一个事务修改，只要另外一个事务未提交，它所访问的数据就不受未提交事务的影响。

比如现在有个交易是从A账户转100元至B账户，在这个交易还未完成的情况下，如果此时B查询自己的账户，是看不到新增加的100元的。

**4、D (Durability) 持久性**

持久性是指一旦事务提交后，它所做的修改将会永久的保存在数据库上，即使出现宕机也不会丢失。

#### RDBMS vs NoSQL

**RDBMS**
\- 高度组织化结构化数据
\- 结构化查询语言（SQL） (SQL)
\- 数据和关系都存储在单独的表中。
\- 数据操纵语言，数据定义语言
\- 严格的一致性
\- 基础事务

**NoSQL**
\- 代表着不仅仅是SQL
\- 没有声明性查询语言
\- 没有预定义的模式
-键 - 值对存储，列存储，文档存储，图形数据库
\- 最终一致性，而非ACID属性
\- 非结构化和不可预知的数据
\- CAP定理
\- 高性能，高可用性和可伸缩性

#### CAP定理（CAP theorem）

在计算机科学中, CAP定理（CAP theorem）, 又被称作 布鲁尔定理（Brewer's theorem）, 它指出对于一个分布式计算系统来说，不可能同时满足以下三点:

- **一致性(Consistency)** (所有节点在同一时间具有相同的数据)
- **可用性(Availability)** (保证每个请求不管成功或者失败都有响应)
- **分隔容忍(Partition tolerance)** (系统中任意信息的丢失或失败不会影响系统的继续运作)

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，最多只能同时较好的满足两个。

因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类：

- CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。
- CP - 满足一致性，分区容忍性的系统，通常性能不是特别高。
- AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。

![img](https://www.runoob.com/wp-content/uploads/2013/10/cap-theoram-image.png)



#### NoSQL的优点/缺点

优点:

- \- 高可扩展性
- \- 分布式计算
- \- 低成本
- \- 架构的灵活性，半结构化数据
- \- 没有复杂的关系

缺点:

- \- 没有标准化
- \- 有限的查询功能（到目前为止）
- \- 最终一致是不直观的程序



#### BASE

BASE：Basically Available, Soft-state, Eventually Consistent。 由 Eric Brewer 定义。

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，最多只能同时较好的满足两个。

BASE是NoSQL数据库通常对可用性及一致性的弱要求原则:

- Basically Available --基本可用
- Soft-state --软状态/柔性事务。 "Soft state" 可以理解为"无连接"的, 而 "Hard state" 是"面向连接"的
- Eventually Consistency -- 最终一致性， 也是 ACID 的最终目的。



#### ACID vs BASE

| ACID                    | BASE                                  |
| :---------------------- | :------------------------------------ |
| 原子性(**A**tomicity)   | 基本可用(**B**asically **A**vailable) |
| 一致性(**C**onsistency) | 软状态/柔性事务(**S**oft state)       |
| 隔离性(**I**solation)   | 最终一致性 (**E**ventual consistency) |
| 持久性 (**D**urable)    |                                       |






