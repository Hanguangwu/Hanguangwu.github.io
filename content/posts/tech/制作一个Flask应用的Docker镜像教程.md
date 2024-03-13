---
title: "制作一个Flask应用的Docker镜像教程"
date: 2023-05-17T23:34:25+08:00
draft: false
categories:
- 容器
- Getting Started
tags:
- Docker
- Flask
---




 [使用Docker部署应用以及容器数据卷Volume](https://www.cnblogs.com/kkbill/p/12950707.html)

Docker是一个广泛使用的容器化平台，可用于部署和运行各种应用程序。使用Docker，我们可以轻松地将应用程序打包成容器并在任何环境中运行，从而实现更高效的开发和部署。

构建镜像的方式有两种：一种是基于容器制作，另一种就是通过Dockerfile。

Dockerfile是一个Docker镜像的描述文本文件,内部是一条条**顺序**执行的指令。

## 步骤1：创建Python Flask应用程序

我们将使用Python Flask框架创建一个简单的Web应用程序。首先，创建一个名为“app.py”的Python文件，并使用以下代码编写应用程序：

```python
from flask import Flask 

app = Flask(__name__) 
@app.route("/") 

def hello():
	return "Hello, World!" 
	
if __name__ == "__main__":
	#app.run(host="0.0.0.0", port=5000)
	app.run()
```

requirements.txt文件中加入：

```bash
flask
```

启动这个应用后，默认通过http://127.0.0.1:5000/访问，也可以按需更改

这个应用程序只返回“Hello, World!”，但它足以演示如何将一个Python Flask应用程序打包成Docker容器。

## 步骤2：编写Dockerfile文件

接下来，我们将编写一个Dockerfile文件，用于将Python Flask应用程序打包成Docker容器。

在项目根目录下创建一个名为“Dockerfile”的文件（Dockerfile文件和app.py同目录），并添加如下内容：

```dockerfile
#指定要使用的基础镜像
FROM python:3.10-slim-buster

# 切换到/app目录并将应用文件全部复制到容器中
WORKDIR /app
COPY . /app

#安装应用所需的依赖项
RUN pip install --no-cache-dir -r requirements.txt
#暴露应用进程的端口号
EXPOSE 5000
#设置启动命令
CMD ["python","app.py"]
```
也可以配置所需环境变量，如下所示：

```dockerfile
# 使用官方 Python 基础镜像 
FROM python:3.8-slim-buster 
# 将工作目录设置为 /app 
WORKDIR /app 
# 将当前目录中的内容复制到 /app 中 
COPY . /app 
# 安装应用程序所需的依赖项 
RUN pip install --no-cache-dir -r requirements.txt 
# 暴露容器的端口 
EXPOSE 5000 
# 定义环境变量 
ENV FLASK_APP=app.py 
# 在容器启动时运行应用程序 
CMD ["flask", "run", "--host=0.0.0.0"]
```

**Dockerfile 的设计思想，是使用一些标准的原语（即`FROM`/`WORKDIR`/...），描述我们所要构建的 Docker 镜像，并且这些原语，都是按顺序处理的**。可以通过下面这个文件了解参数具体意义。

```dockerfile
# 使用官方提供的Python开发镜像作为基础镜像
FROM python:3.6-slim

# 将工作目录切换为/app
WORKDIR /app

# 将当前目录下的所有内容复制到/app下
ADD . /app

# 使用pip命令安装这个应用所需要的依赖
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# 允许外界访问容器的80端口
EXPOSE 80

# 设置环境变量
ENV NAME World

# 设置容器进程为：python app.py，即：这个Python应用的启动命令
CMD ["python", "app.py"]

```

-   FROM：FROM 指令为后续的操作设置基础镜像（[Base Image](https://docs.docker.com/glossary/)），**一个有效的 Dockerfile 文件必须以 FROM 指令开始**。指定了“python:3.6-slim”这个官方维护的基础镜像，在这个基础镜像中，已经安装好了python的语言环境等；
-   WORKDIR：WORKDIR 指令为其后续的RUN/CMD等指令设置工作目录。在这里，将工作目录切换至`/app`，也就是说，在这一句指令执行之后，Dockerfile 之后的操作都以该命令指定的目录（即`/app`）作为当前目录；
-   ADD src ... dest：ADD 指令将`src`目录下的文件或目录拷贝至镜像文件系统的`dest`路径下。在这里，就是将当前目录下的3个文件拷贝至`/app`目录下；
-   RUN：RUN 指令就是在容器里执行相应的shell命令。在这里，使用pip命令安装这个应用所需要的依赖；
-   EXPOSE：对外暴露容器在运行时的监听端口，此外还可以指定是端口监听基于TCP还是UDP的，默认为TCP。在这里，表示允许外界访问容器的80端口。你也可以写成`EXPOSE 80/tcp` ；
-   ENV：设置环境变量；
-   CMD：CMD指令的主要作用就是为容器设置默认行为。在这里表示的意思是 Dockerfile 指定 python app.py 为这个容器的进程。其中app.py 的实际路径是`/app/app.py`。所以，`CMD ["python", "app.py"]`等价于`docker run <image> python app.py` 。注意，一个Dockerfile文件中只能有一个CMD指令，如果出现多个，只有最后一个会起作用。

关于Dockerfile文件各个指令详细说明，参考[Docker reference](https://docs.docker.com/engine/reference/builder/)

## 步骤3：构建Docker镜像

接下来，我们将使用Dockerfile来构建Docker镜像。在项目根目录下打开终端，然后在服务器该项目目录下执行以下命令：

```bash
docker build -t docker-test:v1 .
```

该命令将使用当前目录中的Dockerfile文件，并为该文件创建一个名为“docker-test”的Docker镜像。请确保在命令的结尾包含一个点“.”，这表示使用当前目录作为构建上下文。其中-t 指定生成的镜像的名字及标签，可以只写名字。

当最后出现：  

**Successfully built 9034bvf978e5**  
**Successfully tagged docker-test:v1**  

此时flask镜像就被成功创建。在镜像构建完成后，可以运行`docker images`来查看所有可用的Docker镜像。

## 步骤4：运行Docker容器

现在，我们已经成功地将Python Flask应用程序打包成了Docker容器。接下来，我们将运行该容器并测试Web应用程序是否能够正常工作。

在终端中执行以下命令，运行Docker容器：

```shell
docker run -p 5000:5000 docker-test:v1
```

该命令将在本地计算机上运行一个名为“docker-test”的Docker容器，并将容器的端口映射到本地计算机的端口5000。

现在，在Web浏览器中输入http://localhost:5000，我们应该能够看到“Hello, World!”的消息。这表示我们已经成功地使用Docker发布了我们的第一个Web项目！

先运行`docker ps`命令查看容器id再运行`docker logs id`能查看对应容器的日志。

## 步骤5：上传镜像到DockerHub

如果现在想要把这个容器的镜像上传到 DockerHub 上分享给更多的人，我要怎么做呢？为了能够上传镜像，首先需要注册一个 Docker Hub 账号（如果还没有Docker Hub账号，先去[官网](https://hub.docker.com/)申请），然后使用 docker login命令登录。

登录Docker Hub

```vhdl
root@ubuntu:~# docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: kkbill
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

然后使用 docker tag 命令给容器镜像打标签

```ruby
root@ubuntu:~# docker tag helloworld kkbill/helloworld:v1.0
```

在本地可以看到镜像已经发生了一些变化

```ruby
root@ubuntu:~# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
kkbill/helloworld   v1.0                f4cb037a3aeb        51 minutes ago      184MB
```

接下来，把这个镜像推到 Docker Hub上去。

```ruby
root@ubuntu:~# docker push kkbill/helloworld:v1.0
```

随后在个人主页上可以看到：

![](https://img2020.cnblogs.com/blog/1442950/202005/1442950-20200524131349697-465370630.png)

至此，已经成功把自己打包的镜像上传到Docker Hub上了，如果别人需要，可以通过`docker pull kkbill/helloworld:v1.0` pull下来使用。

此外，还可以使用 docker commit 命令，把一个正在运行的容器，直接提交为一个镜像。在这里，我先进入容器中做了简单的修改（即添加了一个文件），随后再执行 docker commit 命令。

```ruby
root@ubuntu:~# docker exec -ti 7adfd4d9ac2d /bin/sh
# ls
Dockerfile  app.py  requirements.txt
# touch test.txt
# ls
Dockerfile  app.py  requirements.txt  test.txt
# exit
root@ubuntu:~# docker commit 7adfd4d9ac2d kkbill/helloworld:v1.1
sha256:9e677a13bbf067e448ec112aa06dc0a2a397fa921253d838d73a2a873fcc7b5a
root@ubuntu:~# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
kkbill/helloworld   v1.1                9e677a13bbf0        17 seconds ago      184MB
kkbill/helloworld   v1.0                f4cb037a3aeb        About an hour ago   184MB
```

如有必要，也可以再次把这个新的镜像push到Docker Hub上去。

关于 [docker commit](https://docs.docker.com/engine/reference/commandline/commit/) 这个命令，官方的描述是：Create a new image from a container’s **changes**。实际上**就是在容器运行起来后，把最上层的“可读写层”，加上原先容器镜像的只读层，打包组成了一个新的镜像。不过下面这些只读层在宿主机上是共享的，不会占用额外的空间**。

而由于使用了**联合文件系统**，在容器里对镜像 rootfs 所做的任何修改，都会被操作系统先复制到这个可读写层，然后再修改。这就是所谓的Copy-on-Write。而Init 层的存在，就是为了避免执行 docker commit 时，把 Docker 自己对 /etc/hosts 等文件做的修改，也一起提交掉。

另外，在企业内部，能不能也搭建一个跟 Docker Hub 类似的镜像上传系统呢？当然可以，这个统一存放镜像的系统，就叫作 Docker Registry。感兴趣的话，可以查看[Docker 的官方文档](https://docs.docker.com/registry/)，以及 [Harbor 项目](https://github.com/goharbor/harbor)。

## 数据卷（Volume）

容器技术使用了 rootfs 机制和 Mount Namespace，构建出了一个同宿主机完全隔离开的文件系统环境。这时候，我们就需要考虑这样两个问题：

-   容器里进程新建的文件，怎么才能让宿主机获取到？
-   宿主机上的文件和目录，怎么才能让容器里的进程访问到？

这正是 Docker Volume 要解决的问题：Volume 机制，**允许你将宿主机上指定的目录或者文件，挂载到容器里面进行读取和修改操作**。

Docker 支持两种 volume 的声明方式，可以把宿主机中的目录挂载到容器内的指定目录中。

-   **docker run -v /test ...**：这种方式并没有显示声明宿主机目录，那么 Docker 就会默认在宿主机上创建一个临时目录 /var/lib/docker/volumes/[VOLUME_ID]/_data，然后把它挂载到容器的 /test 目录上。
-   **docker run -v /home:/test ...**：把宿主机的 /home 目录挂载到容器的 /test 目录上。

首先，启动一个 helloworld 容器，记得加上 -v 参数，表明要挂载一个数据卷。

```ruby
root@ubuntu:~# docker run -d -v /test kkbill/helloworld:v1.0
4e7b0192d0cde5260298db796c654df238b7a58ac75937a883b2d1cea37a0ad8
```

通过 docker volume 查看一下对应的 volume 信息：

```ruby
root@ubuntu:~# docker volume ls
DRIVER              VOLUME NAME
local               ad20a6c12c554fa429caca5261a6e459ffbc94a41a2db6bfd1a2f6f8fa3a81c7
root@ubuntu:~# docker volume inspect ad20a6c12c554fa42...
[
    {
        "CreatedAt": "2020-05-16T16:38:37+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/ad20a6c12c554fa42.../_data",
        "Name": "ad20a6c12c554fa42...",
        "Options": null,
        "Scope": "local"
    }
]
```

可以看到，挂载点在`/var/lib/docker/volumes/ad20a6c12c554fa42.../_data` ，和之前分析的一致。这个 _data 文件夹，就是这个容器的 Volume 在宿主机上对应的临时目录了。

或者，我们也可以通过 docker inspect container_id 来查看该容器的volume挂载信息，如下：

```ruby
root@ubuntu:~# docker inspect 4e7b0192d0cd
...
        "Mounts": [
            {
                "Type": "volume",
                "Name": "ad20a6c12c554fa42...",
                // 宿主机上的目录
                "Source": "/var/lib/docker/volumes/ad20a6c12c554fa42.../_data",
                // 容器内的目录
                "Destination": "/test",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

接下来，进入容器内部并添加一个文件。

```ruby
root@ubuntu:~# docker exec -ti 4e7b0192d0cd /bin/bash
root@4e7b0192d0cd:/app# ls /   //可以看到，在根目录下已经创建好了/test文件夹
app  boot  etc	 lib	media  opt   root  sbin  sys   tmp  var
bin  dev   home  lib64	mnt    proc  run   srv	 test  usr  
root@4e7b0192d0cd:/app# cd /test
root@4e7b0192d0cd:/test# ls
root@4e7b0192d0cd:/test# echo "hello,world" > test.txt
root@4e7b0192d0cd:/test# ls
test.txt
```

然后回到宿主机，去 _data 文件价下看看发生了什么变化。

```ruby
root@ubuntu:~# ls /var/lib/docker/volumes/ad20a6c12c554fa42.../_data/
test.txt
root@ubuntu:~# cat /var/lib/docker/volumes/ad20a6c12c554fa42../_data/test.txt 
hello,world
```

可以看到，我们在宿主机上能够看到容器添加的文件。

如果在宿主机上对挂载目录下的文件进行修改，或是在挂载目录下新增/删除文件，在容器内部应该也能马上看到：

```shell
root@ubuntu:~# vim /var/lib/docker/volumes/ad20a6c12c554fa42.../_data/test.txt 
root@ubuntu:~# cat /var/lib/docker/volumes/ad20a6c12c554fa42.../_data/test.txt
hello,world
add something here  // 新增一句话
// 进入挂载目录，并新增一个文件
root@ubuntu:/var/lib/docker/volumes/ad20a6c12c554fa42.../_data# touch test2.txt
```

再次进入容器内，可以看到，在宿主机上对文件的修改在容器内也可以看到：

```shell
root@ubuntu:~# docker exec -ti 4e7b0192d0cd /bin/bash
root@4e7b0192d0cd:/app# cat /test/test.txt 
hello,world
add something here
root@4e7b0192d0cd:/app# ls /test/
test.txt  test2.txt
```

以上就是容器数据卷（Volume）的操作演示。

那么，Volume 背后的原理是什么呢？这一操作究竟是怎样实验的呢？

之前已经介绍过，当容器进程被创建之后，尽管开启了 Mount Namespace，但是**在它执行 pivot_root（或者chroot ）之前，容器进程一直可以看到宿主机上的整个文件系统**。

而宿主机上的文件系统，也自然包括了我们要使用的容器镜像。这个镜像的各个层，保存在 /var/lib/docker/aufs/diff 目录下，在容器进程启动后，它们会被联合挂载在 /var/lib/docker/aufs/mnt/ 目录中，这样容器所需的 rootfs 就准备好了。

所以，我们只需要**在 rootfs 准备好之后，在执行 pivot_root之前**，把 Volume 指定的宿主机目录（比如 /home 目录），挂载到指定的容器目录（比如 /test 目录）在宿主机上对应的目录（即 /var/lib/docker/aufs/mnt/[可读写层 ID]/test）上（有点绕，仔细体会），这个 Volume 的挂载工作就完成了。

> 注意：这里提到的"容器进程"，是 Docker 创建的一个容器初始化进程 (dockerinit)，而不是应用进程 (ENTRYPOINT + CMD)。dockerinit 会负责完成根目录的准备、挂载设备和目录、配置 hostname 等一系列需要在容器内进行的初始化操作。最后，它通过 execv() 系统调用，让应用进程取代自己，成为容器里的 PID=1 的进程。（这部分很重要，目前还没有完全搞懂...2020/05/16）

而这里要使用到的挂载技术，就是 Linux 的**绑定挂载（bind mount）机制**。它的主要作用就是，允许你将一个目录或者文件，而不是整个设备，挂载到一个指定的目录上。并且，这时你在该挂载点上进行的任何操作，只是发生在被挂载的目录或者文件上，而原挂载点的内容则会被隐藏起来且不受影响。

从Linux 内核的角度来看，**绑定挂载实际上是一个 inode 替换的过程**。在 Linux 操作系统中，**inode 可以理解为存放文件内容的“对象”，而 dentry，也叫目录项，就是访问这个 inode 所使用的“指针”**。

![img](https://static001.geekbang.org/resource/image/95/c6/95c957b3c2813bb70eb784b8d1daedc6.png)

如上图所示，执行`mount --bind /home /test`操作，会将 /home 挂载到 /test 上。其实**相当于将 /test 的 dentry，重定向到了 /home 的 inode**。这样**当我们修改 /test 目录时，实际修改的是 /home 目录的 inode。这也就是为何，一旦执行 umount 命令，/test 目录原先的内容就会恢复：因为修改真正发生在的，是 /home 目录里**。这样，进程在容器里对这个 /test 目录进行的所有操作，都实际发生在宿主机的对应目录（比如，/home，或者 /var/lib/docker/volumes/[VOLUME_ID]/_data）里，而不会影响容器镜像的内容。












