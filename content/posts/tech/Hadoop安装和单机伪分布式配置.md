---
title: "Hadoop安装和单机伪分布式配置"
date: 2021-08-03T18:34:25+08:00
lastmod: 2021-08-07T22:54:22+08:00
draft: false
categories:
- 技术
- BigData
tags:
- Hadoop
- Getting Started
---

[厦门大学大数据实验室安装教程](http://dblab.xmu.edu.cn/blog/install-hadoop)



```bash
cd /usr/local/hadoop   #进入目录

./sbin/start-dfs.sh     #启动 Hadoop

./sbin/stop-dfs.sh   #关闭Hadoop
```

*注意*

下次启动 hadoop 时，无需进行 NameNode 的初始化，只需要运行 `./sbin/start-dfs.sh` 就可以！

## 安装环境

Ubuntu 18.04 LTS

Hadoop 3.2.1

## 创建hadoop用户

如果你安装 Ubuntu 的时候不是用的 “hadoop” 用户，那么需要增加一个名为 hadoop 的用户。

首先按 **ctrl+alt+t** 打开终端窗口，输入如下命令创建新用户 :

```bash
sudo useradd -m hadoop -s /bin/bash
```

Shell 命令

这条命令创建了可以登陆的 hadoop 用户，并使用 /bin/bash 作为 shell。

**Ubuntu终端复制粘贴快捷键**

在Ubuntu终端窗口中，复制粘贴的快捷键需要加上 shift，即粘贴是 ctrl+shift+v。

接着使用如下命令设置密码，可简单设置为 hadoop，按提示输入两次密码：

```bash
sudo passwd hadoop
```

Shell 命令

可为 hadoop 用户增加管理员权限，方便部署，避免一些对新手来说比较棘手的权限问题：

```bash
sudo adduser hadoop sudo
```

Shell 命令

最后注销当前用户（点击屏幕右上角的齿轮，选择注销），返回登陆界面。在登陆界面中选择刚创建的 hadoop 用户进行登陆。

查看本机IP 

 `ifconfig`

查看本机主机名

`hostname`

查看文件中主机名和公网IP是否正确

`sudo vim /etc/hosts`

使用命令`hostname -i`显示主机的IP地址127.0.1.1

## 安装SSH、配置SSH无密码登陆

集群、单节点模式都需要用到 SSH 登陆（类似于远程登陆，你可以登录某台 Linux 主机，并且在上面运行命令），Ubuntu 默认已安装了 SSH client，此外还需要安装 SSH server：

```bash
sudo apt-get install openssh-server
```

Shell 命令

安装后，可以使用如下命令登陆本机：

```bash
ssh localhost
```

Shell 命令

此时会有如下提示(SSH首次登陆提示)，输入 yes 。然后按提示输入密码 hadoop，这样就登陆到本机了。

![SSH首次登陆提示](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-08-ssh-continue.png)

SSH首次登陆提示

但这样登陆是需要每次输入密码的，我们需要配置成SSH无密码登陆比较方便。

首先退出刚才的 ssh，就回到了我们原先的终端窗口，然后利用 ssh-keygen 生成密钥，并将密钥加入到授权中：

```bash
exit                           # 退出刚才的 ssh localhostcd 

~/.ssh/                     # 若没有该目录，请先执行一次ssh localhost

ssh-keygen -t rsa              # 会有提示，都按回车就可以

cat ./id_rsa.pub >> ./authorized_keys  # 加入授权
```

Shell 命令

**~的含义**

在 Linux 系统中，~ 代表的是用户的主文件夹，即 “/home/用户名” 这个目录，如你的用户名为 hadoop，则 ~ 就代表 “/home/hadoop/”。 此外，命令中的 # 后面的文字是注释，只需要输入前面命令即可。

此时再用 `ssh localhost` 命令，无需输入密码就可以直接登陆了，如下图所示。

![SSH无密码登录](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-09-ssh-localhost.png)

SSH无密码登录

## 安装Java环境

（3）第3种安装JDK方式
 根据大量电脑安装Java环境的情况我们发现，部分电脑按照上述的第一种安装方式会出现安装失败的情况，这时，可以采用这里介绍的另外一种安装方式，命令如下：

```bash
sudo apt-get install default-jre default-jdk
```

Shell 命令

上述安装过程需要访问网络下载相关文件，请保持联网状态。安装结束以后，需要配置JAVA_HOME环境变量，请在Linux终端中输入下面命令打开当前登录用户的环境变量配置文件.bashrc：

```bash
vim ~/.bashrc
```

Shell 命令

在文件最前面添加如下单独一行（注意，等号“=”前后不能有空格），然后保存退出：

```
export JAVA_HOME=/usr/lib/jvm/default-java
```

接下来，要让环境变量立即生效，请执行如下代码：

```bash
source ~/.bashrc    # 使变量设置生效
```

Shell 命令

执行上述命令后，可以检验一下是否设置正确：

```bash
echo $JAVA_HOME     # 检验变量值java -version$JAVA_HOME/bin/java -version  # 与直接执行java -version一样
```

Shell 命令

至此，就成功安装了Java环境。下面就可以进入Hadoop的安装。



## 安装 Hadoop

[hadoop官网](https://hadoop.apache.org)

我们选择将 Hadoop 安装至 /usr/local/ 中：

```bash
sudo tar -zxf ~/下载/hadoop-3.2.1.tar.gz -C /usr/local    # 解压到/usr/local中

cd /usr/local/

sudo mv  ./hadoop-3.2.1/    ./hadoop            # 将文件夹名改为hadoop

sudo chown -R hadoop  ./hadoop       # 修改文件权限
```

Shell 命令

Hadoop 解压后即可使用。输入如下命令来检查 Hadoop 是否可用，成功则会显示 Hadoop 版本信息：

```bash
cd /usr/local/hadoop

./bin/hadoop version
```

Shell 命令

**相对路径与绝对路径**

请务必注意命令中的相对路径与绝对路径，本文后续出现的 `./bin/...`，`./etc/...` 等包含 ./ 的路径，均为相对路径，以 /usr/local/hadoop 为当前目录。例如在 /usr/local/hadoop 目录中执行 `./bin/hadoop version` 等同于执行 `/usr/local/hadoop/bin/hadoop version`。可以将相对路径改成绝对路径来执行，但如果你是在主文件夹 ~ 中执行 `./bin/hadoop version`，执行的会是 `/home/hadoop/bin/hadoop version`，就不是我们所想要的了。

## Hadoop伪分布式配置

Hadoop 可以在单节点上以伪分布式的方式运行，Hadoop 进程以分离的 Java 进程来运行，节点既作为 NameNode 也作为 DataNode，同时，读取的是 HDFS 中的文件。

Hadoop 的配置文件位于 /usr/local/hadoop/etc/hadoop/ 中，伪分布式需要修改2个配置文件 **core-site.xml** 和 **hdfs-site.xml** 。Hadoop的配置文件是 xml 格式，每个配置以声明 property 的 name 和 value 的方式来实现。

修改配置文件 **core-site.xml** (通过 gedit 编辑会比较方便: `gedit ./etc/hadoop/core-site.xml`)，将当中的

```xml
<configuration></configuration>
```

XML

修改为下面配置：

```xml
    <configuration>
        <property>
            <name>hadoop.tmp.dir</name>
            <value>file:/usr/local/hadoop/tmp</value>
            <description>Abase for other temporary directories.</description>
        </property>
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://localhost:9000</value>
        </property>
    </configuration>
```

XML

同样的，修改配置文件 **hdfs-site.xml**：

```xml
    <configuration>
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
        <property>
            <name>dfs.namenode.name.dir</name>
            <value>file:/usr/local/hadoop/tmp/dfs/name</value>
        </property>
        <property>
            <name>dfs.datanode.data.dir</name>
            <value>file:/usr/local/hadoop/tmp/dfs/data</value>
        </property>
    </configuration>
```

XML

*Hadoop配置文件说明*

> Hadoop 的运行方式是由配置文件决定的（运行 Hadoop 时会读取配置文件），因此如果需要从伪分布式模式切换回非分布式模式，需要删除 core-site.xml 中的配置项。
>
> 此外，伪分布式虽然只需要配置 fs.defaultFS 和 dfs.replication 就可以运行（官方教程如此），不过若没有配置  hadoop.tmp.dir 参数，则默认使用的临时目录为  /tmp/hadoo-hadoop，而这个目录在重启时有可能被系统清理掉，导致必须重新执行 format 才行。所以我们进行了设置，同时也指定  dfs.namenode.name.dir 和 dfs.datanode.data.dir，否则在接下来的步骤中可能会出错。

配置完成后，执行 NameNode 的格式化:

```bash
cd /usr/local/hadoop

./bin/hdfs namenode -format
```

Shell 命令

成功的话，会看到 “successfully formatted” 和 “Exitting with status 0” 的提示，若为 “Exitting with status 1” 则是出错。

![执行namenode格式化](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-14-namenode-format.png)执行namenode格式化

如果在这一步时提示 **Error: JAVA_HOME is not set and could not be found.** 的错误，则说明之前设置 JAVA_HOME 环境变量那边就没设置好，请按教程先设置好 JAVA_HOME 变量，否则后面的过程都是进行不下去的。如果已经按照前面教程在.bashrc文件中设置了JAVA_HOME，还是出现 **Error: JAVA_HOME is not set and could not be found.**  的错误，那么，请到hadoop的安装目录修改配置文件“/usr/local/hadoop/etc/hadoop/hadoop-env.sh”，在里面找到“export JAVA_HOME=${JAVA_HOME}”这行，然后，把它修改成JAVA安装路径的具体地址，比如，“export  JAVA_HOME=/usr/lib/jvm/default-java”，然后，再次启动Hadoop。

接着开启 NameNode 和 DataNode 守护进程。

```bash
cd /usr/local/hadoop

./sbin/start-dfs.sh  #start-dfs.sh是个完整的可执行文件，中间没有空格
```

Shell 命令

若出现如下SSH提示，输入yes即可。

![启动Hadoop时的SSH提示](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-15-ssh-continue.png)

启动Hadoop时的SSH提示

启动时可能会出现如下 WARN 提示：WARN util.NativeCodeLoader: Unable to load  native-hadoop library for your platform… using builtin-java classes  where applicable WARN 提示可以忽略，并不会影响正常使用。

启动完成后，可以通过命令 `jps` 来判断是否成功启动，若成功启动则会列出如下进程:  “NameNode”、”DataNode” 和 “SecondaryNameNode”（如果 SecondaryNameNode  没有启动，请运行 sbin/stop-dfs.sh 关闭进程，然后再次尝试启动尝试）。如果没有 NameNode 或 DataNode  ，那就是配置不成功，请仔细检查之前步骤，或通过查看启动日志排查原因。

![通过jps查看启动的Hadoop进程](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-16-jps.png)

通过jps查看启动的Hadoop进程

**Hadoop无法正常启动的解决方法**

一般可以查看启动日志来排查原因，注意几点：

- 启动时会提示形如 “DBLab-XMU: starting namenode, logging to  /usr/local/hadoop/logs/hadoop-hadoop-namenode-DBLab-XMU.out”，其中  DBLab-XMU 对应你的机器名，但其实启动日志信息是记录在  /usr/local/hadoop/logs/hadoop-hadoop-namenode-DBLab-XMU.log  中，所以应该查看这个后缀为 **.log** 的文件；
- 每一次的启动日志都是追加在日志文件之后，所以得拉到最后面看，对比下记录的时间就知道了。
- 一般出错的提示在最后面，通常是写着 Fatal、Error、Warning 或者 Java Exception 的地方。
- 可以在网上搜索一下出错信息，看能否找到一些相关的解决方法。

此外，**若是 DataNode 没有启动**，即Hadoop启动查看jps没有datanode：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021040911394672.png#pic_center)

原因：一般由于多次格式化NameNode导致。在配置文件中保存的是第一次格式化时保存的namenode的ID，因此就会造成datanode与namenode之间的id不一致。

解决方法1：找到datanode带有log的文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210409114546887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTc3NTk0MQ==,size_16,color_FFFFFF,t_70#pic_center)

将文件打开：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210409114904239.png#pic_center)

找到clusterID这一串代码并将其复制

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210409114927846.jpg#pic_center)

打开current的目录下的VERSION文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210409115329834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTc3NTk0MQ==,size_16,color_FFFFFF,t_70#pic_center)

将VERSION中的clusterID改成上面复制的ID

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210409115447890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTc3NTk0MQ==,size_16,color_FFFFFF,t_70#pic_center)

最后重启一下Hadoop就出现datanode了

![r](https://img-blog.csdnimg.cn/20210409115517905.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTc3NTk0MQ==,size_16,color_FFFFFF,t_70#pic_center)



也可尝试如下的方法（注意这会删除 HDFS 中原有的所有数据，如果原有的数据很重要请不要这样做）：

```bash
# 针对 DataNode 没法启动的解决方法cd /usr/local/hadoop./sbin/stop-dfs.sh   # 关闭rm -r ./tmp     # 删除 tmp 文件，注意这会删除 HDFS 中原有的所有数据./bin/hdfs namenode -format   # 重新格式化 NameNode./sbin/start-dfs.sh  # 重启
```

Shell 命令



## 查看WebUI



成功启动后，可以访问 Web 界面 http://localhost:9870 查看 NameNode 和 Datanode 信息，还可以在线查看 HDFS 中的文件。

## 运行Hadoop伪分布式实例

上面的单机模式，grep 例子读取的是本地数据，伪分布式读取的则是 HDFS 上的数据。要使用 HDFS，首先需要在 HDFS 中创建用户目录：

```bash
./bin/hdfs dfs -mkdir -p /user/hadoop
```

Shell 命令

*注意*

> 教材《大数据技术原理与应用》的命令是以”./bin/hadoop dfs”开头的Shell命令方式，实际上有三种shell命令方式。
>
> 1. hadoop fs
> 2. hadoop dfs
> 3. hdfs dfs

> hadoop fs适用于任何不同的文件系统，比如本地文件系统和HDFS文件系统
>  hadoop dfs只能适用于HDFS文件系统
>  hdfs dfs跟hadoop dfs的命令作用一样，也只能适用于HDFS文件系统

接着将 ./etc/hadoop 中的 xml 文件作为输入文件复制到分布式文件系统中，即将  /usr/local/hadoop/etc/hadoop 复制到分布式文件系统中的 /user/hadoop/input 中。我们使用的是  hadoop 用户，并且已创建相应的用户目录 /user/hadoop ，因此在命令中就可以使用相对路径如 input，其对应的绝对路径就是  /user/hadoop/input:

```bash
./bin/hdfs dfs -mkdir input

./bin/hdfs dfs -put ./etc/hadoop/*.xml input
```

Shell 命令

复制完成后，可以通过如下命令查看文件列表：

```bash
./bin/hdfs dfs -ls input
```

Shell 命令

伪分布式运行 MapReduce 作业的方式跟单机模式相同，区别在于伪分布式读取的是HDFS中的文件（可以将单机步骤中创建的本地 input 文件夹，输出结果 output 文件夹都删掉来验证这一点）。

```bash
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar grep input output 'dfs[a-z.]+'
```

Shell 命令

查看运行结果的命令（查看的是位于 HDFS 中的输出结果）：

```bash
./bin/hdfs dfs -cat output/*
```

Shell 命令

结果如下，注意到刚才我们已经更改了配置文件，所以运行结果不同。

![Hadoop伪分布式运行grep结果](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-18-grep-output.png)

Hadoop伪分布式运行grep结果

我们也可以将运行结果取回到本地：

```bash
rm -r ./output    # 先删除本地的 output 文件夹（如果存在）

./bin/hdfs dfs -get output ./output     # 将 HDFS 上的 output 文件夹拷贝到本机

cat ./output/*
```

Shell 命令

Hadoop 运行程序时，输出目录不能存在，否则会提示错误  “org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory  hdfs://localhost:9000/user/hadoop/output already exists”  ，因此若要再次执行，需要执行如下命令删除 output 文件夹:

```bash
./bin/hdfs dfs -rm -r output    # 删除 output 文件夹
```

Shell 命令

*运行程序时，输出目录不能存在*

> 运行 Hadoop 程序时，为了防止覆盖结果，程序指定的输出目录（如 output）不能存在，否则会提示错误，因此运行前需要先删除输出目录。在实际开发应用程序时，可考虑在程序中加上如下代码，能在每次运行时自动删除输出目录，避免繁琐的命令行操作：

```java
    Configuration conf = new Configuration();
    Job job = new Job(conf);
     
    /* 删除输出目录 */
    Path outputPath = new Path(args[1]);
    outputPath.getFileSystem(conf).delete(outputPath, true);
```

若要关闭 Hadoop，则运行

```bash
./sbin/stop-dfs.sh
```

Shell 命令

*注意*

下次启动 hadoop 时，无需进行 NameNode 的初始化，只需要运行 `./sbin/start-dfs.sh` 就可以！



## 附加教程: 配置PATH环境变量

在这里额外讲一下 PATH 这个环境变量（可执行 `echo $PATH` 查看，当中包含了多个目录）。例如我们在主文件夹 ~ 中执行 `ls` 这个命令时，实际执行的是 `/bin/ls` 这个程序，而不是 `~/ls` 这个程序。系统是根据 PATH 这个环境变量中包含的目录位置，逐一进行查找，直至在这些目录位置下找到匹配的程序（若没有匹配的则提示该命令不存在）。

上面的教程中，我们都是先进入到 /usr/local/hadoop 目录中，再执行 `sbin/hadoop`，实际上等同于运行 `/usr/local/hadoop/sbin/hadoop`。我们可以将 Hadoop 命令的相关目录加入到 PATH 环境变量中，这样就可以直接通过 `start-dfs.sh` 开启 Hadoop，也可以直接通过 `hdfs` 访问 HDFS 的内容，方便平时的操作。

同样我们选择在 ~/.bashrc 中进行设置（`vim ~/.bashrc`，与 JAVA_HOME 的设置相似），在文件最前面加入如下单独一行:

```
export PATH=$PATH:/usr/local/hadoop/sbin:/usr/local/hadoop/bin
```

添加后执行 `source ~/.bashrc` 使设置生效，生效后，在任意目录中，都可以直接使用 `hdfs` 等命令了，读者不妨现在就执行 `hdfs dfs -ls input` 查看 HDFS 文件试试看。

## 安装Hadoop集群

在平时的学习中，我们使用伪分布式就足够了。如果需要安装 Hadoop 集群，请查看[Hadoop集群安装配置教程](http://dblab.xmu.edu.cn/blog/install-hadoop-cluster/)。















