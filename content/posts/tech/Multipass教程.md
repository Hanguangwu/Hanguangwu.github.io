---
title: "Multipass教程"
date: 2021-08-02T19:34:25+08:00
lastmod: 2021-08-24T22:54:22+08:00
draft: false
categories:
- 技术
- 虚拟机
tags:
- Multipass
- Ubuntu
---



# Multipass教程





## 一. Multipass 是什么？

Multipass 是一个轻量级 Linux 虚拟机命令行管理工具，支持 Linux、Windows 与 macOS。它不仅使启用虚拟机变得快速简易，还使管理那些虚拟机变得异常简单，因此可以立即开始针对云、边缘、物联网或任何一种类型的技术进行开发。实际上，Multipass包含一个系统任务栏工具，你只要点击一下就可以启动和停止虚拟机，甚至进入虚拟机的外壳。

## 二. 为什么要用 Multipass

1. 能够以最小的成本和资源在本地快速搭建具备完整 Ubuntu 功能小型虚拟机集群（如测试 K8s各类特性、数据库小集群等）
2. 可以方便快速的做各类 Linux 试验，而不用担心把系统搞坏，重建一个新系统只要几分钟
3. 实例通过命令行管理，对开发非常友好，每个实例IP固定

## 三. 如何安装

### Linux 安装

使用 Snapcraft 安装

```bash
sudo snap install multipass
```



## 四. 功能介绍

可在 [Multipass 官网](https://multipass.run/) 查看详细使用文档。

```shell
-> ~ $ multipass help
用法: multipass [options] <command>
创建, 控制和连接 Ubuntu 实例。

multipass 命令行工具, 用于管理 ubuntu 实例。

参数:
  -h, --help     查看本帮助内容
  -v, --verbose  增加日志显示的详细程度。 通过在短参数中增加 'v' 来获取更多日志信息
                 最多支持4个等级，如： -vvvv。

可用的命令:
  delete    删除实例
  exec      在实例中执行命令
  find      查找并列出可用于创建实例的镜像
  get       获取某个配置项
  help      查看帮助
  info      查看实例信息
  launch    创建并启动实例
  list      列出所有实例
  mount     挂载文件夹到实例
  purge     清除已删除的实例
  recover   恢复已删除的实例
  restart   重启实例
  set       设置某个配置项
  shell     通过 shell 连接实例
  start     启动实例
  stop      停止实例
  suspend   挂起实例
  transfer  在本机和实例之间传输文件
  umount    移除实例中挂载的文件夹
  version   查看版本号
```

Multipass常用命令详解

1.查找镜像

```shell
multipass find
```

2.创建虚拟机

语法：multipass launch -n 虚拟机名称

-n, --name: 名称
-c, --cpus: cpu核心数, 默认: 1
-m, --mem: 内存大小, 默认: 1G
-d, --disk: 硬盘大小, 默认: 5G

```shell
multipass launch -n ubuntu-lts -c 4 -m 4G -d 40G
```

3.进入虚拟机

语法：multipass shell 虚拟机名称

```shell
multipass shell ubuntu-lts
```

4.直接使用虚拟机

语法：multipass exec 虚拟机名称 --命令

```shell
multipass exec ubuntu-lts -- ls
```

5.查看虚拟机列表

```shell
multipass ls
multipass list
```

6.查看虚拟机信息

语法：multipass info 虚拟机名称

```shell
multipass info ubuntu-lts
```

7.重启虚拟机

语法：multipass restart 虚拟机名称

```shell
multipass restart ubuntu-lts
```

8.删除虚拟机

语法：multipass delete 虚拟机名称

--purge 彻底删除

```shell
multipass delete ubuntu-lts
multipass delete --purge ubuntu-lts 彻底删除
```

9.恢复删除虚拟机

语法：multipass recover 虚拟机名称

```shell
multipass recover ubuntu-lts
```

10.启动虚拟机

语法：multipass start 虚拟机名称

```shell
multipass start ubuntu-lts
```

11.暂停虚拟机

语法：multipass stop 虚拟机名称

```shell
multipass stop ubuntu-lts
```

12.宿主机挂载虚拟机

语法：multipass mount 宿主机目录 虚拟机名称：虚拟机目录

```shell
multipass mount /mnt ubuntu-python
multipass mount /mnt ubuntu-python:/mnt
```

13.宿主机卸载虚拟机

语法：multipass unmount 虚拟机名称

```shell
multipass unmount ubuntu-lts
```

14.挂起虚拟机

语法：multipass suspend 虚拟机名称

```shell
multipass suspend ubuntu-lts
```

15.获取版本信息

```shell
multipass version
```

16.帮助

```shell
multipass help
```

先来创建一个容器：

```shell
$ multipass launch --name react
Launched: react
```

初次创建时需要下载镜像，网络畅通的情况下，稍等片刻即可。

容器创建后 multipass 会马上启动它，这样创建好容器后我们就可以直接使用了：

```shell
$ multipass exec react -- lsb_release -d
Description:    Ubuntu 18.04.4 LTS
```

`lsb_release` 会打印 Linux 发行版的信息。之前我们创建容器的时候并没有指定使用什么样的镜像，上面命令的输出表明，multipass 默认会使用当前 LTS 版本的 Ubuntu。

除了直接在容器上运行（`exec`）命令外，还可以通过 `shell` 命令「进入」容器：

```shell
multipass shell react
```

我们进入了一个完整的 Linux 环境，可以进行各种操作。例如，假设我们看到了[一篇介绍 React Hooks 的教程](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMzA5NjE3ODExNQ%3D%3D%26mid%3D2652982622%26idx%3D1%26sn%3Df89bc9cc778640ebb9e8ba979cc95c8a%26chksm%3D8b61f7f2bc167ee47c295449073c8afa5ba8d11f1b81cbbc61b699c3be2ccbf8925a9cf010a4%26token%3D536221894%26lang%3Dzh_CN%23rd)，打算体验一下教程的示例项目：

```shell
git clone https://github.com/hjiang/react-hook-demo.git
cd react-hook-demo
npm install 
```

哎呀，系统告诉我们 `npm` 没有安装，并建议通过 `apt` 安装。

```shell
The program 'npm' is currently not installed. You can install it by typing:
sudo apt install npm
```

不过，当前 LTS 版本的 Ubuntu 仓库里的 Node.js 比较老旧，我们转而安装 LTS 版本的 Node.js（12）：

```shell
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install
npm start
```

项目跑起来了，太棒了：

```shell
Compiled successfully!

You can now view leancloud-react-hook-tutorial in the browser.

  Local:            http://localhost:3000/
  On Your Network:  http://192.168.64.5:3000/

Note that the development build is not optimized.
To create a production build, use npm run build.
```

这里 `192.168.64.5` 是 Multipass 分配给 react 这个容器的 IP，所以我们可以直接在宿主机上打开浏览器访问 `http://192.168.64.5:3000/` 查看效果。

## **定制**

接下来我们尝试容器化手头正在开发的一个 Node.js 项目。和之前不同，我们将对容器进行一些定制，这样用起来更方便。

首先，我们的 Node.js 项目将部署到云平台，所以我们希望容器的规格尽可能和云平台上的生产环境一致。其次，我们之前手动安装了 Node.js，这次我们希望自动化这一安装过程。

因此，我们使用以下命令创建容器：

```shell
multipass launch --name lean --disk 2G --mem 256M --cloud-init lean.yaml 18.04
```

我们通过命令行参数指定了容器的磁盘和内存大小，并且显式指定使用 Ubuntu 18.04。容器创建成功后，通过 `multipass info` 可以查看容器的基本信息：

```shell
$ multipass info --all
Name:           lean
State:          Running
IPv4:           192.168.64.2
Release:        Ubuntu 18.04.4 LTS
Image hash:     fe3030939822 (Ubuntu 18.04 LTS)
Load:           0.11 0.30 0.16
Disk usage:     1.3G out of 2.0G
Memory usage:   71.4M out of 229.7M

Name:           react
State:          Running
IPv4:           192.168.64.5
Release:        Ubuntu 18.04.4 LTS
Image hash:     fe3030939822 (Ubuntu 18.04 LTS)
Load:           0.00 0.00 0.00
Disk usage:     1.7G out of 4.7G
Memory usage:   112.1M out of 985.7M
```

可以看到，之前创建的 react 容器，multipass 默认分配了 5G 硬盘和 1G 内存。而 lean 容器则按照我们的要求分配了 2G 硬盘和 256M 内存（这是我们计划使用的云平台 [LeanCloud 云引擎](https://link.zhihu.com/?target=https%3A//www.leancloud.cn/engine/) 免费版体验实例的规格）。另外，基本信息中没有 CPU 核心的信息，multipass 默认会给容器分配 1 个 CPU 核心。

至于 `lean.yaml` 则是容器的初始化配置文件，内容如下：

```text
#cloud-config

runcmd:
  - curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
  - sudo apt-get install -y nodejs
  - wget https://releases.leanapp.cn/leancloud/lean-cli/releases/download/v0.21.0/lean-cli-x64.deb
  - sudo dpkg -i lean-cli-x64.deb
```

`runcmd` 可以指定容器 **首次启动** 时运行的命令，这里我们复制了之前安装 Node.js 的命令，还加上了安装 `lean-cli` 的命令（我们通过 `lean-cli` 将代码部署到云平台）。

容器初始化配置文件遵循 [cloud-init](https://link.zhihu.com/?target=https%3A//cloudinit.readthedocs.io/en/latest/topics/examples.html) 标准，可以通过 yaml 文件进行用户、文件、软件仓库、 DNS 解析、SSH 密钥、puppet、chef 等各种初始化配置。

我们只打算在容器中测试、部署项目，并不打算 `multipass shell` 到容器内使用 vim 或 emacs 开发项目。所以，我们直接挂载宿主机上的一个目录：

```text
multipass mount demo lean:/home/ubuntu/demo
```

> `demo` 是我们的 Node.js 项目目录，如果读者想要测试，可以用下面这个模板项目：
> git clone [https://github.com/leancloud/node-js-getting-started](https://link.zhihu.com/?target=https%3A//github.com/leancloud/node-js-getting-started) demo
>
> 同时去 [LeanCloud](https://link.zhihu.com/?target=https%3A//leancloud.app/) 注册账号、创建应用，方便体验下面的部署操作。

挂载完成后，我们就可以在宿主机上使用趁手的 IDE、编辑器开发项目，之后 `multipass shell lean` 到容器内测试：

```text
cd demo
lean login # 使用之前注册的 LeanCloud 账号登录
lean switch # 选择之前创建的应用
npm install # 安装项目依赖
lean up # 本地（容器内）调试
```

屏幕会输出：

```text
Node app is running on port: 3000
```

之前通过 `multipass info`，我们知道 lean 容器的 IP 是`http://192.168.64.2`，所以在宿主机上访问 `http://192.168.64.2:3000/` 即可查看效果。
如果效果符合预期，我们可以在容器内运行 `lean deploy --prod 1` 部署项目。

运行 `multipass list` 可以列出所有的容器：

```text
Name                    State             IPv4             Image
lean                    Running           192.168.64.2     Ubuntu 18.04 LTS
react                   Running           192.168.64.5     Ubuntu 18.04 LTS
```

如果希望节约资源，我们可以停止暂时用不到的容器，比如之前创建的 react：

```text
multipass stop react
```

之后我们可以运行 `multipass start react` 重新运行容器。如果以后不再使用，那么也可以干脆删除：

```text
multipass delete --purge react
```

最后，很多时候，我们只是想要在 macOS 或 Windows 上起一个 Linux 环境，然后进行一些操作，multipass 应付这一使用场景最是得心应手：

```text
multipass shell
```

是的，你没看错，只需一条命令，你就可以进入一个与宿主机隔离的 Linux 容器！
multipass 会自动创建并运行一个名为 Primary 的容器（如果还没有创建或运行的话），这个容器也会自动挂载宿主机的 Home 目录，就是这么省心省力。





## 五. 常见问题（以 MacOS 为例）

### 问题一：最开始设置的内存或 CPU 数量小了，想扩容，怎么办？

multipass 通过 `/var/root/Library/Application Support/multipassd/multipassd-vm-instances.json` 中的配置来管理实例，可直接在这个配置文件中修改：
`mem_size` 来增加或减少内存
`num_cores` 来增加或减少CPU核心数

修改之前需要先停止 multipass 的进程，原因是 multipass 会在被关闭的时候将各个实例的状态写入到配置文件，所以在没有关闭 multipass 进程的时候修改配置文件，会被覆盖。

```bash
# 停止 multipassd 进程
sudo launchctl unload /Library/LaunchDaemons/com.canonical.multipassd.plist

# 编辑 /var/root/Library/Application Support/multipassd/multipassd-vm-instances.json 文件
# 需要 root 权限

# 重新启动 multipassd 进程
sudo launchctl load /Library/LaunchDaemons/com.canonical.multipassd.plist
```

### 问题二：电脑意外关机，无法启动实例，怎么办？

实例的启动关闭状态也维护在 `/var/root/Library/Application Support/multipassd/multipassd-vm-instances.json` 文件中的 `state` 字段，当电脑意外关机，`state` 字段不会被正确的维护，导致无法启动或关闭实例，这时候，可以先停止 multipassd 进程，然后手动到配置文件中修改 `state` 为 0，即关机状态，保存配置文件，并启动 multipassd 实例即可，这时候就可以正常启动各个实例了。

### 问题三：能安装 Cent OS 实例么？

暂时不能，该工具为 Ubuntu 背后的公司 Canonical 开发，目前仅支持 Ubuntu 系统。

### 问题四：如果在实例之间传递文件？

最简单的方式是通过挂载相同的文件夹到不同的实例中来共享文件。

如何在主机和虚拟机之间共享目录

要在主机和 VM 之间共享目录，必须挂载它。假设您想将目录 ~/code 共享到新创建的云主机 VM。要将其挂载到 VM，请发出以下命令：

`multipass mount ~/code cloud-host`

几秒钟后，您会得到提示，没有错误。要确保挂载成功，请发出以下命令：

`multipass info cloud-host`

您应该会看到列出的新安装。

如何卸载目录

完成工作后，您可能希望卸载该共享目录。为此，请发出以下命令：

`multipass unmount cloud-host`

虚拟机将无法再访问共享目录。VM 目录将保留，但列出的文件不再存在。


And that's all there is to sharing data between a host and a Multipass VM. As you can see, this task couldn't possibly be any simpler. At this point, you might be thinking Multipass might be the perfect VM tool to meet your cloud development needs. 



如何卸载目录

完成工作后，您可能希望卸载该共享目录。为此，请发出以下命令：

`multipass unmount cloud-host`

虚拟机将无法再访问共享目录。VM 目录将保留，但列出的文件不再存在。





