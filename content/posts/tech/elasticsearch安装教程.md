---
title: "elasticsearch安装教程"
date: 2021-08-06T18:34:25+08:00
lastmod: 2021-08-13T22:54:22+08:00
draft: false
categories:
- 技术
- 搜索
tags:
- ES
- Getting Started
---

# ElasticSearch安装教程

[官网](https://www.elastic.co/)

[elasticsearch初学终极教程 - 第二章: 把Elastic Search在本地跑起来](https://kalasearch.cn/blog/chapter2-run-elastic-search-locally/)

命令速查

```bash
sudo systemctl start elasticsearch.service

sudo systemctl stop elasticsearch.service
```




Elasticsearch是一个开源的分布式全文本搜索和分析引擎。它支持RESTful操作，并允许您实时存储，搜索和分析大量数据。

Elasticsearch是最流行的搜索引擎之一，为具有复杂搜索要求的应用程序提供动力，例如大型电子商务商店和分析应用程序。

在本教程中，我们将向您展示如何在Ubuntu 18.04上安装Elasticsearch。相同的说明适用于Ubuntu 16.04和任何基于Ubuntu的发行版，包括Linux Mint，Kubuntu和Elementary OS。

## Docker安装

`docker pull elasticsearch:7.14.0`

由于ES运行占内存过大容易造成本机卡死，因此要修改环境变量参数来限制ES占用内存大小

`docker run -d --name elasticsearch01 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx256m"  elasticsearch:7.14.0`


-e "discovery.type=single-node" 设置为单节点

特别注意：
-e ES_JAVA_OPTS="-Xms64m -Xmx256m" \ 测试环境下，设置ES的初始内存和最大内存，否则导致过大启动不了ES


查看运行状态

`docker stats`

输出内容
```
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
3f484684b613        elasticsearch01     143.39%             360.5MiB / 3.674GiB   9.58%               5.75kB / 0B         40MB / 381kB        36

```
测试是否安装成功

`curl localhost:9200`

输出内容：
```
{
  "name" : "3f484684b613",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "IbpTXKSlTgKLFQyNT5s0PA",
  "version" : {
    "number" : "7.14.0",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "dd5a0a2acaa2045ff9624f3729fc8a6f40835aa1",
    "build_date" : "2021-07-29T20:49:32.864135063Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

```






## 本地安装Elasticsearch

在Ubuntu 18.04上安装Elasticsearch的最简单方法是从官方Elasticsearch存储库中安装deb软件包。

在撰写本文时，Elasticsearch的最新版本为`7.14.0`，并且要求在系统上安装Java8。

首先，更新软件包索引并安装通过HTTPS访问存储库所需的apt-transport-https软件包：

```bash
sudo apt update
sudo apt install apt-transport-https
```

[安装OpenJDK 8 ](https://www.myfreax.com/install-java-on-ubuntu-18-04/)：

```bash
sudo apt install openjdk-8-jdk
```

通过运行以下命令来验证Java安装，该命令将打印Java版本：

```bash
java -version
```

输出应如下所示：

```bash
openjdk version "1.8.0_191"
OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
```

现在已安装Java，下一步是添加Elasticsearch存储库。

使用以下[ `wget` ](https://www.myfreax.com/wget-command-examples/)命令导入存储库的GPG：

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

上面的命令应该输出`OK`，这意味着密钥已成功导入，并且来自此存储库的软件包将被视为受信任的。

接下来，通过发出以下命令将Elasticsearch存储库添加到系统中：

```bash
sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list'
```

如果要安装Elasticsearch的先前版本，请在上面的命令中将`7.x`更改为所需的版本。

启用存储库后，更新`apt`软件包列表并输入以下内容来安装Elasticsearch引擎：

```bash
sudo apt update
sudo apt install elasticsearch
```

安装过程完成后，Elasticsearch服务将不会自动启动。要启动该服务并启用该服务，请运行：

```bash
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
```

您可以使用以下[ curl命令](https://www.myfreax.com/curl-command-examples/)，通过向本地主机上的端口9200发送HTTP请求来验证Elasticsearch是否正在运行：

```bash
curl -X GET "localhost:9200/"
```

您应该会看到类似的内容：

```bash
{
  "name" : "kwEpA2Q",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "B-5B34LXQFqDeIYwSgD3ww",
  "version" : {
    "number" : "7.0.0",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "b7e28a7",
    "build_date" : "2019-04-05T22:55:32.697037Z",
    "build_snapshot" : false,
    "lucene_version" : "8.0.0",
    "minimum_wire_compatibility_version" : "6.7.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

服务需要5到10秒钟才能启动。如果看到`curl: (7) Failed to connect to localhost port 9200: Connection refused`，请等待几秒钟，然后重试。

如果要查看Elasticsearch服务记录的消息，可以使用以下命令：

```bash
sudo journalctl -u elasticsearch
```

恭喜，这时您已经在Ubuntu服务器上安装了Elasticsearch。



### Elasticsearch服务超时，启动失败

[链接](https://tech.mindseed.cn/Website/714.html)

服务器重新安装了系统，所有工作都要重新来过。之前没意识到要装Elasticsearch服务，直到有人问起CirrusSearch的问题，我才想起我竟然都没安装这个服务。

按照我之前的文章《[为MediaWiki安装高级搜索（AdvancedSearch）插件](https://tech.mindseed.cn/MediaWiKi/578.html)》中的方法，一路都很顺利，安装也成功了，但是启动服务会失败。

```
Job for elasticsearch.service failed because a timeout was exceeded. See "systemctl status elasticsearch.service" and "journalctl -xe" for details.
```

用`systemctl status elasticsearch.service`命令检查结果如下：
[![Elasticsearch-Starting-Failed.png](https://ms-blog.mindseed.cn/usr/uploads/2021/01/4178940458.png)](https://ms-blog.mindseed.cn/usr/uploads/2021/01/4178940458.png)
错误代码143。用`journalctl -xe`检查的结果，主要错误如下：

```
Jan 29 16:04:49 mindseed systemd[1]: elasticsearch.service start operation timed out. Terminating.
Jan 29 16:05:01 mindseed systemd[1]: Failed to start Elasticsearch.
```

在网上查了一圈，各种说法都有，修改超时时间的：

```
sudo systemctl edit --full elasticsearch.service
```

然后将其中的TimeoutStartSec值改为900，但是没用。

还有将143作为成功退出的代码，源配置本来就已经设置好了。还有其他一堆乱七八糟的，总之我试了一圈都没成功，也就不一一列举了。

无意中翻到一个回答，提到了elasticsearch可能失败的原因——没有足够的内存让服务运行。

> 作为JVM应用程序，Elasticsearch主服务器进程仅利用专用于JVM的内存。所需的内存可能取决于所使用的JVM（32位或64位）。

```
sudo vim /etc/elasticsearch/jvm.options
```

**Error 1**

> There is insufficient memory for the Java Runtime Environment to continue

**解决方案**

作为 JVM 应用程序，**Elasticsearch**主服务器进程仅使用专用于 JVM 的内存。所需的内存可能取决于所使用的 JVM（32 位或 64 位）。JVM 使用的内存通常包括：

- **堆空间**（通过`-Xms`和配置`-Xmx`）
- **元空间**（受可用本机内存量的限制）
- **内部 JVM**（通常为数十 Mb）
- 依赖于操作系统的内存功能，如**内存映射文件**。

Elasticsearch 主要依赖于**堆内存**，并且通过将`-Xms`and `-Xmx`（堆空间）选项传递给运行**Elasticsearch**服务器的 JVM 来手动设置。

First, un-comment the value of `Xmx` and `Xms`

Next, modify the value of `-Xms` and `-Xmx` to no more than 50% of your physical RAM. The value for these settings depends on the amount of RAM available on your server and Elasticsearch requires memory for purposes other than the JVM heap and it is important to leave space for this.

找到Xms行如下：

```
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms1g
-Xmx1g
```

按照文章中的内容，-Xms和-Xmx的值不应该超过服务器物理内存的50％，很显然是我那只有1G内存的服务器拖了后腿。将值改为128m后重新启动服务，终于成功了。

**Medium requirements**: If your physical RAM is **>= 2 GB** but **<= 4 GB**

Then, your settings should be:

```bash
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms512m
-Xmx512m
```

OR

```bash
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms750m
-Xmx750m
```

**Error 2**

> Initial heap size not equal to the maximum heap size

**Solution**

Ensure the value of `-Xms` and `Xmx` are equal. That is, say, you are using the **minimum requirements** since your physical RAM is **<= 1 GB**, instead of this:

```bash
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms128m
-Xmx256m
```

it should be this:

```bash
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms128m
-Xmx128m
```

OR this:

```bash
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms256m
-Xmx256m
```

#### 参考文章

[Elasticsearch: Job for elasticsearch.service failed](https://stackoverflow.com/questions/58656747/elasticsearch-job-for-elasticsearch-service-failed)

[How to prevent systemd service start operation from timing out](https://sleeplessbeastie.eu/2020/02/29/how-to-prevent-systemd-service-start-operation-from-timing-out/)



## 配置Elasticsearch

Elasticsearch数据存储在`/var/lib/elasticsearch`目录中，配置文件位于`/etc/elasticsearch`中，并且Java启动选项可以在`/etc/default/elasticsearch`文件中进行配置。

默认情况下，Elasticsearch配置为仅在本地主机上侦听。如果连接到数据库的客户端也正在同一主机上运行，并且您正在设置单个节点群集，则无需更改默认配置文件。

### 远程访问

开箱即用的Elasticsearch，不实现身份验证，因此任何可以访问HTTP API的人都可以访问它。如果要允许远程访问您的Elasticsearch服务器，则需要配置防火墙，并仅允许从受信任的客户端访问Elasticsearch端口9200。

Ubuntu带有一个称为[ UFW ](https://www.myfreax.com/how-to-setup-a-firewall-with-ufw-on-ubuntu-18-04/)的防火墙配置工具。默认情况下，UFW已安装但未启用。在启用UFW防火墙之前，首先添加一条规则，该规则将允许传入的SSH连接：

```bash
sudo ufw allow 22
```

允许从远程可信IP地址进行评估：

```bash
sudo ufw allow from 192.168.100.20 to any port 9200
```

不要忘记用远程IP地址更改`192.168.100.20`。

通过键入以下内容来启用UFW：

```bash
sudo ufw enable
```

最后，检查防火墙的状态：

```bash
sudo ufw status
```

输出应如下所示：

```bash
Status: active

To                         Action      From
--                         ------      ----
22                         ALLOW       Anywhere
9200                       ALLOW       192.168.100.20
22 (v6)                    ALLOW       Anywhere (v6)
```

配置好防火墙后，下一步就是编辑Elasticsearch配置并允许Elasticsearch监听外部连接。

为此，请打开`elasticsearch.yml`配置文件：

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

搜索包含`network.host`的行，取消注释，然后将值更改为`0.0.0.0`：

/etc/elasticsearch/elasticsearch.yml

```bash
network.host: 0.0.0.0
```

如果您的计算机上有多个网络接口，则可以指定接口IP地址，这将使Elasticsearch仅在指定的接口上侦听。

重新启动Elasticsearch服务以使更改生效：

```bash
sudo systemctl restart elasticsearch
```

就是这样。您现在可以从远程位置连接到Elasticsearch服务器。



##  安装elasticsearch-head

我直接通过Chrome安装的插件，插件地址：https://chrome.google.com/webstore/detail/elasticsearch-head/ffmkiejjmecolpfloofpjologoblkegm?utm_source=chrome-ntp-icon 。

至此，ES安装成功，并通过elasticsearch-head工具向其中插入了两条数据。














