---
title: "ElasticSearch教程"
date: 2021-08-16T18:34:25+08:00
lastmod: 2021-08-24T22:54:22+08:00
draft: false
categories:
- 技术
- 搜索
tags:
- ES
- Getting Started
---

# ElasticSearch教程

ES是高扩展的实时分布式全文检索引擎，基于Lucence，Restful API

ELK技术：ES+logstash+kibana

[kikana官网](https://www.elastic.co/cn/downloads/past-releases/kibana-7-14-0)

[kikana-localhost](http://localhost:5601/app/home)

![img](https://s4.51cto.com/images/blog/202105/06/13be0cdfd8c01f2ea961283610795942.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

[狂神说Elasticsearch7.X学习笔记整理](https://www.cnblogs.com/DAYceng/p/14755532.html)

## 一、什么是Elasticsearch？

### Lucene简介

![Lucene Logo](https://lucene.apache.org/theme/images/lucene/lucene_logo_green_300.png)

- Lucene是一套用于**全文检索**和**搜寻**的**开源**程序库，由Apache软件基金会支持和提供

- Lucene提供了一个简单却强大的应用程序接口（API），能够做全文索引和搜寻，在Java开发环境里Lucene是一个成熟的免费开放源代码工具

- Lucene并不是现成的搜索引擎产品，但可以用来制作搜索引擎产品

- Lucene是一套信息检索工具包! jar包!不包含搜索引擎系统!
  Lucene包含:索引结构!读写索引的工具!排序,搜索规则...工具类!等

  **Lucene和ElasticSearch关系**:
  简单来说，Elasticsearch是基于Lucene做了一些**封装**和**增强 **

### Elasticsearch简介

Elasticsearch是一个**实时分布式搜索和分析引擎**。它让你以前所未有的速度处理大数据成为可能。
它用于**全文搜索、结构化搜索、分析**以及将这三者混合使用:
维基百科使用Elasticsearch提供**全文搜索并高亮关键字** ,以及**输入实时搜索(search-asyou-type)**和**搜索纠错(did-you-mean)**等搜索建议功能。
英国卫报使用Elasticsearch结合用户日志和社交网络数据提供给他们的编辑以**实时的反馈**,以便及时了解公众对新发表的文章的回应。
StackOverflow结合**全文搜索**与**地理位置查询**,以及more-like-this功能来找到相关的问题和答案。
Github使用Elasticsearch检索**1300亿**行的代码。

并且Elasticsearch不仅用于大型企业,它还让像DataDog以及Klout这样的创业公司将最初的想法变成可扩展的解决方案。

Elasticsearch可以在你的笔记本。上运行，也可以在数以百计的服务器上**处理PB级别的数据**。
Elasticsearch是一个基于Apache Lucene(TM)的开源搜索引擎。 无论在开源还是专有领域, Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。
但是, Lucene只是一个库。想要使用它,你必须使用Java来作为开发语言并将其直接集成到你的应用中,更糟糕的是, Lucene非常复杂,你需要深入了解检索的相关知识来理解它是如何工作的。

Elasticsearch也使用**Java**开发并使用Lucene作为其核心来实现所有索引和搜索的功能,它的目的是通过简单的**RESTful API**（REST风格的网络接口，是当下主流的API）来隐藏Lucene的复杂性,从而让全文搜索变得简单。

**ElasticSearch的应用场景**

1、维基百科,类似百度百科,**全文检索**,**高亮**,**搜索推荐**

2、The Guardian (国外新闻网站) , 类似搜狐新闻,用户行为日志(点击,浏览,收藏,评论) +社交网络数据(对某某新闻的相关看法) ,数据分析,给到每篇新闻文章的作者,让他知道他的文章的公众反馈

3、Stack Overflow (国外的程序异常讨论论坛) , IT问题,程序的报错, 提交上去,有人会跟你讨论和回答,全文检索,搜索相关问题和答案,程序报错了,就会将报错信息粘贴到里面去,搜索有没有对应的答案

4、GitHub (开源代码管理)

5、电商网站,**检索商品**.

6、日志数据分析, logstash采集日志, ES进行复杂的数据分析, **ELK技术, elasticsearch（搜索）+logstash（过滤）+kibana（可视化分析）**

7、商品价格监控网站,用户设定某商品的价格阈值,当低于该阈值的时候,发送通知消息给用户,比如说订阅牙膏的监控：如果高露洁牙膏的家庭套装低于50块钱,就通知我,我就去买

8、BI系统 ,商业智能, Business Intelligence.比如说有个大型商场集团, BI分析一下某某区域最近3年的用户消费金额的趋势以及用户群体的组成构成,产出相关的数张报表, 最近3年,每年消费金额呈现100%的增长,而且用户群体85%是高级白领。。。

9、国内:**站内搜索**(电商，招聘，门户，等等)，**IT系统搜索**(OA,CRM,ERP，等等)，**数据分析**(ES热门的一个使用场景)

总而言之，Elasticsearch就是提供高效、个性化检索需求的一种解决方案

### ELK简介

ELK是**Elasticsearch、Logstash、Kibana**三大开源框架首字母大写简称。市面上也被成为Elastic Stack。

其中Elasticsearch是一个基于Lucene、分布式、通过Restful方式进行交互的近实时搜索平台框架。像类似百度、谷歌这种大数据全文搜索引擎的场景都可以使用Elasticsearch作为底层支持框架,可见Elasticsearch提供的搜索能力确实强大,市面上很多时候我们简称Elasticsearch为es.

Logstash是ELK的**中央数据流引擎**,用于**从不同目标**(文件/数据存储/MQ )**收集的不同格式数据**,经过过滤后支持输出以到不同目的地(文件/MQ/redis/elasticsearch/kafka等)。

Kibana可以将es的**数据**通过友好的页面**展示**出来 ,提供实时分析的功能。

总结一下就是：**收集清洗数据-->建立索引，储存-->Kibana分析**

市面上很多开发只要提到ELK能够一致说出它是一 个日志分析架构技术栈总称,但实际上ELK不仅仅适用于日志分析,它还可以**支持其它任何数据分析和收集的场景**,日志分析和收集只是更具有代表性，并非唯一性。

![image-20210131094725452](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105031.png)

## 二、其他搜索引擎

### Solr简介

Solr是Apache下的一个顶级开源项目,采用java开发,它是基于Lucene的全文搜索服务器。Solr提供了比Lucene更为丰富的查询语言,同时实现了可配置、可扩展，并对索引、搜索性能进行了优化

Solr可以独立运行在Jetty、Tomcat等这些Servlet容器中 , Solr索引的实现方法很简单,用POST方法向Solr服务器发送一个描述Field及其内容的XML文档，Solr根据xml文档添加、删除、更新索引。

列如：搜索name==dayceng<>

Solr 搜索只需要发送HTTP GET请求,然后对Solr返回xml、**json**等格式的查询结果进行解析,组织页面布局。Solr不提供构建UI的功能, Solr提供了一个管理界面,通过管理界面可以查询Solr的配置和运行情况。

solr是基于lucene开发企业级搜索服务器,实际上就是封装了lucene.

Solr是一个独立的企业级搜索应用服务器,它对外提供类似于Web-service的API接口。用户可以通过http请求,向搜索引擎服务器提交一定格式的文件,生成索引;也可以通过提出查找请求,并得到返回结果。

### ES与Solr对比

单纯地对已有的数据进行搜索，Solr更快

![image-20210129115554753](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105035.png)

当建立**实时索引**时，Solr会产生IO阻塞，查询性能较差，此时ES具有明显优势

![image-20210129115715020](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105045.png)

随着搜索量的增加，Solr的劣势愈发明显，ES无明显变化

![image-20210129120150760](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105121.png)

**总结**

1、ES基本是开箱即用，非常简单。Solr安装略微复杂
2、Solr 利用Zookeeper进行分布式管理，而Elasticsearch 自身带有分布式协调管理功能。
3、Solr 支持更多格式的数据，比如JSON、XML、 CSV ，而Elasticsearch仅支持JSON文件格式。
4、Solr 官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能多有第三方插件提供，例如图形化界面需要kibana友好支撑
5、Solr 查询快，但更新索引时慢(即插入删除慢) ，用于电商等查询多的应用;
●ES建立索引快(即查询慢) ，即**实时性查询快**，用于facebook新浪等搜索。
●Solr是传统搜索应用的有力解决方案，但Elasticsearch 更适用于新兴的实时搜索应用。
6、Solr比较成熟，有一个更大，更成熟的用户、开发和贡献者社区,而Elasticsearch相对开发维护者较少，更新太快,学习使用成本较高。

# Elasticsearch安装

注意：java版本至少为JDK1.8或以上

## **一、安装**

> 下载：https://www.elastic.co/cn/downloads/elasticsearch
>
> windows下解压即可使用

## **二、Elasticsearch目录介绍**

> bin 相关启动文件
>
> config 配置文件
>
>  log4j2.properties 日志配置文件
>
>  jvm.options java虚拟机配置文件
>
>  elasticsearch.yml ES配置文件（默认端口：9200，这里在tpot中，docker默认分配的是1111，需要再映射到9200才行）
>
> lib 相关jar包
>
> logs 日志
>
> modules 功能模块
>
> plugins 插件

## **三、启动**

双击bin下的**elasticsearch.bat**即可

![image-20210129150734078](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105122.png)

## **四、测试访问**

访问其暴露的端口进行验证（http://127.0.0.1:9200/）

![image-20210129150840763](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105123.png)

## **五、安装可视化界面Elasticsearch-head**

**优先使用Chrome插件elasticsearch-head**

http://mobz.github.io/elasticsearch-head

[ElasticSearch Head](https://chrome.google.com/webstore/detail/elasticsearch-head/ffmkiejjmecolpfloofpjologoblkegm)

Chrome Extension containing the excellent ElasticSearch Head application.

注：本地安装需要前置环境

1、请下载Node.js（https://nodejs.org/en/），并检查npm为较新的版本

2、安装npm淘宝镜像源（cnpm）

> npm install -g cnpm *--registry=https://registry.npm.taobao.org*

 查看是否安装成功

> cnpm -v

 出现以下信息即可

> C:\Users\XXXX> cnpm -v
>
> cnpm@5.1.1 (F:\Live\NODE\node_global\node_modules\cnpm\lib\parse_argv.js)
> npm@5.6.0 (F:\Live\NODE\node_global\node_modules\cnpm\node_modules\npm\lib\npm.js)
> node@8.9.1 (F:\Live\NODE\node.exe)
> npminstall@3.2.1 (F:\Live\NODE\node_global\node_modules\cnpm\node_modules\npminstall\lib\index.js)
> prefix=F:\Live\NODE\node_global
> win32 x64 10.0.16299
> registry=http://registry.npm.taobao.org

### **在ElasticSearch\elasticsearch-head-master即ES head目录下**

 **下载依赖**

> cnpm install

![image-20210131091020099](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105124.png)

 **运行**

> npm run start

![image-20210131091205063](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105125.png)

 **访问http://localhost:9100/**

![image-20210131091312021](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105126.png)

 **此时因为跨域访问（跨端口）导致集群无法连接，要通过配置elasticsearch.yml来解决**

## 六、解决跨域问题

配置es，打开elasticsearch.yml文件，在最后一行加入（注意yalm语法，冒号后要加一个空格）

> http.cors.enabled: true
> http.cors.allow-origin: "*"

使用elasticsearch.bat重启es，连接成功

![image-20210131092227531](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105127.png)

注1：Linux下可能会因为进程问题启动失败

*#查看elastic的进程号 并杀死*

```
ps aux | grep elasticsearch 

kill -9 进程号 
```

*#重启 -d 后台运行*

```
./bin/elasticsearch -d
```

注2：Linux下eshead启动失败，提示9100端口占用

*#查看占用端口的进程id*

```
 lsof -i:9100 
```

*#杀死进行*

```
kill -9 4852
```

## 七、Kibana安装

Kibana是一个针对Elasticsearch的开源分析及可视化平台 ，用来搜索、查看交互存储在Elasticsearch索引中的数据。 使用Kibana，可以通过各种图表进行高级数据分析及展示。Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板( dashboard )实时显示Elasticsearch查询动态。设置Kibana非常简单。无需编码或者额外的基础架构，几分钟内就可以完成Kibana安装并启动Elasticsearch索引监测。

官网：https://www.elastic.co/cn/kibana

注意：**使用的ES版本要与Kibana的对应**

下载完成解压，双击kibana.bat启动即可

Ubuntu下进入kibana目录执行`./bin/kibana`启动kibana

![image-20210131103309389](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105128.png)

**汉化**

在Kibana目录下的config中修改Kibana.yml文件，最后一行加上

> i18n.locale: "zh-CN"

重启即可

kiban.yml

```yaml
# Kibana is served by a back end server. This setting specifies the port to use.
server.port: 5601

# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: "localhost"

# Enables you to specify a path to mount Kibana at if you are running behind a proxy.
# Use the `server.rewriteBasePath` setting to tell Kibana if it should remove the basePath
# from requests it receives, and to prevent a deprecation warning at startup.
# This setting cannot end in a slash.
#server.basePath: ""

# Specifies whether Kibana should rewrite requests that are prefixed with
# `server.basePath` or require that they are rewritten by your reverse proxy.
# This setting was effectively always `false` before Kibana 6.3 and will
# default to `true` starting in Kibana 7.0.
#server.rewriteBasePath: false

# Specifies the public URL at which Kibana is available for end users. If
# `server.basePath` is configured this URL should end with the same basePath.
#server.publicBaseUrl: ""

# The maximum payload size in bytes for incoming server requests.
#server.maxPayload: 1048576

# The Kibana server's name.  This is used for display purposes.
#server.name: "your-hostname"

# The URLs of the Elasticsearch instances to use for all your queries.
elasticsearch.hosts: ["http://localhost:9200"]

# Kibana uses an index in Elasticsearch to store saved searches, visualizations and
# dashboards. Kibana creates a new index if the index doesn't already exist.
kibana.index: ".kibana"

# The default application to load.
#kibana.defaultAppId: "home"

# If your Elasticsearch is protected with basic authentication, these settings provide
# the username and password that the Kibana server uses to perform maintenance on the Kibana
# index at startup. Your Kibana users still need to authenticate with Elasticsearch, which
# is proxied through the Kibana server.
#elasticsearch.username: "kibana_system"
#elasticsearch.password: "pass"

# Enables SSL and paths to the PEM-format SSL certificate and SSL key files, respectively.
# These settings enable SSL for outgoing requests from the Kibana server to the browser.
#server.ssl.enabled: false
#server.ssl.certificate: /path/to/your/server.crt
#server.ssl.key: /path/to/your/server.key

# Optional settings that provide the paths to the PEM-format SSL certificate and key files.
# These files are used to verify the identity of Kibana to Elasticsearch and are required when
# xpack.security.http.ssl.client_authentication in Elasticsearch is set to required.
#elasticsearch.ssl.certificate: /path/to/your/client.crt
#elasticsearch.ssl.key: /path/to/your/client.key

# Optional setting that enables you to specify a path to the PEM file for the certificate
# authority for your Elasticsearch instance.
#elasticsearch.ssl.certificateAuthorities: [ "/path/to/your/CA.pem" ]

# To disregard the validity of SSL certificates, change this setting's value to 'none'.
#elasticsearch.ssl.verificationMode: full

# Time in milliseconds to wait for Elasticsearch to respond to pings. Defaults to the value of
# the elasticsearch.requestTimeout setting.
#elasticsearch.pingTimeout: 1500

# Time in milliseconds to wait for responses from the back end or Elasticsearch. This value
# must be a positive integer.
#elasticsearch.requestTimeout: 30000

# List of Kibana client-side headers to send to Elasticsearch. To send *no* client-side
# headers, set this value to [] (an empty list).
#elasticsearch.requestHeadersWhitelist: [ authorization ]

# Header names and values that are sent to Elasticsearch. Any custom headers cannot be overwritten
# by client-side headers, regardless of the elasticsearch.requestHeadersWhitelist configuration.
#elasticsearch.customHeaders: {}

# Time in milliseconds for Elasticsearch to wait for responses from shards. Set to 0 to disable.
#elasticsearch.shardTimeout: 30000

# Logs queries sent to Elasticsearch. Requires logging.verbose set to true.
#elasticsearch.logQueries: false

# Specifies the path where Kibana creates the process ID file.
#pid.file: /run/kibana/kibana.pid

# Enables you to specify a file where Kibana stores log output.
#logging.dest: stdout

# Set the value of this setting to true to suppress all logging output.
#logging.silent: false

# Set the value of this setting to true to suppress all logging output other than error messages.
#logging.quiet: false

# Set the value of this setting to true to log all events, including system usage information
# and all requests.
#logging.verbose: false

# Set the interval in milliseconds to sample system and process performance
# metrics. Minimum is 100ms. Defaults to 5000.
#ops.interval: 5000

# Specifies locale to be used for all localizable strings, dates and number formats.
# Supported languages are the following: English - en , by default , Chinese - zh-CN .
#i18n.locale: "en"
i18n.locale: "zh-CN"
```

# ES核心概念

## 概述

Elasticsearch是面向文档的一种数据库，这意味着其不再需要行列式的表格字段约束。

ES会存储整个构造好的数据或文档，然而不仅仅是储存数据，这使得文档中每个数据可以被标识，进而可以被检索。在ES中，执行index，search，sort或过滤文档等操作都不是传统意义上的行列式的数据。

ES从根本上对数据的不同思考方式也正是他能应对复杂数据结构的全文检索的原因之一。

**关系型数据库与Elasticsearch的对比**

以下数据格式均为JSON

| Relational DB      | Elasticsearch                   |
| ------------------ | ------------------------------- |
| 数据库（database） | 索引（index）                   |
| 表（tables）       | 类型（types，新版本中逐步弃用） |
| 行（rows）         | 文档（documents）               |
| 字段（columns）    | 字段（file）                    |

Elasticsearch(一般为集群)中可以包含多个索引（对应数据库) ，每个索引中可以包含多个类型(对应表) ，每个类型下又包含多个文档(对应行)，每个文档中又包含多个字段(对应列)。

**物理设计**:
Elasticsearch在后台**把每个索引划分成多个分片**,每分分片可以在集群中的不同服务器间迁移（方便集群的搭建）

实际上只建立一个索引它自己也是一个集群，默认名称就是elasticsearch

![image-20210202093954072](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105129.png)

**逻辑设计:**
一个索引类型中，包含多个文档，如文档1 ,文档2。

当我们索引一篇文档时，可以通过这样的一个顺序找到它:

 索引-->类型-->文档ID

通过这个组合我们就能索引到某个具体的文档。

（注:ID不必是整数,实际上它是个字符串。）

## 文档

Elasticsearch是面向文档的,那么就意味着索引和搜索数据的**最小单位是文档**, Elasticsearch中,文档有几个重要属性:

●自我包含，一篇文档同时包含字段和对应的值,也就是同时包含key:value

●可以是层次型的，一个文档中包含着文档，复杂的逻辑实体就是这么来的（即文档就是JSON格式的对象，可用fastjson进行自动转换自动）

●灵活的结构，文档不依赖预先定义的模式，我们知道关系型数据库中，要提前定义字段才能使用，在Elasticsearch中，对于字段是非常灵活的，有时候我们可以忽略该字段，或者动态的添加一个新的字段。

我们可以随意的新增或者忽略某个字段，但每个字段的类型非常重要。比如一个年龄字段类型，可以是字符串也可以是整形。因为elasticsearch会保存字段和类型之间的映射及其他的设置。这种映射具体到每个映射的每种类型,这也是为什么在elasticsearch中，类型有时候也称为**映射类型**

文档就是**一条条打好标签的数据**

**举个例子：**

> user
>
> 1 xiaoming 22
>
> 2 liming 19
>
> 。。。

这是一个表，名称为user，里面的每一行就是一个文档，文档中包含着序号、名字、年龄等信息（有点像之前要使用TFIDF算法时做的那个设备文档）

## 类型

类型是文档的**逻辑容器**，就像关系型数据库一样，表格是行的容器。

类型中对于字段的定义称为**映射**，比如name可以映射为字符串类型。

我们说文档是无模式的，它们不需要拥有映射中所定义的所有字段，比如新增一个字段。那么elasticsearch是怎么做的呢?elasticsearch会自动的将新字段加入映射，但是这个字段的不确定它是什么类型，elasticsearch就开始猜，如果这个值是18 ,那么elasticsearch会认为它是整形。

但是elasticsearch也可能猜不对 ，所以最安全的方式就是提前定义好所需要的映射，这点跟关系型数据库殊途同归了，先定义好字段，然后再使用

类比MySQL中，建立一个表的时候需要设定的数据类型

![image-20210202095941630](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105130.png)

## 索引

索引是**映射类型的容器**， elasticsearch中的索引是一个非常大的文档集合（即数据库）。索引存储了映射类型的字段和其他设置。 然后它们被存储到了各个分片上了。我们来研究下分片是如何工作的。

**物理设计: 节点和分片如何工作**

![image-20210202100254208](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105131.png)

存在数据库的数据可以通过不同的分片放在不同的集群上



一个集群至少有一个节点，而一个节点就是一个elasricsearch进程，节点可以有多个索引默认的，如果你创建索引，那么索引将会有个5个分片( primary shard ,又称**主分片**)构成，每一个主分片会有一个副本( replica shard ,又称**复制分片**)

![image-20210202100505894](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105132.png)

上图是一个有3个节点的集群，可以看到主分片和对应的复制分片都不会在同一个节点内（比如主分片P0和主分片的复制分片P1分别在节点1/3，同样的分片Px在每个至少有一个），当某个节点挂掉了，数据也不至于丢失。

实际上，一个分片是一个Lucene索引，一个包含**倒排索引**的文件目录，倒排索引的结构使得elasticsearch在不扫描全部文档的情况下，就能告诉你哪些文档包含特定的关键字。什么是倒排索引？

## 倒排索引

elasticsearch使用的是一种称为倒排索引的结构 ,采用**Lucene倒排索引**作为底层。这种结构适用于快速的全文搜索，一个索引由文档中所有不重复的列表构成，对于每一个词，都有一个包含它的文档列表。

例如，现在有两个文档，每个文档包含如下内容:

> study every day, good good up to forever #文档1包含的内容
> To forever, study every day， good good up #文档2包含的内容

为了创建倒排索引，我们首先要将每个文档拆分成独立的词(或称为词条或者tokens) ，然后创建一个包含所有不重复的词条的排序列表，然后列出每个词条出现在哪个文档:（大小写要区分，重复单词也要加入）

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| Study   | 〇    | X     |
| To      | X     | X     |
| forever | 〇    | 〇    |
| every   | 〇    | 〇    |
| study   | X     | 〇    |
| day     | 〇    | 〇    |
| good    | 〇    | 〇    |
| up      | 〇    | 〇    |
| to      | 〇    | X     |
| every   | 〇    | 〇    |

现在，我们试图搜索to forever，只需要查看每个词条在对应文档是否出现即可。这里to和forever在doc1里面都有，而doc2中to没有，所以搜索结果为：doc1的**权重**更大，即“to forever”更可能出现在doc1

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| to      | 〇    | X     |
| forever | 〇    | 〇    |
| SUM     | 2     | 1     |

这里的“权重”，即为文档的**score**，es搜索完成会对分数进行自动统计

两个文档都匹配，但是第一个文档比第二个匹配程度更高。如果没有别的条件，这两个包含关键字的文档都将返回。

**再举一个例子**

再来看一个示例比如我们通过博客标签来搜索博客文章。 那么倒排索引列表就是这样的一个结构:

![image-20210202111236177](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105133.png)

如果要搜索含有python标签的文章，那相对于查找所有原始数据而言，查找倒排索引后的数据将会快的多。只需要查看标签这一栏,然后获取相关的文章ID即可。比如这里搜索“Linux”就绝对不会出现1或者2

**elasticsearch的索引和Lucene的索引对比**

在elasticsearch中，索引（数据库）这个词被频繁使用。在elasticsearch中 ，索引被分为多个分片，每份分片是一个Lucene的索引。**所以一个elasticsearch索引是由多个Lucene倒排索引组成的**。（因为elasticsearch使用Lucene作为底层）

![image-20210202112920334](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105134.png)

如无特指，说起**索引都是指elasticsearch的索引**。

# IK分词器

## **什么是IK分词器?**

 **分词**:即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把数据库中或者索引库中的数据进行分词，然后进行一个匹配操作，默认的中文分词是将每个字看成一个词，比如“我爱黎明”会被分为"我","爱","“黎","明" 。这显然是不符合要求的，所以我们需要安装中文分词器IK来解决这个问题。

IK提供了两个分词算法: ik. smart和ik_max_ word

其中ik_smart为最少切分， ik _max_word为最细粒度划分

## 安装IK分词器

**1、下载**

> https://github.com/medcl/elasticsearch-analysis-ik

Install

1.download or compile

- optional 1 - download pre-build package from here: https://github.com/medcl/elasticsearch-analysis-ik/releases

  create plugin folder `cd your-es-root/plugins/ && mkdir ik`

  unzip plugin to folder `your-es-root/plugins/ik`

- optional 2 - use elasticsearch-plugin to install ( supported from version v5.5.1 ):

  ```
  ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.3.0/elasticsearch-analysis-ik-6.3.0.zip
  ```

  NOTE: replace `6.3.0` to your own elasticsearch version

2.restart elasticsearch

Quick Example

1.create a index

```
curl -XPUT http://localhost:9200/index
```

2.create a mapping

```
curl -XPOST http://localhost:9200/index/_mapping -H 'Content-Type:application/json' -d'
{
        "properties": {
            "content": {
                "type": "text",
                "analyzer": "ik_max_word",
                "search_analyzer": "ik_smart"
            }
        }

}'
```

3.index some docs

```
curl -XPOST http://localhost:9200/index/_create/1 -H 'Content-Type:application/json' -d'
{"content":"美国留给伊拉克的是个烂摊子吗"}
'
curl -XPOST http://localhost:9200/index/_create/2 -H 'Content-Type:application/json' -d'
{"content":"公安部：各地校车将享最高路权"}
'
curl -XPOST http://localhost:9200/index/_create/3 -H 'Content-Type:application/json' -d'
{"content":"中韩渔警冲突调查：韩警平均每天扣1艘中国渔船"}
'
curl -XPOST http://localhost:9200/index/_create/4 -H 'Content-Type:application/json' -d'
{"content":"中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首"}
'
```

4.query with highlighting

```
curl -XPOST http://localhost:9200/index/_search  -H 'Content-Type:application/json' -d'
{
    "query" : { "match" : { "content" : "中国" }},
    "highlight" : {
        "pre_tags" : ["<tag1>", "<tag2>"],
        "post_tags" : ["</tag1>", "</tag2>"],
        "fields" : {
            "content" : {}
        }
    }
}
'
```

Result

```
{
    "took": 14,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "failed": 0
    },
    "hits": {
        "total": 2,
        "max_score": 2,
        "hits": [
            {
                "_index": "index",
                "_type": "fulltext",
                "_id": "4",
                "_score": 2,
                "_source": {
                    "content": "中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首"
                },
                "highlight": {
                    "content": [
                        "<tag1>中国</tag1>驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首 "
                    ]
                }
            },
            {
                "_index": "index",
                "_type": "fulltext",
                "_id": "3",
                "_score": 2,
                "_source": {
                    "content": "中韩渔警冲突调查：韩警平均每天扣1艘中国渔船"
                },
                "highlight": {
                    "content": [
                        "均每天扣1艘<tag1>中国</tag1>渔船 "
                    ]
                }
            }
        ]
    }
}
```

Dictionary Configuration

```
IKAnalyzer.cfg.xml` can be located at `{conf}/analysis-ik/config/IKAnalyzer.cfg.xml` or `{plugins}/elasticsearch-analysis-ik-*/config/IKAnalyzer.cfg.xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">custom/mydict.dic;custom/single_word_low_freq.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords">custom/ext_stopword.dic</entry>
 	<!--用户可以在这里配置远程扩展字典 -->
	<entry key="remote_ext_dict">location</entry>
 	<!--用户可以在这里配置远程扩展停止词字典-->
	<entry key="remote_ext_stopwords">http://xxx.com/xxx.dic</entry>
</properties>
```

热更新 IK 分词使用方法

目前该插件支持热更新 IK 分词，通过上文在 IK 配置文件中提到的如下配置

```
 	<!--用户可以在这里配置远程扩展字典 -->
	<entry key="remote_ext_dict">location</entry>
 	<!--用户可以在这里配置远程扩展停止词字典-->
	<entry key="remote_ext_stopwords">location</entry>
```

其中 `location` 是指一个 url，比如 `http://yoursite.com/getCustomDict`，该请求只需满足以下两点即可完成分词热更新。

1. 该 http 请求需要返回两个头部(header)，一个是 `Last-Modified`，一个是 `ETag`，这两者都是字符串类型，只要有一个发生变化，该插件就会去抓取新的分词进而更新词库。
2. 该 http 请求返回的内容格式是一行一个分词，换行符用 `\n` 即可。

满足上面两点要求就可以实现热更新分词了，不需要重启 ES 实例。

可以将需自动更新的热词放在一个 UTF-8 编码的 .txt 文件里，放在 nginx 或其他简易 http server 下，当 .txt 文件修改时，http server 会在客户端请求该文件时自动返回相应的 Last-Modified 和 ETag。可以另外做一个工具来从业务系统提取相关词汇，并更新这个 .txt 文件。

have fun.

常见问题

1.自定义词典为什么没有生效？

请确保你的扩展词典的文本格式为 UTF8 编码

2.如何手动安装？

```
git clone https://github.com/medcl/elasticsearch-analysis-ik
cd elasticsearch-analysis-ik
git checkout tags/{version}
mvn clean
mvn compile
mvn package
```

拷贝和解压release下的文件: #{project_path}/elasticsearch-analysis-ik/target/releases/elasticsearch-analysis-ik-*.zip 到你的 elasticsearch 插件目录, 如: plugins/ik 重启elasticsearch

3.分词测试失败 请在某个索引下调用analyze接口测试,而不是直接调用analyze接口 如:

```
curl -XGET "http://localhost:9200/your_index/_analyze" -H 'Content-Type: application/json' -d'
{
   "text":"中华人民共和国MN","tokenizer": "my_ik"
}'
```

1. ik_max_word 和 ik_smart 什么区别?

ik_max_word: 会将文本做最细粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,中华人民,中华,华人,人民共和国,人民,人,民,共和国,共和,和,国国,国歌”，会穷尽各种可能的组合，适合 Term Query；

ik_smart: 会做最粗粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,国歌”，适合 Phrase 查询。

Changes

*自 v5.0.0 起*

- 移除名为 `ik` 的analyzer和tokenizer,请分别使用 `ik_smart` 和 `ik_max_word`



**2、安装**

解压后放入es的插件目录plugins下即可

![image-20210202144827745](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105135.png)

**3、重启es加载插件**

![image-20210202145019120](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105136.png)

可使用以下命令查看插件列表

> elasticsearch-plugin list

**4、启动Kibana测试**

测试ik_smart（使用RESTful风格的语句发起GET请求，对我们提供的JSON数据进行分词）

> ```json
> GET _analyze
> {
> "analyzer": "ik_smart",
> "text": ["新型冠状病毒"]
> }
> ```

运行结果：

![image-20210202150329532](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105137.png)

测试ik _max _word

> ```json
> GET _analyze
> {
> "analyzer": "ik_max_word",
> "text": ["新型冠状病毒"]
> }
> ```

运行结果：最细粒度划分会把所有可能的组合都划分出来（划分方式由某个字典规定）

![image-20210202150645756](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105138.png)

问题：当遇到自造词时，拆分结果不是我们想要的

![image-20210202151449769](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105139.png)

这时需要把关键词加入字典

## 配置IK分词器

**添加自定义字典**

在

> elasticsearch-7.6.1\plugins\ik\config

中可以找到配置文件**IKAnalyzer.cfg.xml**

![image-20210202152120411](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105140.png)

只需要在config目录下新建一个自己的.dic字典文件并录入IKAnalyzer.cfg.xml中然后**重启es**即可

![image-20210202152518579](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105141.png)

测试能够识别自定义词语

![image-20210202153349063](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105142.png)

# REST风格

## **概念**

一种软件架构风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

即**通过不同的命令实现不同的操作**

## 基本REST命令

| method | url地址                                          | 描述                   |
| ------ | ------------------------------------------------ | ---------------------- |
| PUT    | localhost:9200/索引名称/类型名称/文档id          | 创建文档(指定文档id )  |
| POST   | localhost:9200/索弓\|名称/类型名称               | 创建文档(随机文档id )  |
| POST   | localhost:9200/索引名称/类型名称/文档id/_ update | 修改文档               |
| DELETE | localhost:9200/索引名称/类型名称/文档id          | 删除文档               |
| GET    | localhost:9200/索引名称/类型名称/文档id          | 查询文档（通过文档id） |
| POST   | localhost:9200/索弓\|名称/类型名称/_ search      | 查询所有数据           |

# 测试

### 关于索引的操作

## 2.新建操作 

### 2.1 新建索引 {PUT /索引名/类型名/文档id}

 ![img](https://s4.51cto.com/images/blog/202105/06/71eac03a363ee396cbac1de9d7f9949d.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

类型名，ES 8后不再建议设置，默认是_doc类型 

字段属性，(下图的name, age)，ES会自行猜测匹配。

![img](https://s4.51cto.com/images/blog/202105/06/5a2e16473601ec7caec3f3aeeedacde5.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

 

 

 ![img](https://s4.51cto.com/images/blog/202105/06/be567ac7ecfc490c3f91ad6757721c6f.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

![img](https://s4.51cto.com/images/blog/202105/06/f007c256452ae2065206b7a096e13f21.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

 

### 2.2 新建规则（类似Mysql新建一张空表）{PUT /索引名}

没有新建文档id，只单纯的新建规则，指定字段类型。类似Mysql新建一张空表。

![img](https://s4.51cto.com/images/blog/202105/06/77af9273cd67c5d58731f4222b8d116a.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

 

## 3.获取操作

### 3.1 得到索引信息 {GET 索引名}

![img](https://s4.51cto.com/images/blog/202105/06/1a02778088d12b1f91f25c47355c35df.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

![img](https://s4.51cto.com/images/blog/202105/06/114a3e00901981d24e8cef4f15d04dc7.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

### 3.2 查看ES默认信息 {GET _cat/XXX}

![img](https://s4.51cto.com/images/blog/202105/06/f1ff47e02c03ef373e40722acc16a571.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 **1、创建一个索引**

> ```
> PUT /索引名/类型名（新版本逐步废弃）/文档ID
> {
> 请求体
> }
> ```

```
PUT /test1/type/1
{
  "name": "xiaoming",
  "age": 13
}
```

![image-20210203092727349](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105143.png)

![image-20210203110817742](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105144.png)

**注意：**

 1、创建的索引名要**小写**

 2、es head中数据浏览不显示内容就换个浏览器试试

#### 2、更新一个索引

修改索引依旧可以使用PUT，此时返回的版本号会增加，"result"会提示update，但如果漏了一些信息，原始信息就会丢失，故现在一般**使用POST来更新索引**

```
POST /test1/type1/1
{
 "doc":{
   "name": "张三"
 }
}
```

![image-20210203115556670](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105145.png)

![image-20210203115639183](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105146.png)

没有写age，它就会保持原样（如果用PUT的话age就直接没了）

通过PUT覆盖老信息

_version会增加，result从created到updated

缺点：如果覆盖的时候，无意间少了一些字段，那么就视同删除了该字段。

![img](https://s4.51cto.com/images/blog/202105/06/b1749fbd692a0754fe8ce6f79e211797.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

通过POST更新信息

优点：只需要列出需要修改的字段即可！

![img](https://s4.51cto.com/images/blog/202105/06/b406a396c4a964af13c6cfba91c09e40.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

 

#### 3、删除一个索引/文档

```
DELETE test1（索引名或文档记录ID）
```

![img](https://s4.51cto.com/images/blog/202105/06/3844db38b4f6c669c69e3089e0add97f.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

你的restful命令，写到索引级别，就删除索引；写到文档级别，就精确删除文档

![img](https://s4.51cto.com/images/blog/202105/06/1956dd3927826640a5058c37ae8b1691.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

 

#### 4、指定类型

**常用的字段类型有：**

●字符串类型
text、keyword
●数值类型
long,. integer, short, byte, double, float, half float, scaled float
●日期类型
date
●te布尔值类型
boolean
●二进制类型
binary.

创建一个t2索引（或者说索引库）但不创建文档，此时称其为一个“规则”

![image-20210203111936090](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105147.png)

查看t2，里面没有值，后续可以往里面放数据

![image-20210203112007717](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105148.png)

通过GET可以查看规则信息

![image-20210203112304214](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105149.png)

GET请求很好用，除了规则，还可以查看索引、文档的信息

**注：**

1、新版本es中正在逐步弃用type，我们创建索引库的时候可以将原来的type换成_doc，这样es就会自动帮我们配置字段类型

如下面的新建的test3：

![image-20210203113420834](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105150.png)

2、查看默认配置命令 GET _cat/indices?v

可以使用这个命令查看集群健康状态等一些信息

![image-20210203114043227](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105151.png)

### 关于文档的操作

#### 基本操作

**1、添加数据**

```json
PUT /dayceng/user/1
{
  "name": "DAYceng",
  "age": "22",
  "desc": "如此生活三十年",
  "tags": ["穷","阿宅","脚本小子"]
}
```

**2、查询（获取，GET）数据**

![image-20210205094004174](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105152.png)

**3、简单搜索**(GET条件查询)

```json
GET /dayceng/user/_search?q=name:条件
```

![image-20210205095050550](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105153.png)

简单的条件查询可根据默认的映射规则产生基本查询结果

（这里的"_score"代表匹配度，分值越高，匹配度越高）

说明：

![image-20210205095252732](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105154.png)

字段name，它的类型是keyword，此时进行搜索，分词器不会对类型为keyword的name进行分词（即如果你搜“丹”是不会返回“丹霞”的结果的），如果类型是text则可以查到

#### **复杂操作（主要是搜索）**

**复杂搜索select（排序、分页、模糊/精准查询、高亮）**

##### **1、使用请求体查询**

一般来说，我们进行搜索不是直接写条件搜索，而是需要构建一个JSON格式的请求体，这样可以设置更多参数以实现定制化的搜索

```json
GET /dayceng/user/_search
{
  "query": {
    "match": {
      "name": "丹霞"
    }
  }
}
```

![image-20210205105110638](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105155.png)

如果有相似关键字的多个结果，他们的分数会有不同

![image-20210205110206156](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105156.png)

hits是我们比较关注的一部分，其中包含：

 索引和文档的信息

 查询的结果总数

 查询出来的具体的文档

 分数：用来判断哪个结果更加符合预期

通过hits我们可以把数据的信息遍历出来，让我们想要的结果优先显示出来

后面使用java操控es，所有的方法和对象就是这里的hits、source等key

##### 2、请求体参数

我们通过在请求体后添加参数的方式实现一些自定义的操作

###### **筛选结果**

只返回特定结果

```
GET /dayceng/user/_search
{
  "query": {
    "match": {
      "name": "丹霞"
    }
  },
   "_source":["name","tags"]
}
```

![image-20210205114109589](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105157.png)

###### **排序**

```json
GET /dayceng/user/_search
{
  "query": {
    "match": {
      "name": "丹霞"
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}
```

注意：排序只能用于数值类型，我这里的age是text类型，运行就会报错

![image-20210205115918067](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105158.png)

> "Text fields are not optimised for operations that require per-document field data like aggregations and sorting, so these operations are disabled by default. Please use a keyword field instead. Alternatively, set fielddata=true on [age] in order to load field data by uninverting the inverted index. Note that this can use significant memory."

把“age”换成“age.keyword”即可正常排序

![image-20210205120324780](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105159.png)

###### **分页**

```json
GET /dayceng/user/_search
{
  "query": {
    "match": {
      "name": "丹霞在"
    }
  },
  "sort": [
    {
      "age.keyword": {
        "order": "desc"
      }
    }
  ],
   "from": 0,---从第几个数据开始
   "size": 2 ---返回几个数据

}
```

![image-20210206093248369](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105200.png)

###### **过滤**

使用filter参数即可

![image-20210206094548128](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105201.png)

 gt：大于

 gte：大于等于

 lt：小于

 lte：小于等于

 （以上条件可混合使用）

##### 3、布尔值查询

使用布尔值可进行多条件查询

**must**：设置的所有match都要匹配才会返回结果

**must not**：返回与设置条件相反的结果

**should**：满足条件之一即可返回结果

```json
GET /dayceng/user/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "DAYceng"
          }
        },
        {
          "match": {
            "age": 22
          }
        }
      ]
    }
  }

}
```

![image-20210206093144473](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105202.png)

##### 4、精确查询

term查询是直接使用倒排索引进行精确查询的

**term与match的比较**

 ·term使用倒排索引直接进行精确查询

 ·match则会使用分词器进行解析后再查询（先分析文档，在通过分析结果进行查询）

**类型text与keyword的比较**

text会使用分词器进行分词后再查询

![image-20210206102310427](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105203.png)

keyword不会拆分特定词语

![image-20210206102247047](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105204.png)

##### 5、高亮查询

使用highlight参数

```
GET /dayceng/user/_search
{
  "query": {
    "match": {
      "name": "丹霞"
    }
  },
  "highlight": {
    "fields": {
      "name":{}
    }
  }
}
```

![image-20210206104615060](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105205.png)

在highlight中也可以自定义高亮标签

```
GET /dayceng/user/_search
{
  "query": {
    "match": {
      "name": "丹霞"
    }
  },
  "highlight": {
    "pre_tags": "<p class=key style='color:red'>", 
    "post_tags": "</p>", 
    "fields": {
      "name":{}
    }
  }
}
```

![image-20210206105030939](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105206.png)











