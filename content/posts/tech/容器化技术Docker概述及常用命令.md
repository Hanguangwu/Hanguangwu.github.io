---
title: "容器化技术Docker概述及常用命令"
date: 2021-08-13T18:34:25+08:00
lastmod: 2021-08-16T22:54:22+08:00
draft: false
categories:
- 技术
- 容器化
tags:
- Docker
- Getting Started
---

# 容器化技术Docker概述及常用命令

[官网](https://www.docker.com/)

[文档](https://docs.docker.com/)

[Docker Hub is the world's easiest way to create, manage, and deliver your teams' container applications.](https://hub.docker.com/)

[Docker使用入门到进阶 常用命令+网络+实战](https://www.jyoryo.com/archives/295.html)

Docker 是 Docker 公司开源的一个基于轻量级虚拟化技术的容器引擎项目, 整个项目基于 Go 语言开发，并遵从 Apache 2.0 协议。目前，Docker 可以在容器内部快速自动化部署应用，并可以通过内核虚拟化技术（namespaces 及 cgroups 等）来提供容器的资源隔离与安全保障等。由于 Docker 通过操作系统层的虚拟化实现隔离，所以 Docker 容器在运行时，不需要类似虚拟机（VM）额外的操作系统开销，提高资源利用率，并且提升诸如 IO 等方面的性能。

目的：通过容器化隔离机制解决开发运维中程序运行环境配置问题

Docker和虚拟机区别：

> Docker只虚拟软件，不虚拟硬件，是内核级别的虚拟化
>
> 虚拟机既虚拟软件，也虚拟硬件

## 基本概念

Docker基本组成：

![Cover image for Getting Started with Docker for Developers](https://cdn.jyoryo.com/oss/ac/7c64fi83if4.png)

### 镜像 image

docker 镜像好比一个模板，可以通过这个目标创建容器服务。比如：`nginx`镜像 ===》`run`命令 ===》`nginx01`容器（提供服务）

### 容器 container

docker利用容器技术，独立运行一个或一组应用。容器是通过镜像创建的。

容器基本命令有：启动、停止、删除

### 仓库 repository

仓库是用来存放镜像的地方。

仓库可以分为：共有仓库、私有仓库。

## Docker安装及卸载

卸载旧版本

`sudo apt-get remove docker docker-engine docker.io containerd runc`

安装

`sudo apt-get install docker-ce docker-ce-cli containerd.io`

卸载Docker Engine, CLI, and Containerd packages

`sudo apt-get purge docker-ce docker-ce-cli containerd.io`

主机上的镜像、容器、卷或自定义配置文件不会自动删除。删除所有镜像、容器和卷：

```shell
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

必须自己手动删除任何已编辑的配置文件。

### Ubuntu18.04LTS Snap版docker修改镜像源

```shell
sudo vim /var/snap/docker/796/config/daemon.json

{
    "log-level":        "error",
    "storage-driver":   "overlay2",
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}

sudo service snap.docker.dockerd restart
```

普通版本修改docker镜像仓库配置

修改/etc/docker/daemon.json文件，如果没有先建一个即可

```shell
sudo vim /etc/docker/daemon.json
```

修改配置文件

```shell
{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

使配置文件生效

```shell
sudo systemctl daemon-reload
```

重启Docker

```shell
sudo service docker restart
```

测试配置是否成功

```shell
docker search nginx
```



## 常用命令

命令网址 https://docs.docker.com/engine/reference

![r](https://www.jyoryo.com/usr/uploads/2021/03/917301888.png)

### 帮助命令

```shell
docker version  #显示docker版本信息
docker info  #显示docker系统信息，包括镜像和容器的数量，
docker command --help  #万能帮助命令
```



### 镜像命令

1.列出本地主机上所有镜像

`docker images [OPTIONS] [REPOSITORY[:TAG]]`

可选项Options

| Name, shorthand   | Default | Description                                         |
| ----------------- | ------- | --------------------------------------------------- |
| `--all` , `-a`    |         | Show all images (default hides intermediate images) |
| `--digests`       |         | Show digests                                        |
| `--filter` , `-f` |         | Filter output based on conditions provided          |
| `--format`        |         | Pretty-print images using a Go template             |
| `--no-trunc`      |         | Don't truncate output                               |
| `--quiet` , `-q`  |         | Only show image IDs                                 |

本地镜像

```txt
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
hello-world             latest              d1165f221234        5 months ago        13.3kB
ricklamers/gridstudio   release             b72639f5bf32        10 months ago       1.51GB
```



2.搜索镜像

`docker search [OPTIONS] TERM`

Options

| Name, shorthand   | Default | Description                                |
| ----------------- | ------- | ------------------------------------------ |
| `--filter` , `-f` |         | Filter output based on conditions provided |
| `--format`        |         | Pretty-print search using a Go template    |
| `--limit`         | `25`    | Max number of search results               |
| `--no-trunc`      |         | Don't truncate output                      |

如`docker search mysql --filter=STARS=3000`

本地输出

```shell
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql               MySQL is a widely used, open-source relation…   11258               [OK]                
mariadb             MariaDB Server is a high performing open sou…   4277                [OK]                
```

3.下载镜像

`docker pull [OPTIONS] NAME[:TAG|@DIGEST]`

Options

| Name, shorthand           | Default | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| `--all-tags` , `-a`       |         | Download all tagged images in the repository                 |
| `--disable-content-trust` | `true`  | Skip image verification                                      |
| `--platform`              |         | [**API 1.32+**](https://docs.docker.com/engine/api/v1.32/) Set platform if server is multi-platform capable |
| `--quiet` , `-q`          |         | Suppress verbose output                                      |

```shell
docker pull mysql    #等价于 docker pull  docker.io/library/mysql:latest

Using default tag: latest  #如果不写Tag默认是latest
latest: Pulling from library/mysql
33847f680f63: Pull complete   #分层下载, docker imgae的核心 联合文件系统
5cb67864e624: Pull complete 
1a2b594783f5: Pull complete 
b30e406dd925: Pull complete 
48901e306e4c: Pull complete 
603d2b7147fd: Pull complete 
802aa684c1c4: Pull complete 
715d3c143a06: Pull complete 
6978e1b7a511: Pull complete 
f0d78b0ac1be: Pull complete 
35a94d251ed1: Pull complete 
36f75719b1a9: Pull complete 
Digest: sha256:8b928a5117cf5c2238c7a09cd28c2e801ac98f91c3f8203a8938ae51f14700fd #签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest# 真实地址 

```

```shell
docker pull mysql:5.7
```



4.删除镜像

`docker rmi [OPTIONS] IMAGE [IMAGE...]`

Options

| Name, shorthand  | Default | Description                    |
| ---------------- | ------- | ------------------------------ |
| `--force` , `-f` |         | Force removal of the image     |
| `--no-prune`     |         | Do not delete untagged parents |

```shell
docker rmi  -f 镜像id   # 删除指定的镜像
docker rmi test:latest # 删除指定的镜像

docker rmi -f  镜像id  镜像id    # 删除多个指定的镜像

docker rmi -f $(docker images -aq) #删除所有的镜像

```



### 容器命令

有了镜像才能创建容器

1.新建容器并启动

` docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

```shell
docker run [可选参数] image

#常用参数说明

--name="Name"  容器名字   tomcat1 tomcat2 用来区分容器
-d                          后台方式运行
-it                          使用交互方式运行，进入容器查看内容
-p                      指定容器的端口 -p  8080:8080
				-p  ip:主机端口:容器端口
				-p  主机端口:容器端口
				-p  容器端口
				容器端口
-P          随机指定端口

# 测试，启动并进入容器
docker run -it  centos /bin/bash

#从容器中返回主机
exit
```

后台启动容器

```shell
# 命令：  docker run -d  镜像名
docker run -d  centos

#问题  docker ps 发现centos停止了

#常见的坑，docker容器使用后台运行，就必须有要有一个前台进程，docker发现没有应用就会自动停止

#nginx 容器启动後，发现自己没有提供服务，就会立刻停止，就是没有程序了
```

测试hello-world  `sudo docker run hello-world`

输出文档：

```txt
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```



2.列出所有运行的容器

` docker ps [OPTIONS]`

Options

| Name, shorthand   | Default | Description                                             |
| ----------------- | ------- | ------------------------------------------------------- |
| `--all` , `-a`    |         | Show all containers (default shows just running)        |
| `--filter` , `-f` |         | Filter output based on conditions provided              |
| `--format`        |         | Pretty-print containers using a Go template             |
| `--last` , `-n`   | `-1`    | Show n last created containers (includes all states)    |
| `--latest` , `-l` |         | Show the latest created container (includes all states) |
| `--no-trunc`      |         | Don't truncate output                                   |
| `--quiet` , `-q`  |         | Only display container IDs                              |
| `--size` , `-s`   |         | Display total file sizes                                |

3.退出容器

```shell
exit    #停止容器并退出

Ctrl +P+Q #容器不停止退出
```

4.删除容器

`docker rm [OPTIONS] CONTAINER [CONTAINER...]`

Options

| Name, shorthand    | Default | Description                                             |
| ------------------ | ------- | ------------------------------------------------------- |
| `--force` , `-f`   |         | Force the removal of a running container (uses SIGKILL) |
| `--link` , `-l`    |         | Remove the specified link                               |
| `--volumes` , `-v` |         | Remove anonymous volumes associated with the container  |

```shell
docker rm 容器id #删除指定容器，不能删除正在运行的容器

docker rm  -f $(docker ps -aq) #删除所有容器

docker ps -a -q|xargs  docker rm #删除所有容器
```

5.启动和停止容器

```shell
docker start 容器id # 启动容器

docker restart 容器id #重启容器

docker stop 容器id #停止当前正在运行的容器

docker kill 容器id #强制停止当前容器
```

注：docker run 只在第一次运行时使用，将镜像放到容器中，以后再次启动这个容器时，只需要使用命令docker start 即可。docker run相当于执行了两步操作：将镜像放入容器中（docker create）,然后将容器启动，使之变成运行时容器（docker start）。



6.查看日志

`docker logs [OPTIONS] CONTAINER`

Options

| Name, shorthand       | Default | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| `--details`           |         | Show extra details provided to logs                          |
| `--follow` , `-f`     |         | Follow log output                                            |
| `--since`             |         | Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes) |
| `--tail` , `-n`       | `all`   | Number of lines to show from the end of the logs             |
| `--timestamps` , `-t` |         | Show timestamps                                              |
| `--until`             |         | [**API 1.35+**](https://docs.docker.com/engine/api/v1.35/) Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes) |

```shell
#自己编写一段shell脚本
docker run -d centos /bin/bash -c "while true; do echo RM; sleep 1; done"

docker ps #查看容器id

#显示日志
-tf  #显示日志
--tail  number #要显示日志条数
docker logs -tf --tail 10  XXXXXXXXX
```

7.查看容器中进程信息ps

`docker top CONTAINER [ps OPTIONS]`

```shell
docker top 容器id
```

8.查看镜像的元数据

`docker inspect [OPTIONS] NAME|ID [NAME|ID...]`

`docker inspect 容器id`

9.进入当前正在运行的容器

```shell
#容器通常都是使用后台方式运行，需要进入容器，修改一些配置
#第一种
docker exec -it 容器id /bin/bash   #进入容器後打开一个新的终端，可以在里面操作（常用）

#第二种
docker attach 容器id   #进入容器正在执行的终端，不会启动新的进程
```

10.从容器内拷贝文件到主机上

`docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-`

```shell
docker cp 容器id:容器内路径  目的主机:目的路径

touch hello.java  #创建文件

docker cp a72be5a7db55:/hello.java /home/RM/java

# 拷贝是一个手动过程，之後可使用 volume卷技术，可以实现自动同步
```

## **Docker相似的命令与区别**

[Docker相似的命令与区别](https://www.imooc.com/article/31928)

### kill vs stop

两个命令都是停止docker，不同之处在于：

- `docker stop`: 先发`SIGTERM`信号给docker，允许其在一定时间（默认10s）内进行一些操作（例如资源回收），若这段时间内容器未停止，则发送`SIGKILL`信号强行杀死容器；
- `docker kill`: 直接发送`SIGKILL`信号杀死容器。

`SIGTERM`与`SIGKILL`的区别在于，前者是告知你的租期到了，请你赶紧收拾行李离开；后者是你的租期到了，直接将你扫地出门。

简而言之，相比kill，stop给了容器自行处理结束的时间，**更为优雅**。

### run vs start

- `docker run`: 从镜像启动一个容器;
- `docker start`: 运行已停止的容器。

例如你需要从`docker images`列表中启动一个镜像，你需要执行

```
docker run <IMAGE ID>
```

你在该镜像中做了某些修改，然后结束（kill/stop）该容器，通过`docker ps -a`可以看到一个状态为*EXITED*的容器，此时你可以通过执行

```
docker start <CONTAINER ID>
```

再次启动该容器。通过start启动的容器会保留上一次结束前做的变动，不会像run一样执行的是全新的容器。

### export vs save

- `docker export`: 持久化**容器**，类似给一个虚拟机打快照，会保留容器启动后的一系列修改；
- `docker save`: 持久化一个**镜像**，只是将镜像导出，如果通过镜像启动容器，那么容器中的修改并不会写入镜像中，也因此不会被save。

要注意的是，export导出后的镜像会丢失历史，等于没法回滚到以前的某个状态，而save则保留全部的历史；也因此，export导出后的镜像更小。

另外，export一次只能导出一个容器，而save可以一次性导出多个镜像。

实际使用中，仅仅导出镜像使用save，而如果对容器做了修改后需要保存，可以export，也可以先commit再save。

### import vs load

- `docker import`: 从一个tar包创建一个镜像，可以指定新的NAME:TAG；
- `docker load`: 可以同时将多个tar包导入到仓库中，不可指定新的NAME:TAG。

二者几乎与export/save相呼应，若将save保存的tar包镜像用import导入回去，那么新生成的镜像会丢失原来镜像的历史。

### rm vs rmi

- `docker rm <CONTAINER ID>`: 删除容器
- `docker rmi <IMAGE ID>`: 删除镜像

### image vs container

如果你是一个刚接触Docker的新手，那么你可能会对这两者有疑惑。

其实很容易区分二者，`image`即镜像，类似操作系统的iso文件，而`container`则是用这iso文件安装好的一个操作系统。

### 可视化面板

先用Portainer，再用Rancher（CI/CD)

Portainer

Docker图形化界面管理工具，提供一个后台面板供我们操作

### Docker镜像加速原理

分层的联合文件系统UnionFS

bootfs主要包括bootloader和kernel

rootfs主要包括/dev，/proc，/bin，/etc

从容器中创建一个新的镜像

`docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`

OPTIONS说明：

-a: 提交镜像的作者（例如："John Hannibal Smith <hannibal@a-team.com>"）；
-c: 使用Dockerfile指令来创建镜像；
-m: 提交的说明信息；
-p: 提交镜像时将容器暂停（默认：true）。

通过容器创建镜像
`docker commit -m="提交的说明信息" -a="作者" 容器id 目标镜像:[TAG]`

```shell
docker commit -a="RM" -m="add webapps app"  7c163f4bc111 tomcat01:1.0
```

测试结果输出内容：

```txt
docker images

REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
tomcat01                       1.0                 952fcdeea9b5        19 seconds ago      673MB

```

## 容器数据卷

数据保存持久化需求==>数据保存在本地==>容器之间数据共享即Docker容器中产生的数据同步到本地

挂载目录：将容器内的目录挂载到linux上

```shell
#指定路径挂载
 -v /主机目录: 容器内目录
 
# 具名挂载
-v juming-nginx :容器内路径

# 匿名挂载
-v 容器内路径

#通过rw，ro改变读写权限
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx#ro表示这个路径只能通过宿主机来操作，容器内部无法操作
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx
```



### 使用方式

`docker run -it -v 主机目录: 容器内目录 /bin/bash`

如执行`docker run -it -v /home/naesar/java:/home centos /bin/bash`

查看是否挂载成功`docker inspect 525f30265bd4`

输出内容：

```txt
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/RM/test",
                "Destination": "/home",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],

```

测试生成文件`touch test.java`

同步即双向绑定

### 同步mysql数据

`docker run -d -p 3310:3306 -v /home/RM/mysql/conf:/etc/mysql/conf.d -v /home/RM/mysql/data:/var/bin/mysql -e MYSQL_ROOT_PASSWORD=XXXXXXX --name mysql01  mysql:5.7`

打开mysql workbench新增connection输出如下内容

```txt
Successfully made the MySQL connection

Information related to this connection:

Host: 127.0.0.1
Port: 3310
User: root
SSL: enabled with ECDHE-RSA-AES128-GCM-SHA256

A successful MySQL connection was made with
the parameters defined for this connection.
```



### 具名挂载

-v juming-nginx :容器内路径

如`docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx`

查看目录信息`docker volume inspect juming-nginx`

所有的Docker容器内的卷在没有指定目录的情况下都在/var/lib/docker/volumes/xxxx（snap安装的在/snap/docker）

具名挂载便于查找



### 匿名挂载

-v 容器内路径， 不写主机路径，自动生成主机路径

如`docker run -d -P --name nginx01 -v /etc/nginx nginx`

查看卷情况 `docker volume ls` 

Manage volumes: `docker volume COMMAND`

Commands:

```
 create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes
```

### 数据卷之DockerFile

Dockerfile是创建Docker镜像的文件，实质是命令脚本，一层一层生成镜像

```shell
mkdir docker-volume

cd docker-volume
#创建名字建议Dockerfile
sudo vim dockerfile1
#文件中内容 命令（大写） 参数
FROM centos

VOLUME ["volume01", "volume02"]

CMD echo "----end----"
CMD /bin/bash

cat dockerfile1
```

匿名挂载方式

`docker build -f /home/RM/docker-volume/dockerfile1 -t kuangshen007/centos:1.0  .`

输出内容

```txt
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 300e315adb2f
Step 2/4 : VOLUME ["volume01", "volume02"]
 ---> Running in 43876596e9d1
Removing intermediate container 43876596e9d1
 ---> 8abb35ce8c3a
Step 3/4 : CMD echo "----end----"
 ---> Running in fc526bfc2c86
Removing intermediate container fc526bfc2c86
 ---> 4977f6d53f7b
Step 4/4 : CMD /bin/bash
 ---> Running in a158b397abdb
Removing intermediate container a158b397abdb
 ---> 93c6077695ac
Successfully built 93c6077695ac
Successfully tagged kuangshen007/centos:1.0

```

```txt
REPOSITORY                     TAG                 IMAGE ID            CREATED              SIZE
kuangshen007/centos            1.0                 93c6077695ac        About a minute ago   209MB

```

测试：`docker run -it 93c6077695ac /bin/bash`

ls输出内容

```txt
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var	   volume02
dev  home  lib64  media       opt  root  sbin  sys  usr  volume01#目录生成镜像时自动挂载数据卷目录
```

volume01和volume02肯定有同步的目录！

`cd volume01 && touch container.txt`

执行`docker inspect 3bd82cf6b7e8`

输出内容

```txt
 "Mounts": [
            {
                "Type": "volume",
                "Name": "8e0ee95fde8a1efd5d480d9acde5e8dffcfa2787a3a498101b61a105c7e96c81",
                "Source": "/var/snap/docker/common/var-lib-docker/volumes/8e0ee95fde8a1efd5d480d9acde5e8dffcfa2787a3a498101b61a105c7e96c81/_data",
                "Destination": "volume01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "59f83ef307cadd859f5d86046e1814763214033e6ef0f41766a710324fcca909",
                "Source": "/var/snap/docker/common/var-lib-docker/volumes/59f83ef307cadd859f5d86046e1814763214033e6ef0f41766a710324fcca909/_data",
                "Destination": "volume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],

```

测试一下刚才的文件是否同步出去了

`cd /var/snap/docker/common/var-lib-docker/volumes/8e0ee95fde8a1efd5d480d9acde5e8dffcfa2787a3a498101b61a105c7e96c81/_data`

如果构建镜像时没有挂载卷，要手动挂载， -v 卷名:容器内路径

### 数据卷容器

`--volumes-from  父容器dockerfile`

实现数据同步即两个或多个容器之间实现数据共享

开启第一个

`docker run -it --name docker01 kuangshen007/centos:1.0`

类似下图

![r](https://cdn.jyoryo.com/oss/18/7j9xw89w9a8.png)

开启第二个

`docker run -it --name docker02 --volumes-from docker01 kuangshen007/centos:1.0`

类似下图

![r](https://cdn.jyoryo.com/oss/9c/7jan04enpq8.png)

在docker01中执行`cd volume01&& touch docker01  `发现docker02/volume01中也有docker01

多个共享容器中只要还有一个容器在被使用，数据就不会丢失

例如多个mysql容器实现同步数据

```shell
docker run -d -p 3310:3306 -v  /etc/mysql/conf.d -v  /var/bin/mysql -e MYSQL_ROOT_PASSWORD=XXXXXXX --name mysql01  mysql:5.7

docker run -d -p 3310:3306  -e MYSQL_ROOT_PASSWORD=XXXXXXX --name mysql02 --volumes-from mysql01  mysql:5.7
```

结论：容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器再使用为止。但是数据一旦持久化到本地後，本地数据是不会被删除的

## Dockerfile

### DockerFile介绍

Dockerfile 就是用来构建`docker`镜像的构建文件。

构建步骤：

1. 编写`Dockerfile`文件
2. `docker build`构建成为一个镜像
3. `docker run`运行镜像
4. `docker push`发布镜像（DockerHub、阿里云镜像仓库）

![image-20210318011654334](https://cdn.jyoryo.com/oss/06/7jdi4fo4zcw.png)

![image-20210318011807584](https://cdn.jyoryo.com/oss/f4/7jdm0th7chs.png)

背景：很多官方镜像都是基础包，很多功能都没有，我们通常会根据自己的需要搭建自己的镜像。

再比如[sig-cloud-instance-images](https://github.com/CentOS/sig-cloud-instance-images/tree/ccd17799397027acf9ee6d660e75b8fce4c852e8)/[docker](https://github.com/CentOS/sig-cloud-instance-images/tree/ccd17799397027acf9ee6d660e75b8fce4c852e8/docker)/**Dockerfile**

```shell
FROM scratch
ADD centos-8-x86_64.tar.xz /
LABEL org.label-schema.schema-version="1.0"     org.label-schema.name="CentOS Base Image"     org.label-schema.vendor="CentOS"     org.label-schema.license="GPLv2"     org.label-schema.build-date="20201204"
CMD ["/bin/bash"]
```

### Dockerfile构建过程

基础内容：

1.每个保留关键字（指令）都必须大写

2.顺序执行每条指令

3.#表示注释

4.每一个命令都会创建提交一个新的镜像层并提交

![r](https://cdn.jyoryo.com/oss/b5/7cc0qnhs0sg.png)



步骤：开发、部署、运维……

`Dockerfile`是面向开发的，发布项目时做镜像，就需要编写dockerfile文件。

`Dockerfile`镜像逐渐成为企业交付的标准，必须熟练掌握。

步骤：

**DockerFile**：构建文件，定义所有步骤，相当于镜像源代码。

**Docker Image镜像**：通过DockerFile构建生成的镜像，最终发布和运行的产品。

**Docker Container容器**：通过镜像运行起来提供的服务。

### Dockerfile指令

```dockerfile
FROM                  # 基础镜像，一切从这里开始
MAINTAINER            # 镜像的作者。 姓名+邮箱
RUN                   # 镜像构建时需要运行的命令
ADD                   # 添加内容。比如制作tomcat镜像，需要添加tomcat压缩包
WORKDIR               # 镜像工作的目录
VOLUME                # 挂载的目录
EXPOSE                # 暴露端口配置
CMD                   # 指定容器启动的时候要运行的命令。只有最后一个会生效，可被替代
ENTRYPOINT            # 指定容器启动的时候要执行的命令，可以追加命令
ONBUILD               # 当构建一个被继承 DockerFile 这个时候就会运行 ONBUILD 指令。触发指令
COPY                  # 类似 ADD ，将我们的文件拷贝到镜像中
ENV                   # 构建的时候设置环境变量    
```

![r](https://cdn.jyoryo.com/oss/a8/7je7kywwfsw.png)



### Dockerfile实战----创建自己的Dockerfile

1、编写Dockerfile文件

```dockerfile
FROM centos
MAINTAINER RM<XXX@gmail.com>

ENV MYPATH /usr/local 
WORKDIR $MYPATH 

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash  #CMD ["/bin/bash"]
```

2、通过编写好的Dockerfile构建镜像

```shell
docker build -f mydockerfile_centos -t mycentos:0.1 .
# -f 指定需要构建的Dockerfile文件路径
# -t 镜像名:tag
```

3、根据镜像运行容器

![image-20210318021820609](https://cdn.jyoryo.com/oss/61/7jizi16gg74.png)

通过`docker history`获取镜像变更历史：

（我们可以通过查看现有的镜像变更历史，来研究它们是怎么做的）

![image-20210318022120568](https://cdn.jyoryo.com/oss/ff/7jj91klq800.png)

#### `CMD`和`ENTRYPOINT`区别

- `CMD`:指定这个容器启动的时候要执行的命令，只有最后一个会生效。
- `ENTRYPOINT`:指定这个容器启动的时候要运行的命令，可以追加命令。我们追加的命令，是直接拼接在`ENTRYPOINT`命令后面的。

使用`CMD`:

Dockerfile内容：

```dockerfile
FROM centos
CMD ["ls","-a"]
```

![image-20210318195902515](https://cdn.jyoryo.com/oss/8a/7m5lzmkjif4.png)

使用`ENTRYPOINT`:

Dockerfile内容

```dockerfile
FROM centos
ENTRYPOINT ["ls","-a"]
```

![image-20210318200837941](https://cdn.jyoryo.com/oss/9e/7m6gqq9fj7k.png)

![image-20210318200926153](https://cdn.jyoryo.com/oss/7a/7m6j59ws5q8.png)

#### 创建自己的`Tomcat`镜像

1、准备好需要的文件：Tomcat、JDK或JRE压缩包

![image-20210318202823718](https://cdn.jyoryo.com/oss/b6/7m88c9sgohs.png)

2、编写Dockerfile文件。官方命名`Dockerfile`，build会自动寻找这个文件，就不需要通过`-f`指定dockerfile文件。

```shell
FROM centos
MAINTAINER RM<xxx@qq.com>

COPY readme.txt /usr/local/readme.txt

ADD apache-tomcat-10.0.10.tar.gz /usr/local
ADD jdk-16.0.2_linux-x64_bin.tar.gz /usr/local

RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk-16.0.2
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-10.0.10
ENV CATALINA_BASH /usr/local/apache-tomcat-10.0.10
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-10.0.10/bin/startup.sh && tail -F /usr/local/apache-tomcat-10.0.10/bin/logs/catalina.out
```

3、构建镜像

```shell
docker build -t mydiytomcat:0.1 .
```

![image-20210318211228143](https://cdn.jyoryo.com/oss/09/7mc5ixjltkw.png)

4、启动镜像

```shell
docker run -d -p 9090:8080 -v /home/XXX/docker/data/test:/usr/local/apache-tomcat-10.0.10/webapps/test -v /home/XXX/docker/data/tomcatlog:/usr/local/apache-tomcat-10.0.10/logs --name mytomcat03 mydiytomcat:0.1
```

5、访问

`curl localhost:9090`

类似下图

![image-20210318213251549](https://cdn.jyoryo.com/oss/2f/7mdz2l8j5s0.png)

6、本地增加页面

`cd docker/data/test && sudo mkdir WEB.INF `

`cd WEB.INF && sudo vim web.xml`

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</web-app>
```

`cd .. & sudo vim index.jsp`

index.jsp

```html
<html>
<head><title>Hello World</title></head>
<body>
Hello World!<br/>
<%
System.out.println("my test tomcat web app");
%>
</body>
</html>
```

访问`http://local:9090/test/`页面显示Hello World

### 发布自己的镜像

> 发布镜像至`DockerHub`

1、需要有自己的账号

2、通过`docker login`命令登录

![image-20210318225351531](https://cdn.jyoryo.com/oss/eb/7ml7cyd4z5s.png)

3、使用`docker push`发布镜像

提交的时候是按镜像层级进行提交的。

```shell
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```

![image-20210318230703916](https://cdn.jyoryo.com/oss/b4/7mmdnxtxm9s.png)

> 发布镜像至阿里云镜像服务

1、登录阿里云账号进入**镜像服务**

![image-20210318231550025](https://cdn.jyoryo.com/oss/b0/7mn5phb3cao.png)

2、创建命名空间

![image-20210318232048268](https://cdn.jyoryo.com/oss/34/7mnlpkqjp4w.png)

3、创建镜像仓库

![image-20210318231859748](https://cdn.jyoryo.com/oss/22/7mnfw482br4.png)

![image-20210318232012088](https://cdn.jyoryo.com/oss/4b/7mnjosx1hj4.png)

![image-20210318232113841](https://cdn.jyoryo.com/oss/2f/7mnn2bu7bi8.png)

4、获取仓库信息

![image-20210318232421986](https://cdn.jyoryo.com/oss/47/7mnxb8buxhc.png)

5、通过`docker login`登录

![image-20210318232953907](https://cdn.jyoryo.com/oss/fc/7moevru7kzk.png)

6、使用`docker push`发布镜像

![image-20210318233719660](https://cdn.jyoryo.com/oss/69/7mp2vu6988w.png)

### Dockerfile小结

![Figure0](https://cdn.jyoryo.com/oss/2c/7mpxtwcgpa8.png)

## Docker网络

### docker0网络

通过命令`docker network`查看docker网络相关信息。

```shell
root@docker00:/home/docker# docker network --help

Usage:    docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
root@docker00:/home/docker# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
41f5dee77e3e        bridge              bridge              local
2942fab7a5f0        host                host                local
8dc183da36de        none                null                local
```

![image-20210319000044013](https://cdn.jyoryo.com/oss/ce/7mr6299qm80.png)

启动一个容器：

```shell
root@docker00:/home/docker# docker run -d -P --name tomcat01 tomcat

# 查看容器内网络地址 ip addr
# 可以看到容器启动的时候会得到一个 eth0@if19 ip地址。 这个是docker分配的
root@docker00:/home/docker# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
18: eth0@if19: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

# 通过宿主机可以ping同docker容器
root@docker00:/home/docker# ping -c 4 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.186 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.080 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.112 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.208 ms

--- 172.17.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3022ms
rtt min/avg/max/mdev = 0.080/0.146/0.208/0.053 ms
```

![image-20210319002038308](https://cdn.jyoryo.com/oss/68/7msy3xvnf9c.png)

再启动一个容器：又多了一对网卡。

![image-20210319002947765](https://cdn.jyoryo.com/oss/59/7mtrdcm43cw.png)

tomcat01和tomcat02相互能ping通`docker exec -it tomcat02 ping 172.17.0.2`

查看docker0网卡信息`docker network inspect 804cd60f60ac`

![image-20210319010306580](https://cdn.jyoryo.com/oss/87/7mwqemwsnwg.png)

> 原理说明

1. 只要安装了docker，就会有一个网卡`docker0`，桥接模式；
2. 每启动一个docker容器，docker就会给容器分配一个ip；
3. 每启动一个docker容器，docker带来的网卡都是成对的，使用的技术是`veth-pair`技术；
4. 多个docker容器是公用一个路由器，即`docker0`；
5. 所有容器如果不指定网络的情况下，都是`docker0`路由的，docker会给我们的容器分配一个默认的可用**IP**。
6. docker中所有的网络接口都是虚拟的。虚拟的网络接口转发效率高。
7. 只要容器删除，对应的一对网桥就没了。

![network.png](https://cdn.jyoryo.com/oss/88/7mv80uwrlz4.png)

### veth-pair技术

`veth-pair`就是一对虚拟设备接口，它们都是成对出现的，一端连着协议，另一端彼此相连。

我们可以利用这个特性，使`veth-pair`充当一个桥梁，连接各种虚拟网络设备的。



### --link

问题：有一个服务连接数据库，要做到在项目不重启的前提下更换数据库ip。

解决方法：我们可以通过名称进行访问。

直接用`docker exec -it tomcat02 ping tomcat01`报错ping: tomacat10: No address associated with hostname

```shell
root@docker00:/home/docker# docker run -d -P --name tomcat03 --link tomcat02 tomcat
017be3dbf8a6f94c48cb58afa704dde9900a676eae343cc670ddc3e2d996a557
root@docker00:/home/docker# docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.194 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.094 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.140 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=4 ttl=64 time=0.207 ms

#查看hosts配置
root@docker00:/home/docker# docker exec -it tomcat03 cat /etc/hosts
127.0.0.1    localhost
::1    localhost ip6-localhost ip6-loopback
fe00::0    ip6-localnet
ff00::0    ip6-mcastprefix
ff02::1    ip6-allnodes
ff02::2    ip6-allrouters
172.17.0.3    tomcat02 8666f2a63042
172.17.0.4    017be3dbf8a6
```

**--link**本质：在hosts配置中增加一条记录，比如上面的`172.17.0.3 tomcat02 8666f2a63042`。

注意：--link现在不建议使用。

docker0的问题：不支持容器名访问。

### 自定义网络

**网络模式：**

1. bridge:桥接模式（默认，我们自定义网络也适用bridge模式）
2. none:不配置网络
3. host:和宿主机共享网络
4. container:容器网络连通（用得少，局限比较多）

```shell
# 直接启动容器的命令 --net bridge, 这个bridge就是docker0网络
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat01 --net bridge tomcat

# docker0的特点：默认不能通过域名访问， --link可以打通链接。
```

docker 网络相关命令：`docker network`和`docker network create`

![image-20210319012017286](https://cdn.jyoryo.com/oss/59/7my9f78h9ts.png)

定义自己的网络：

```shell
docker network create --driver bridge --subnet 192.168.6.0/24 --gateway 192.168.6.1 mynet
# --driver bridge   定义网络模式
# --subnet 192.168.6.0/24 子网
# --gateway 192.168.6.1 网关
root@docker00:/home/docker# docker network create --driver bridge --subnet 192.168.6.0/24 --gateway 192.168.6.1 mynet
3c9fe291cfbe918693055e85551a3a1b1e5894cdf3c1742f873e3d2f9c7de232
root@docker00:/home/docker# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
804cd60f60ac        bridge              bridge              local
e9112dc56247        host                host                local
3c9fe291cfbe        mynet               bridge              local
9212c1c4c5f7        none                null                local
root@docker00:/home/docker # docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "3f4ba4b2b80fd55b78f5fe366039ac1221651380435035b1764f0f60c72c834e",
        "Created": "2021-08-15T16:31:32.22308705+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]

root@docker00:/home/docker# docker run -d -P --name tomcat01 --net mynet tomcat
ac83ca2e3a007e49d3a76eee4e51357ff54f7a320bb4d595094f459de08e75f1
root@docker00:/home/docker# docker run -d -P --name tomcat02 --net mynet tomcat
0f7a6d2165bdb488ae58ef6cf42d35fd42bc5afbd88adaa29f5b70033c7a1d0a

root@docker00:/home/docker # docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "3f4ba4b2b80fd55b78f5fe366039ac1221651380435035b1764f0f60c72c834e",
        "Created": "2021-08-15T16:31:32.22308705+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "c0320bd423bffcce26a078d07841554ea7cc44309384f0b3cb3c7b2b6d564fdf": {
                "Name": "tomcat-net-01",
                "EndpointID": "3154b28298637ecef0466aa41917dfcf54046abd6a7eb05624b0dbabf26c2aeb",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "d3d960e0044a02d439fd0f87f8664209af8f935ce8ef9f0fecd5d8273ff9ee6d": {
                "Name": "tomcat-net-02",
                "EndpointID": "653d462c40a6ee26e3f63a2c8f010bc03cda88a989a03095c3065d0c51ecd04e",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]


root@docker00:/home/docker# docker exec -it tomcat01 ping -c 4 tomcat02
PING tomcat02 (192.168.6.3) 56(84) bytes of data.
64 bytes from tomcat02.mynet (192.168.6.3): icmp_seq=1 ttl=64 time=0.125 ms
64 bytes from tomcat02.mynet (192.168.6.3): icmp_seq=2 ttl=64 time=0.161 ms
64 bytes from tomcat02.mynet (192.168.6.3): icmp_seq=3 ttl=64 time=0.096 ms
64 bytes from tomcat02.mynet (192.168.6.3): icmp_seq=4 ttl=64 time=0.107 ms

--- tomcat02 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 40ms
rtt min/avg/max/mdev = 0.096/0.122/0.161/0.025 ms


root@docker00:/home/docker# docker exec -it tomcat02 ping -c 4 tomcat01
PING tomcat01 (192.168.6.2) 56(84) bytes of data.
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=1 ttl=64 time=0.039 ms
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=2 ttl=64 time=0.150 ms
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=3 ttl=64 time=0.145 ms
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=4 ttl=64 time=0.153 ms

--- tomcat01 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 36ms
rtt min/avg/max/mdev = 0.039/0.121/0.153/0.049 ms
```

![image-20210319012832542](https://cdn.jyoryo.com/oss/0f/7myzw7ddgxs.png)

![image-20210319013841189](https://cdn.jyoryo.com/oss/8f/7mzwlhdkjcw.png)

![image-20210319014020931](https://cdn.jyoryo.com/oss/55/7n01s4xx9mo.png)

我们自定义网络docker都已经帮我们维护好了对应关系，推荐使用我们自定义的网络。

优点：

可以使不同的应用使用不同的网络。

比如：

Redis/MySQL：不同的集群使用不同的网络，保障集群是安全、健康的。

### 网络连通

```shell
docker network connect
root@docker00:/home/docker# docker network connect --help

Usage:    docker network connect [OPTIONS] NETWORK CONTAINER

Connect a container to a network

Options:
      --alias strings           Add network-scoped alias for the container
      --driver-opt strings      driver options for the network
      --ip string               IPv4 address (e.g., 172.30.100.104)
      --ip6 string              IPv6 address (e.g., 2001:db8::33)
      --link list               Add link to another container
      --link-local-ip strings   Add a link-local address for the container

# 创建2个使用默认 bridge 网络的 tomcat 容器
root@docker00:/home/docker# docker run -d -P --name tomcat01bridge tomcat
65c996904744eee0e1f9f47ddd15dc76805c9d363c45ece4a9e245d8b5449db3
root@docker00:/home/docker# docker run -d -P --name tomcat02bridge tomcat
a7336a1a8dc3f4120d7b61051c3a082f911d31865f5e5447a363a4483cd2c66e

# 将容器 tomcat01bridge 连通到网络 mynet
root@docker00:/home/docker# docker network connect mynet tomcat01bridge
[
    {
        "Name": "mynet",
        "Id": "3f4ba4b2b80fd55b78f5fe366039ac1221651380435035b1764f0f60c72c834e",
        "Created": "2021-08-15T16:31:32.22308705+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "90f8630f9b7b9ae641a85e11ef26b317621f098ae83508abe5474d96ab9c9a68": {
                "Name": "tomcat01bridge",
                "EndpointID": "0faa94b8c141adb7b798373f289b4a5c31736855cfa0743379e6bab1a0657ab6",
                "MacAddress": "02:42:c0:a8:00:04",
                "IPv4Address": "192.168.0.4/16",
                "IPv6Address": ""
            },
            "c0320bd423bffcce26a078d07841554ea7cc44309384f0b3cb3c7b2b6d564fdf": {
                "Name": "tomcat-net-01",
                "EndpointID": "3154b28298637ecef0466aa41917dfcf54046abd6a7eb05624b0dbabf26c2aeb",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "d3d960e0044a02d439fd0f87f8664209af8f935ce8ef9f0fecd5d8273ff9ee6d": {
                "Name": "tomcat-net-02",
                "EndpointID": "653d462c40a6ee26e3f63a2c8f010bc03cda88a989a03095c3065d0c51ecd04e",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
#测试打通 tomcat01bridge - mynet
# 连通之後就是将tomcat01bridge放到了mynet下
# 1个容器对应2个IP类似于阿里云服务中公网IP和私网IP

# 在容器 tomcat01 中 ping tomcat01bridge
root@docker00:/home/docker# docker exec -it tomcat01 ping -c4 tomcat01bridge
PING tomcat01bridge (192.168.6.4) 56(84) bytes of data.
64 bytes from tomcat01bridge.mynet (192.168.6.4): icmp_seq=1 ttl=64 time=0.163 ms
64 bytes from tomcat01bridge.mynet (192.168.6.4): icmp_seq=2 ttl=64 time=0.108 ms
64 bytes from tomcat01bridge.mynet (192.168.6.4): icmp_seq=3 ttl=64 time=0.146 ms
64 bytes from tomcat01bridge.mynet (192.168.6.4): icmp_seq=4 ttl=64 time=0.107 ms

# 在容器 tomcat01bridge 中 ping tomcat01
root@docker00:/home/docker# docker exec -it tomcat01bridge ping -c 4 tomcat01
PING tomcat01 (192.168.6.2) 56(84) bytes of data.
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=1 ttl=64 time=0.109 ms
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=2 ttl=64 time=0.155 ms
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=3 ttl=64 time=0.109 ms
64 bytes from tomcat01.mynet (192.168.6.2): icmp_seq=4 ttl=64 time=0.129 ms

# 再次查看mynet的信息
root@docker00:/home/docker# docker network inspect mynet
```

![image-20210319015732109](https://cdn.jyoryo.com/oss/7e/7n1kyic3xfk.png)



小技巧：要跨网络操作别人的容器，就需要使用`docker network connect`来进行连通。

# Docker实战

## 实战创建`Redis`集群

分片+高可用+负载均衡



创建网卡：

```shell
docker network create redis --driver bridge --subnet 172.68.0.0/16
# 创建网卡
root@docker00:~# docker network create redis --driver bridge --subnet 172.68.0.0/16
d7a4cf0688143f703cf4f9426da8f93404fdc7697fbff576952dc5dc055fabcf

# 创建Redis配置（通过脚步创建6个）
for port in $(seq 1 6); \
do \
mkdir -p /data/redis/node-${port}/conf
touch /data/redis/node-${port}/conf/redis.conf
cat << EOF >/data/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.68.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done
```

![image-20210323202212288](https://cdn.jyoryo.com/oss/b0/841y9nd1fy8.png)

![image-20210323203043283](https://cdn.jyoryo.com/oss/4f/842q0akn75s.png)

```shell
# 创建Redis容器，一共有6个。
root@docker00:/data/redis# docker run -p 6371:6379 -p 16371:16379 --name redis-1 -v /data/redis/node-1/data:/data -v /data/redis/node-1/conf/redis.conf:/etc/redis/redis.conf -d --net redis --ip 172.68.0.11 redis:5.0.12-alpine3.13 redis-server /etc/redis/redis.conf

root@docker00:/data/redis# docker run -p 6372:6379 -p 16372:16379 --name redis-2 -v /data/redis/node-2/data:/data -v /data/redis/node-2/conf/redis.conf:/etc/redis/redis.conf -d --net redis --ip 172.68.0.12 redis:5.0.12-alpine3.13 redis-server /etc/redis/redis.conf

root@docker00:/data/redis# docker run -p 6373:6379 -p 16373:16379 --name redis-3 -v /data/redis/node-3/data:/data -v /data/redis/node-3/conf/redis.conf:/etc/redis/redis.conf -d --net redis --ip 172.68.0.13 redis:5.0.12-alpine3.13 redis-server /etc/redis/redis.conf

root@docker00:/data/redis# docker run -p 6374:6379 -p 16374:16379 --name redis-4 -v /data/redis/node-4/data:/data -v /data/redis/node-4/conf/redis.conf:/etc/redis/redis.conf -d --net redis --ip 172.68.0.14 redis:5.0.12-alpine3.13 redis-server /etc/redis/redis.conf

root@docker00:/data/redis# docker run -p 6375:6379 -p 16375:16379 --name redis-5 -v /data/redis/node-5/data:/data -v /data/redis/node-5/conf/redis.conf:/etc/redis/redis.conf -d --net redis --ip 172.68.0.15 redis:5.0.12-alpine3.13 redis-server /etc/redis/redis.conf

root@docker00:/data/redis# docker run -p 6376:6379 -p 16376:16379 --name redis-6 -v /data/redis/node-6/data:/data -v /data/redis/node-6/conf/redis.conf:/etc/redis/redis.conf -d --net redis --ip 172.68.0.16 redis:5.0.12-alpine3.13 redis-server /etc/redis/redis.conf
# 创建集群
/data # redis-cli --cluster create 172.68.0.11:6379 172.68.0.12:6379 172.68.0.13:6379 172.68.0.14:6379 172.68.0.15:6379 172.68.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.68.0.15:6379 to 172.68.0.11:6379
Adding replica 172.68.0.16:6379 to 172.68.0.12:6379
Adding replica 172.68.0.14:6379 to 172.68.0.13:6379
M: a56213ba8e6512f47e9b113242cbd26a04c9b1c4 172.68.0.11:6379
   slots:[0-5460] (5461 slots) master
M: 9970c171e1d4d3b14d01d64f008e98287edfd6e3 172.68.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: 8041851b334790ee99be68e525252a80a0ca6c1d 172.68.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: f9eca8ade53518d881abe06a7702960989b0670a 172.68.0.14:6379
   replicates 8041851b334790ee99be68e525252a80a0ca6c1d
S: 374d5e6755744ceee4c2d6c3c33fa7e01ff968f9 172.68.0.15:6379
   replicates a56213ba8e6512f47e9b113242cbd26a04c9b1c4
S: 11adc56af02e65684b5353096cd396de354f5eba 172.68.0.16:6379
   replicates 9970c171e1d4d3b14d01d64f008e98287edfd6e3
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 172.68.0.11:6379)
M: a56213ba8e6512f47e9b113242cbd26a04c9b1c4 172.68.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: f9eca8ade53518d881abe06a7702960989b0670a 172.68.0.14:6379
   slots: (0 slots) slave
   replicates 8041851b334790ee99be68e525252a80a0ca6c1d
S: 11adc56af02e65684b5353096cd396de354f5eba 172.68.0.16:6379
   slots: (0 slots) slave
   replicates 9970c171e1d4d3b14d01d64f008e98287edfd6e3
M: 9970c171e1d4d3b14d01d64f008e98287edfd6e3 172.68.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: 8041851b334790ee99be68e525252a80a0ca6c1d 172.68.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 374d5e6755744ceee4c2d6c3c33fa7e01ff968f9 172.68.0.15:6379
   slots: (0 slots) slave
   replicates a56213ba8e6512f47e9b113242cbd26a04c9b1c4
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```

连接集群：`redis-cli -c`，查看集群信息`cluster info`、集群节点`cluster nodes`。

![image-20210323205733978](https://cdn.jyoryo.com/oss/98/84544g7xukg.png)

```shell
# 集群测试
# 设置值
127.0.0.1:6379> set k1 v1
-> Redirected to slot [12706] located at 172.68.0.13:6379
OK

# 将172.68.0.13对应的容器停止
root@docker00:~# docker stop redis-3

# 获取值
/data # redis-cli -c
127.0.0.1:6379> get k1
-> Redirected to slot [12706] located at 172.68.0.14:6379
"v1"

# 查看集群节点
172.68.0.14:6379> cluster nodes
a56213ba8e6512f47e9b113242cbd26a04c9b1c4 172.68.0.11:6379@16379 master - 0 1616504607000 1 connected 0-5460
374d5e6755744ceee4c2d6c3c33fa7e01ff968f9 172.68.0.15:6379@16379 slave a56213ba8e6512f47e9b113242cbd26a04c9b1c4 0 1616504607000 5 connected
8041851b334790ee99be68e525252a80a0ca6c1d 172.68.0.13:6379@16379 master,fail - 1616504393932 1616504392000 3 connected
11adc56af02e65684b5353096cd396de354f5eba 172.68.0.16:6379@16379 slave 9970c171e1d4d3b14d01d64f008e98287edfd6e3 0 1616504608745 6 connected
9970c171e1d4d3b14d01d64f008e98287edfd6e3 172.68.0.12:6379@16379 master - 0 1616504607730 2 connected 5461-10922
f9eca8ade53518d881abe06a7702960989b0670a 172.68.0.14:6379@16379 myself,master - 0 1616504608000 7 connected 10923-16383
```

## SpringBoot打包Docker镜像

步骤：

1. 开发项目
2. 打包应用
3. 编写Dockerfile
4. 构建镜像
5. 运行

> 编写Dockerfile

```dockerfile
FROM java:8

COPY *.jar /app.jar

CMD ["--server.port=8080"]

EXPOSE 8080

ENTRYPOINT ["java","-jar","/app.jar"]
```

> 构建镜像

```shell
# 查看工作目录文件
root@docker00:/data/docker/springboot# ll
total 19884
-rw-r--r-- 1 root root      112 Mar 23 21:31 Dockerfile
-rw-r--r-- 1 root root 20357002 Mar 23 21:31 springboot-demo-1.0-SNAPSHOT.jar

# 打包镜像
root@docker00:/data/docker/springboot# docker build -t demospringboot:1.0 .
Sending build context to Docker daemon  20.36MB
Step 1/5 : FROM java:8
8: Pulling from library/java
5040bd298390: Pull complete
fce5728aad85: Pull complete
76610ec20bf5: Pull complete
60170fec2151: Pull complete
e98f73de8f0d: Pull complete
11f7af24ed9c: Pull complete
49e2d6393f32: Pull complete
bb9cdec9c7f3: Pull complete
Digest: sha256:c1ff613e8ba25833d2e1940da0940c3824f03f802c449f3d1815a66b7f8c0e9d
Status: Downloaded newer image for java:8
 ---> d23bdf5b1b1b
Step 2/5 : COPY *.jar /app.jar
 ---> bd52eb60c058
Step 3/5 : CMD ["--server.port=8080"]
 ---> Running in 6dd1c6b0d94a
Removing intermediate container 6dd1c6b0d94a
 ---> 9d61c697466a
Step 4/5 : EXPOSE 8080
 ---> Running in 6e7460e821a5
Removing intermediate container 6e7460e821a5
 ---> ddf374755516
Step 5/5 : ENTRYPOINT ["java","-jar","/app.jar"]
 ---> Running in 392e27aeefb4
Removing intermediate container 392e27aeefb4
 ---> e8ce8b7283a7
Successfully built e8ce8b7283a7
Successfully tagged demospringboot:1.0
```

![image-20210323213742375](https://cdn.jyoryo.com/oss/c3/848oo4hee4g.png)

> 运行测试

```shell
root@docker00:/data/docker/springboot# docker run -d -P --name demospringboot01 demospringboot:1.0
f88f4029e199c212483f79f40ed023eb079c1b1e9c7c089f351c57dd3fed1fec

root@docker00:/data/docker/springboot# docker ps
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                     NAMES
f88f4029e199        demospringboot:1.0   "java -jar /app.jar …"   25 seconds ago      Up 24 seconds       0.0.0.0:32768->8080/tcp   demospringboot01
root@docker00:/data/docker/springboot#

# 测试
root@docker00:/data/docker/springboot# curl http://localhost:32768
index >>> Welcome To SpringBoot!
```

![image-20210323214150049](https://cdn.jyoryo.com/oss/c7/8491vhv90xs.png)










