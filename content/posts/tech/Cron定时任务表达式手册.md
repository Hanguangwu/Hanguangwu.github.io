---
title: "Cron定时任务表达式手册"
date: 2021-08-11T18:34:25+08:00
lastmod: 2021-08-11T22:54:22+08:00
draft: false
categories:
- 技术
tags:
- Cron
- Getting Started
---





# Cron定时任务表达式手册

[Cron 定时任务表达式手册](https://www.gairuo.com/p/cron-expression-sheet)

Cron 表达式，是应用在 Unix 和类 Unix 操作系统之中，让脚本、任务定时进行周期性重复的执行。Cron 表达式有差丰富的表达能力，能够适应各种时间表达需求。本页整理了一些基本语法和常用样例，供大家参考。

## 基本语法

![cron 语法示例](https://www.gairuo.com/file/pic/2019/09/cron-2131.jpg)

## 语法说明

共 7 位，最后一位可选，可以不写，至少 6 位，从左到右各位置分别是：

| 位置 | 意义 | 取值              | 支持的符号        |
| ---- | ---- | ----------------- | ----------------- |
| 1    | 秒   | 0-59              | `, - * /`         |
| 2    | 分   | 0-59              | `, - * /`         |
| 3    | 时   | 0-23              | `, - * /`         |
| 4    | 日   | 1-31              | `, - * ? / L W C` |
| 5    | 月   | 1-12 或 JAN - DEC | `, - * /`         |
| 6    | 周   | 1-7 或 MON - SAT  | `, - * ? / L C #` |
| 7    | 年   | 空或 1970-2099    | `, - * /`         |

注：Cron 表达式对日期英文缩写、特殊字符大小写不敏感。

## 符号说明

| 符号 | 名称     | 功能                                                         |
| ---- | -------- | ------------------------------------------------------------ |
| `*`  | 星号     | 表示重复对应位置上的周期，比如在第四位上表示每日             |
| `,`  | 逗号     | 代表一个列表值，表示多个指定时间，如周位上`SAT,SUN`表示每周六周日 |
| `?`  | 问号     | 无意义，占位符，只能在日、周位上                             |
| `-`  | 减号     | 表示一个范围，如时位上 `20-22`表示 20、21、22点              |
| `/`  | 斜杠     | a/b 可以表示以 a 为起点步长为 b 的时间序列，如日位上`10/10`表示10日20日30日 |
| `L`  | Last     | 月份最后一天或星期六，周位上 `6L` 表示月份的最后一个周五     |
| `W`  | Weekday  | 后边最近的工作日，`3W` 3日如是周五，则在6日（周一）执行      |
| `#`  | 井号     | a#b 表示当月第 b 个星期 a，如 `6#1` 当月第一个星期五         |
| `C`  | Calendar | 关联的“日历”的计算结果                                       |

注：

- 周位上给定值后，在日位上要用 ?
- “L” 和 “W” 可在日位中联合使用，LW 表示这个月最后一周的工作日
- “W” 字符指定的最近工作日是不能跨月
- W 字符串只能指定单一日期，而不能指定日期范围
- 日位建议最大值为 28 ，因为 2 月有时候是 28 天

星期对照：

> 0 或 7=Sun, 1=Mon, 2=Tue, 3=Wed, 4=Thu, 5=Fri, 6=Sat
> 1,3,5=Mon,Wed,Fri

## 示例

```
# 每五分钟
0 5 * * * ?
# 每天 5 点半
0 30 5 * * ?
# 每周五 18:18
0 18 18 ? * FRI *
# 每天 8 点到 23 点，每小时
1 1 8-23 * * ?
# 周一到周五每天 8 点到 22 点，每三小时
1 1 8-22/3 ? * MON-FRI
# 当月 13日 10:30 14:30 17:30
30 10,14,17 13 * *
# 每天 6 到 8 点，每 10 分钟
*/10 6-8 * * *
# At 00:05 on Monday
5 0 * * 1
```

## 定时设置

### Linux 设置定时任务

以 unbuntu 为例：

```
# 格式是：分 时 日 月 星期 要运行的命令
# week (0 - 6) = sun,mon,tue,wed,thu,fri,sat
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,...,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
# 编辑修改，centOS 可用 vi /etc/crontab，i 进入编辑状态，:q 退出 :wq 保存退出
sudo nano /etc/crontab
# 监控日志 cron_log 手动创建
*/1 * * * * /bin/date >> /home/cron_log/log_$(date +\%Y-\%m\%d-\%H\%M)
 # 生效一下
crontab /etc/crontab
# 看到的生效的
crontab -l
# 观察执行情况
tail -f /var/log/cron
# 查看最近的 crontab 执行情况
tail -f /var/spool/mail/root
# 其他的与 MAC 差不多，不累述
```

执行命令行示例：

```
# 执行 PHP 脚本:
/usr/bin/php /home/username/public_html/cron.php

# 执行 Python 脚本:
/home/miniconda3/envs/my_env/bin/python /home/do.py

# 执行 MySQL dump 备份:
mysqldump -u root -pPASSWORD database > /root/db.sql

# 获取 URL 信息:
/usr/bin/wget --spider "http://www.domain.com/cron.php"
```

### Windows 设置定时任务

不支持，可以使用 我的电脑 - 右键‘管理’ - 任务计划程序 进行操作。

### Mac 设置定时任务

```
# 查看是否启动
sudo launchctl list | grep cron
# 如无文件，创建一个
sudo touch /etc/crontab
vi /etc/crontab
crontab /etc/crontab
# 状态操作
sudo /usr/sbin/cron start | restart | stop
# 查看已有任务列表
sudo crontab -l
# 编辑任务
sudo crontab -e
# 保存并退款 esc，wq!保存，目测即可生效
# 删除 crontab 文件
sudo crontab -r
# 不执行(如:python)可能要增加 cron 的 Full Disk Access 权限
# System Preferences > Security & Privacy > Privacy > Full Disk Access:
# command+shift+G GOTO: /usr/sbin/cron, 选择 cron
# 可检查是否文件权限问题
cd /Users/gai/Documents/Dev/bin/
chmod +x auto_run.sh
```

crontab 的文件格式：

```
分 时 日 月 星期 要运行的命令
* 第1列分钟0～59
* 第2列小时0～23（0表示子夜）
* 第3列日1～31
* 第4列月1～12
* 第5列星期0～7（0和7表示星期天）
* 第6列要运行的命令
```

脚本命令中带参数写法：

```
# 时间参数会解析成 2019-0911-2243，多条命令用 & 连接
15 11 * * * touch "/tmp/task-$(date +\%Y-\%m\%d-\%H\%M).sql"
# 每分钟会在文档里增加一条时间记录
*/1*/1 * * * * /bin/date >> /Users/hui/Downloads/time.txt
# 如执行脚本
*/1 * * * * /bin/sh /Dev/bin/auto_run.sh
# 如 查看 crontab 环境变量
* * * * * env >> /Users/gai/Downloads/envs.txt
```

其他：

如果 cron 任务有错误，系统会发信给本机 `/var/mail/$user`，打开终端会看到 `You have new mail.`，输入 `mail` 可查看，阅读完后删除 $user 文件即可。

更多: https://www.jianshu.com/p/7ecf40421cf2

## 代码应用

在 JAVA 中的应用：

```
@Component
public class ScheduleTask {
// 每天5点半
    @Scheduled(cron = "0 30 5 * * ?")
}
```

JavaScript：

```
var CronJob = require('cron').CronJob;
new CronJob('* * * * * *', function() {
  console.log('You will see this message every second');
}, null, true, 'America/Los_Angeles');
```

## 更新说明

Cron 表达式在线解析:

- http://cron.qqe2.com
- https://crontab.guru
- https://crontab-generator.org

linux内置的cron进程能帮我们实现这些需求，cron搭配shell脚本，非常复杂的指令也没有问题。

### cron介绍

我们经常使用的是crontab命令是cron table的简写，它是cron的配置文件，也可以叫它作业列表，我们可以在以下文件夹内找到相关配置文件。

- /var/spool/cron/ 目录下存放的是每个用户包括root的crontab任务，每个任务以创建者的名字命名
- /etc/crontab 这个文件负责调度各种管理和维护任务。
- /etc/cron.d/ 这个目录用来存放任何要执行的crontab文件或脚本。
- 我们还可以把脚本放在/etc/cron.hourly、/etc/cron.daily、/etc/cron.weekly、/etc/cron.monthly目录中，让它每小时/天/星期、月执行一次。

### crontab的使用

我们常用的命令如下：

```
crontab [-u username]　　　　//省略用户表表示操作当前用户的crontab
    -e      (编辑工作表)
    -l      (列出工作表里的命令)
    -r      (删除工作作)
```

我们用**crontab -e**进入当前用户的工作表编辑，是常见的vim界面。每行是一条命令。

crontab的命令构成为 时间+动作，其时间有**分、时、日、月、周**五种，操作符有

- ***** 取值范围内的所有数字
- **/** 每过多少个数字
- **-** 从X到Z
- **，**散列数字

------

## Linux Crontab 定时任务实例

[菜鸟教程链接](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)

### 实例1：每1分钟执行一次myCommand

```
* * * * * myCommand
```

### 实例2：每小时的第3和第15分钟执行

```
3,15 * * * * myCommand
```

### 实例3：在上午8点到11点的第3和第15分钟执行

```
3,15 8-11 * * * myCommand
```

### 实例4：每隔两天的上午8点到11点的第3和第15分钟执行

```
3,15 8-11 */2  *  * myCommand
```

### 实例5：每周一上午8点到11点的第3和第15分钟执行

```
3,15 8-11 * * 1 myCommand
```

### 实例6：每晚的21:30重启smb

```
30 21 * * * /etc/init.d/smb restart
```

### 实例7：每月1、10、22日的4 : 45重启smb

```
45 4 1,10,22 * * /etc/init.d/smb restart
```

### 实例8：每周六、周日的1 : 10重启smb

```
10 1 * * 6,0 /etc/init.d/smb restart
```

### 实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb

```
0,30 18-23 * * * /etc/init.d/smb restart
```

### 实例10：每星期六的晚上11 : 00 pm重启smb

```
0 23 * * 6 /etc/init.d/smb restart
```

### 实例11：每一小时重启smb

```
0 */1 * * * /etc/init.d/smb restart
```

### 实例12：晚上11点到早上7点之间，每隔一小时重启smb

```
0 23-7/1 * * * /etc/init.d/smb restart
```








