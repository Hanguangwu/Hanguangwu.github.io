---
title: "Docker+WSL2安装教程"
date: 2024-02-18T16:34:25+08:00
lastmod: 2024-02-18T22:54:22+08:00
draft: false
categories:
- Linux
- Docker
tags:
- wsl2
- docker
---

# 前言

日常开发常用到Ubuntu等Linux操作系统，通常的选择如下：
- 双系统
- 用VMware、VirtualBox等安装相应的虚拟机
- 使用微软开发的适用于 Linux 的 Windows 子系统 (WSL)

前两种方式以前都进行过好几次，轻车熟路，最近开始使用wsl2开发，下面wsl2入门教程。

[官方教程](https://learn.microsoft.com/zh-cn/windows/wsl/install)

# 相关资源

[在Windows10中安装WSL2(Ubuntu 22.04.2 LTS)](https://blog.csdn.net/qq_63432403/article/details/130297605)

[旧版 WSL 的手动安装步骤](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)

优先使用旧版 WSL 的手动安装步骤

[如何使用 WSL 在 Windows 上安装 Linux](https://learn.microsoft.com/zh-cn/windows/wsl/install)

[【WSL2教程】WSL迁移到非系统盘](https://blog.csdn.net/m0_37605642/article/details/127812965)

[WSL2安装Debian(Ubuntu)并配置国内apt源](https://zhuanlan.zhihu.com/p/99938831)

[在 Windows 中配置 WSL2 与 Debian 的全流程 ](https://www.cnblogs.com/wanderoff-night/p/17988720)

[# Win11 安装 Docker Desktop 和 WSL2 并进行安装位置迁移](https://blog.csdn.net/cn_ljr/article/details/132047516)

# 安装过程概述

安装WSL2，并下载linux系统

若要更新到 WSL 2，需要运行 Windows 10。

对于 x64 系统：版本 1903 或更高版本，内部版本为 18362 或更高版本。

对于 ARM64 系统：版本 2004 或更高版本，内部版本为 19041 或更高版本。

安装WSL2的详细步骤官网上都有：旧版 WSL 的手动安装步骤 | Microsoft Learn

之后是安装linux系统，这里以Ubuntu 22.04 LTS为例。

打开微软商店，搜索linux，找到Ubuntu 22.04 LTS进行安装。地址：

https://www.microsoft.com/store/apps/9PN20MSR04DW

下载好后，等几分钟进行初始化。

之后进行用户名和密码的配置。

安装好linux后，可以将包管理进行换源，让下载速度更快。

以管理员权限打开cmd终端修改WSL2安装目录。

WSL镜像文件默认安装的时候会安装在C盘，会占用C盘很大的空间。导致C盘吃紧，因此需要迁移到非系统盘。


# [旧版 WSL 的手动安装步骤](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)

## 步骤 1 - 启用适用于 Linux 的 Windows 子系统

需要先启用“适用于 Linux 的 Windows 子系统”可选功能，然后才能在 Windows 上安装 Linux 分发。

以管理员身份打开 PowerShell（“开始”菜单 >“PowerShell” >单击右键 >“以管理员身份运行”），然后输入以下命令：

PowerShell复制

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

建议现在转到步骤2，更新到 WSL 2，但如果只想安装 WSL 1，现在可以重新启动计算机，然后继续执行[步骤 6 - 安装所选的 Linux 发行版](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual#step-6---install-your-linux-distribution-of-choice)。 若要更新到 WSL 2，请等待重新启动计算机，然后继续执行下一步。

## 步骤 2 - 检查运行 WSL 2 的要求

若要更新到 WSL 2，需要运行 Windows 10。

- 对于 x64 系统：版本 1903 或更高版本，内部版本为 18362.1049 或更高版本。
- 对于 ARM64 系统：版本 2004 或更高版本，内部版本为 19041 或更高版本。

或 Windows 11。

## 步骤 3 - 启用虚拟机功能

安装 WSL 2 之前，必须启用“虚拟机平台”可选功能。 计算机需要[虚拟化功能](https://learn.microsoft.com/zh-cn/windows/wsl/troubleshooting#error-0x80370102-the-virtual-machine-could-not-be-started-because-a-required-feature-is-not-installed)才能使用此功能。

以管理员身份打开 PowerShell 并运行：

PowerShell复制

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

**重新启动**计算机，以完成 WSL 安装并更新到 WSL 2。

## 步骤 4 - 下载 Linux 内核更新包

Linux 内核更新包会安装最新版本的 [WSL 2 Linux 内核](https://github.com/microsoft/WSL2-Linux-Kernel)，以便在 Windows 操作系统映像中运行 WSL。 （若要运行 [Microsoft Store 中的 WSL](https://learn.microsoft.com/zh-cn/windows/wsl/compare-versions#wsl-in-the-microsoft-store) 并更频繁地推送更新，请使用 `wsl.exe --install` 或 `wsl.exe --update`。）

1. 下载最新包：
    
    - [适用于 x64 计算机的 WSL2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
    
     备注
    
    如果使用的是 ARM64 计算机，请下载 [ARM64 包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi)。 如果不确定自己计算机的类型，请打开命令提示符或 PowerShell，并输入：`systeminfo | find "System Type"`。 **Caveat：** 在非英文版 Windows 上，你可能必须修改搜索文本，对“System Type”字符串进行翻译。 你可能还需要对引号进行转义来用于 find 命令。 例如，在德语版中使用 `systeminfo | find '"Systemtyp"'`。
    
2. 运行上一步中下载的更新包。 （双击以运行 - 系统将提示你提供提升的权限，选择“是”以批准此安装。）
    

安装完成后，请继续执行下一步 - 在安装新的 Linux 分发时，将 WSL 2 设置为默认版本。 （如果希望将新的 Linux 安装设置为 WSL 1，请跳过此步骤。）

## 步骤 5 - 将 WSL 2 设置为默认版本

打开 PowerShell，然后在安装新的 Linux 发行版时运行以下命令，将 WSL 2 设置为默认版本：

PowerShell复制

```
wsl --set-default-version 2
```

## 步骤 6 - 安装所选的 Linux 分发

 打开 [Microsoft Store](https://aka.ms/wslstore)，并选择你偏好的 Linux 分发版。

在分发版的页面中，选择“获取”。

首次启动新安装的 Linux 分发版时，将打开一个控制台窗口，系统会要求你等待一分钟或两分钟，以便文件解压缩并存储到电脑上。 未来的所有启动时间应不到一秒。

然后，需要[为新的 Linux 分发版创建用户帐户和密码](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment#set-up-your-linux-username-and-password)。

**祝贺你！ 现已成功安装并设置了与 Windows 操作系统完全集成的 Linux 分发！**
# 修改WSL2安装目录

[【WSL2教程】WSL迁移到非系统盘](https://blog.csdn.net/m0_37605642/article/details/127812965)

## 一、问题引入

默认情况下，WSL会安装在C盘（系统盘），而C盘空间有限，随着WSL子系统的使用，C盘空间越来越少，最后有可能出现C盘不足导致WSL系统崩溃。

有效的解决方法是，将WSL子系统迁移到其他盘（非系统盘）。

## 二、WSL磁盘迁移（方式一）

wsl的安装及迁移至其他盘

1.整体流程

![a](https://img-blog.csdnimg.cn/74671959ffe747b39ea5706be8ab6ccb.png#pic_center)


2.查看wsl状态

查看wsl下的Linux是否为关闭状态，当wsl为Stopped才能进行下一步。

`wsl -l -v`

3.导出系统镜像

以压缩包的形式导出到其他盘。

`wsl --export Ubuntu D:\UbuntuWSL\ubuntu.tar`

4.注销原有的linux系统

`wsl --unregister Ubuntu`

5.查看系统状态

查看是否真的注销成功。

`wsl -l -v`

6.导入系统

`wsl --import Ubuntu D:\UbuntuWSL\ D:\UbuntuWSL\ubuntu.tar --version 2`

参数详解：wsl --import <导入的Linux名称> <导入盘的路径> <ubuntu.tar的路径> --version 2 (代表wsl2)

7.修改默认用户

打开wsl ubuntu之后，默认以root身份登录。

`ubuntu.exe config --default-user vincent`

在导入任意盘linux系统时，我起名Ubuntu，所以这里是ubuntu.exe；如果你起的名字是Ubuntu-20.04，那这里就是ubuntu2004.exe；如果你起的名字是ubuntu-18.04，那这里就是ubuntu1804.exe。

vincent是原有wsl ubuntu的用户名称。

## Powershell指令

```
PS C:\Windows\system32> wsl -l -v
适用于 Linux 的 Windows 子系统没有已安装的分发版。
可以通过访问 Microsoft Store 来安装分发版:
https://aka.ms/wslstore
PS C:\Windows\system32> wsl --export Ubuntu D:\wsl\ubuntu.tar
不存在具有提供的名称的分布。
PS C:\Windows\system32> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
  Debian    Running         2
PS C:\Windows\system32> wsl --shutdown
PS C:\Windows\system32> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
  Debian    Stopped         2
PS C:\Windows\system32> wsl --export Ubuntu D:\wsl\ubuntu.tar
PS C:\Windows\system32> wsl --unregister Ubuntu
正在注销...
PS C:\Windows\system32> wsl -l -v
  NAME      STATE           VERSION
* Debian    Stopped         2
PS C:\Windows\system32> wsl --import Ubuntu D:\wsl\ D:\wsl\ubuntu.tar --version 2
PS C:\Windows\system32> ubuntu.exe config --default-user xiaobaozi
PS C:\Windows\system32> wsl -l -v
  NAME      STATE           VERSION
* Debian    Stopped         2
  Ubuntu    Stopped         2
PS C:\Windows\system32> wsl --export Debian D:\wsl\debian.tar
PS C:\Windows\system32> wsl --unregister Debian
正在注销...
PS C:\Windows\system32> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
PS C:\Windows\system32> wsl --import Debian D:\wsl\ D:\wsl\debian.tar --version 2
提供的安装位置已在使用中。
PS C:\Windows\system32> wsl --import Debian D:\wsl\ D:\debian.tar --version 2
提供的安装位置已在使用中。
PS C:\Windows\system32> wsl --import Debian D:\wsl\ D:\debian.tar --version 2
提供的安装位置已在使用中。
PS C:\Windows\system32> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
  Debian    Stopped         2
PS C:\Windows\system32> wsl --unregister Debian
正在注销...
PS C:\Windows\system32> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
PS C:\Windows\system32> wsl --import Debian D:\wsl\ D:\debian.tar --version 2
提供的安装位置已在使用中。
PS C:\Windows\system32> wsl --import Debian D:\wsl2\ D:\debian.tar --version 2
PS C:\Windows\system32> debian.exe config --default-user xiaobaozi
PS C:\Windows\system32>
```

注意：Ubuntu和Debian要分开放，否则报错`提供的安装位置已在使用中。`

# [如何使用 WSL 在 Windows 上安装 Linux](https://learn.microsoft.com/zh-cn/windows/wsl/install)


开发人员可以在 Windows 计算机上同时访问 Windows 和 Linux 的强大功能。 通过适用于 Linux 的 Windows 子系统 (WSL)，开发人员可以安装 Linux 发行版（例如 Ubuntu、OpenSUSE、Kali、Debian、Arch Linux 等），并直接在 Windows 上使用 Linux 应用程序、实用程序和 Bash 命令行工具，不用进行任何修改，也无需承担传统虚拟机或双启动设置的费用。

## 先决条件

必须运行 Windows 10 版本 2004 及更高版本（内部版本 19041 及更高版本）或 Windows 11 才能使用以下命令。 如果使用的是更早的版本，请参阅[手动安装页](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)。

## 安装 WSL 命令

现在，可以使用单个命令安装运行 WSL 所需的一切内容。 在管理员模式下打开 PowerShell 或 Windows 命令提示符，方法是右键单击并选择“以管理员身份运行”，输入 wsl --install 命令，然后重启计算机。

```
wsl --install
```

此命令将启用运行 WSL 并安装 Linux 的 Ubuntu 发行版所需的功能。 （[可以更改此默认发行版](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands#install)）。

如果你运行的是旧版，或只是不想使用 install 命令并希望获得分步指引，请参阅[旧版 WSL 手动安装步骤](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)。

首次启动新安装的 Linux 发行版时，将打开一个控制台窗口，要求你等待将文件解压缩并存储到计算机上。 未来的所有启动时间应不到一秒。

 >备注
 >
 >上述命令仅在完全未安装 WSL 时才有效，如果运行 `wsl --install` 并查看 WSL 帮助文本，请尝试运行 `wsl --list --online` 以查看可用发行版列表并运行 `wsl --install -d <DistroName>` 以安装发行版。 若要卸载 WSL，请参阅[卸载旧版 WSL](https://learn.microsoft.com/zh-cn/windows/wsl/troubleshooting#uninstall-legacy-version-of-wsl) 或[注销或卸载 Linux 发行版](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands#unregister-or-uninstall-a-linux-distribution)。


## 更改默认安装的 Linux 发行版

默认情况下，安装的 Linux 分发版为 Ubuntu。 可以使用 `-d` 标志进行更改。

-   若要更改安装的发行版，请输入：`wsl --install -d <Distribution Name>`。 将 `<Distribution Name>` 替换为要安装的发行版的名称。
-   若要查看可通过在线商店下载的可用 Linux 发行版列表，请输入：`wsl --list --online` 或 `wsl -l -o`。
-   若要在初始安装后安装其他 Linux 发行版，还可使用命令：`wsl --install -d <Distribution Name>`。

>提示
>
>如果要通过 Linux/Bash 命令行（而不是通过 PowerShell 或命令提示符）安装其他发行版，必须在命令中使用 .exe：`wsl.exe --install -d <Distribution Name>` 或若要列出可用发行版，则使用：`wsl.exe -l -o`。

如果在安装过程中遇到问题，请查看[疑难解答指南的安装部分](https://learn.microsoft.com/zh-cn/windows/wsl/troubleshooting#installation-issues)。

要安装未列为可用版本的 Linux 发行版，可使用 TAR 文件[导入任何 Linux 发行版](https://learn.microsoft.com/zh-cn/windows/wsl/use-custom-distro)。 在某些情况下，[与 Arch Linux](https://wsldl-pg.github.io/ArchW-docs/How-to-Setup/) 一样，也可使用 `.appx` 文件进行安装。 还可以通过 WSL 创建自己的[自定义 Linux 发行版](https://learn.microsoft.com/zh-cn/windows/wsl/build-custom-distro)，以供使用。

## 设置 Linux 用户信息

安装 WSL 后，需要为新安装的 Linux 发行版创建用户帐户和密码。 请参阅[设置 WSL 开发环境的最佳做法](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment#set-up-your-linux-username-and-password)指南来了解详细信息。

## 设置和最佳做法

建议遵循[设置 WSL 开发环境的最佳做法](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment)这一指南，逐步了解以下操作：为已安装的 Linux 发行版设置用户名和密码、使用基本的 WSL 命令、安装和自定义 Windows 终端、针对 Git 版本控制进行设置、使用 VS Code 远程服务器进行代码编辑和调试、文件存储的最佳做法、设置数据库、装载外部驱动器、设置 GPU 加速等。

## 检查正在运行的 WSL 版本

可列出已安装的 Linux 发行版，并通过在 PowerShell 或 Windows 命令提示符中输入以下命令来检查每个发行版的 WSL 版本：`wsl -l -v`。

要在安装新的 Linux 发行版时将默认版本设置为 WSL 1 或 WSL 2，请使用命令 `wsl --set-default-version <Version#>`，将 `<Version#>` 替换为 1 或 2。

要设置与 `wsl` 命令一起使用的默认 Linux 发行版，请输入 `wsl -s <DistributionName>` 或 `wsl --setdefault <DistributionName>`，将 `<DistributionName>` 替换为要使用的 Linux 发行版的名称。 例如，从 PowerShell/CMD 输入 `wsl -s Debian`，将默认发行版设置为 Debian。 现在从 Powershell 运行 `wsl npm init` 将在 Debian 中运行 `npm init` 命令。

要在 PowerShell 或 Windows 命令提示符下运行特定的 WSL 发行版而不更改默认发行版，请使用命令 `wsl -d <DistributionName>`，将 `<DistributionName>` 替换为要使用的发行版的名称。

有关详细信息，请参阅 [WSL 的基本命令](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands)指南。

## 将版本从 WSL 1 升级到 WSL 2

使用 `wsl --install` 命令安装的新 Linux 安装将默认设置为 WSL 2。

`wsl --set-version` 命令可用于从 WSL 2 降级到 WSL 1，或将以前安装的 Linux 发行版从 WSL 1 更新到 WSL 2。

要查看 Linux 发行版是设置为 WSL 1 还是 WSL 2，请使用命令 `wsl -l -v`。

要更改版本，请使用 `wsl --set-version <distro name> 2` 命令将 `<distro name>` 替换为要更新的 Linux 发行版的名称。 例如，`wsl --set-version Ubuntu-20.04 2` 会将 Ubuntu 20.04 发行版设置为使用 WSL 2。

如果在 `wsl --install` 命令可用之前手动安装了 WSL，则可能还需要[启用 WSL 2 所使用的虚拟机可选组件](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual#step-3---enable-virtual-machine-feature)并[安装内核包](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)（如果尚未这样做）。

如需了解更多信息，请参阅 [WSL 命令参考](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands)以获取 WSL 命令列表，并参阅[比较 WSL 1 和 WSL 2](https://learn.microsoft.com/zh-cn/windows/wsl/compare-versions)，获取有关用于你的工作场景的指南，或参阅[设置 WSL 开发环境的最佳做法](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment)，了解有关使用 WSL 设置良好开发工作流的一般指南。

## 使用 WSL 运行多个 Linux 发行版的方法

WSL 支持运行想要安装的任意数量的不同 Linux 发行版。 这可能包括从 [Microsoft Store](https://aka.ms/wslstore) 选择发行版、[导入自定义发行版](https://learn.microsoft.com/zh-cn/windows/wsl/use-custom-distro)或[生成自己的自定义发行版](https://learn.microsoft.com/zh-cn/windows/wsl/build-custom-distro)。

安装后，有几种方法可以运行 Linux 发行版：

1.  [安装 Windows 终端](https://learn.microsoft.com/zh-cn/windows/terminal/get-started)（推荐）：使用 Windows 终端支持你想要安装的任意数量的命令行，并允许你在多个标签或窗口窗格中打开它们并在多个 Linux 发行版或其他命令行（PowerShell、命令提示符、PowerShell、Azure CLI 等）之间快速切换。 可使用独特的配色方案、字体样式、大小、背景图像和自定义键盘快捷键来完全自定义终端。 [了解详细信息。](https://learn.microsoft.com/zh-cn/windows/terminal)
2.  通过访问 Windows“开始”菜单并键入已安装的发行版的名称，可以直接打开 Linux 发行版。 例如：“Ubuntu”。 这会在其自己的控制台窗口中打开 Ubuntu。
3.  在 Windows 命令提示符或 PowerShell 中，可以输入已安装的发行版的名称。 例如：`ubuntu`
4.  在 Windows 命令提示符或 PowerShell 中，可以在当前命令行中打开默认的 Linux 发行版，方法是输入：`wsl.exe`。
5.  在 Windows 命令提示符或 PowerShell 中，可以在当前命令行中使用默认的 Linux 发行版，而无需输入新的发行版名称，方法是输入：`wsl [command]`。 将 `[command]` 替换为 WSL 命令，例如，替换为 `wsl -l -v` 以列出已安装的发行版，或 `wsl pwd` 以查看当前目录路径在 WSL 中的装载位置。 在 PowerShell 中，命令 `get-date` 将提供 Windows 文件系统中的日期，而 `wsl date` 将提供 Linux 文件系统中的日期。

选择的方法应取决于所执行的操作。 如果已在 Windows 提示符或 PowerShell 窗口中打开 WSL 命令行并想退出，请输入命令：`exit`。


## 把WSL迁移出C盘

使用WSL2一段时间后，C盘很可能爆满，从而计算机运行效率会被大幅降低，所以我们应该把安装的系统文件移出C盘。wsl迁移具体操作步骤如下：

```shell
PS C:\WINDOWS\system32> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
PS C:\WINDOWS\system32> wsl --shutdown
PS C:\WINDOWS\system32> wsl --export Ubuntu E:\export_Ubuntu.tar
正在导出，这可能需要几分钟时间。
操作成功完成。
PS C:\WINDOWS\system32> wsl --unregister Ubuntu
正在注销。
操作成功完成。
PS C:\WINDOWS\system32> wsl --import Ubuntu E:\wsl\Ubuntu\ E:\export_Ubuntu.tar
正在导入，这可能需要几分钟时间。
操作成功完成。
PS C:\WINDOWS\system32> Ubuntu.exe config --default-user username
PS C:\WINDOWS\system32> wsl -s Ubuntu
操作成功完成。
PS C:\WINDOWS\system32> del E:\export_Ubuntu.tar
PS C:\WINDOWS\system32>
```

如果安装的是Ubuntu-22.04则执行`Ubuntu2204.exe config --default-user usernamexx`来更改用户名。

## WSL2和Windows系统进行文件交互

在wsl2中键入`pwd`会打印出`/home/username`

键入`cd /mnt && ls`能打印出`c  d  e  f  h  wsl  wslg`，随后便能选择进入自己想操作的盘并使用Linux系统操作命令来进行文件交互。复制文件夹（要加-r）过程如下：

```md
cp /mnt/f/blog/blog2/blog2/myblog/ /home/xxxxx
cp: -r not specified; omitting directory '/mnt/f/blog/blog2/blog2/myblog/'
xxx@DESKTOP-ROR9E2L:~$ cp -r /mnt/f/blog/blog2/blog2/myblog/ /home/xxxx
```

# Ubuntu换源

[# Ubuntu 22.04换国内源 清华源 阿里源 中科大源 163源](https://blog.csdn.net/xiangxianghehe/article/details/122856771)

注意：版本不对会报错

备份镜像源设置文件

`sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak`

编辑镜像源设置文件

`sudo vi /etc/apt/sources.list`

复制下面的所有内容替换掉原文件中的所有内容(一次只可以选一个镜像源，根据你的情况选)

清华源：

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

然后执行命令：

```
sudo apt-get update
sudo apt-get upgrade
```





# Debian配置 国内镜像源

注意：版本不对会报错

## 方式1：先换源再安装证书

[链接](https://www.cnblogs.com/wanderoff-night/p/17988720)

国内镜像源很多，这里选择清华的镜像源。

1. 打开并登录 WSL - Debian
    
2. 将原有文件设置备份
    
    ```shell
    sudo mv ~/etc/apt/sources.list ~/etc/apt/sources.list.backup
    ```
    
3. 创建新的 `~/etc/apt/sources.list`
    
    ```shell
    sudo touch ~/etc/apt/sources.list
    ```
    
4. 在新的 `sources.list` 中输入：
    
    ```shell
    deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
    
    deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
    
    deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
    
    deb http://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
    ```
    
5. Debian CLI 中输入下述代码安装 镜像站 https 证书
    
    ```shell
    sudo apt update -y
    sudo apt install apt-transport-https ca-certificates
    ```
    
6. 在 `~/etc/apt/sources.list` 中，将所有的 `http://` 改成 `https://`
    
7. Debian CLI 中输入下述代码更新 Debian 软件
    
    ```shell
    sudo apt update -y && sudo apt upgrade -y
    ```
    

至此，国内镜像站配置完成。


## 方式2：先安装证书再换源

[链接](https://zhuanlan.zhihu.com/p/99938831)

二、切換Debian的apt-get至國內源

1. 用預設源做sudo apt-get update（**不能一開始就換源！**否則換源後需要的ca-certificates組件沒辦法裝）

```bash
sudo apt-get update
```

2. 安裝ca-certificates，Debian Buster(10) 版開始己經不需要apt-transport-https

```bash
sudo apt-get install ca-certificates
```

3. 打開內建的vim，編輯/etc/apt/sources.list，注意是**vi**不是**vim**

```text
sudo vi /etc/apt/sources.list
```

4. 將以下內容取代原本的，記得先備份（[trusted=yes]很重要）。內建的vim有bug，Insert模式下按方向鍵會輸出ABCD，所以正確姿勢是**按dd清除每一行，按i進入insert，按右鍵黏貼，按esc退出insert，:wq保存後離開。**

```text
deb [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
# deb-src [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
deb [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
#deb-src [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
deb [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
# deb-src [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
deb [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
# deb-src [trusted=yes] https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
```

5. 幹掉坑爹內建vim，換成完整版，測試一下換源是否成功。

```bash
sudo apt-get update
sudo apt-get remove vim-common
sudo apt-get install vim
```



# 安装Docker

[# Win11 安装 Docker Desktop 和 WSL2 并进行安装位置迁移](https://blog.csdn.net/cn_ljr/article/details/132047516)

[# 解决在Windows11上新安装的Docker Desktop一直显示"starting the Docker Engine"登录不上去的问题](https://zhuanlan.zhihu.com/p/663821762)

[# Docker在Window10系统上Docker Engine的配置](https://blog.csdn.net/m0_56000832/article/details/120900633)
## [System requirements](https://docs.docker.com/desktop/install/windows-install/#system-requirements)

有两种后端：WSL 2 backend、Hyper-V backend and Windows containers

### WSL 2 backend

- WSL version 1.1.3.0 or later.
    
- Windows 11 64-bit: Home or Pro version 21H2 or higher, or Enterprise or Education version 21H2 or higher.
    
- Windows 10 64-bit:
    
    - We recommend Home or Pro 22H2 (build 19045) or higher, or Enterprise or Education 22H2 (build 19045) or higher.
    - Minimum required is Home or Pro 21H2 (build 19044) or higher, or Enterprise or Education 21H2 (build 19044) or higher.
- Turn on the WSL 2 feature on Windows. For detailed instructions, refer to the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
    
- The following hardware prerequisites are required to successfully run WSL 2 on Windows 10 or Windows 11:
    
    - 64-bit processor with [Second Level Address Translation (SLAT)](https://en.wikipedia.org/wiki/Second_Level_Address_Translation)
    - 4GB system RAM
    - Enable hardware virtualization in BIOS. For more information, see [Virtualization](https://docs.docker.com/desktop/troubleshoot/topics/#virtualization).

### Hyper-V backend and Windows containers

---

- Windows 11 64-bit: Pro version 21H2 or higher, or Enterprise or Education version 21H2 or higher.
    
- Windows 10 64-bit:
    
    - We recommend Home or Pro 22H2 (build 19045) or higher, or Enterprise or Education 22H2 (build 19045) or higher.
    - Minimum required is Home or Pro 21H2 (build 19044) or higher, or Enterprise or Education 21H2 (build 19044) or higher.
    
    For Windows 10 and Windows 11 Home, see the system requirements in the WSL 2 backend tab.
    
- Turn on Hyper-V and Containers Windows features.
    
- The following hardware prerequisites are required to successfully run Client Hyper-V on Windows 10:
    
    - 64 bit processor with [Second Level Address Translation (SLAT)](https://en.wikipedia.org/wiki/Second_Level_Address_Translation)
    - 4GB system RAM
    - Turn on BIOS-level hardware virtualization support in the BIOS settings. For more information, see [Virtualization](https://docs.docker.com/desktop/troubleshoot/topics/#virtualization).


## WSL2后端安装

```powershell
PS C:\Windows\system32> wsl --update
正在安装: 适用于 Linux 的 Windows 子系统
已安装 适用于 Linux 的 Windows 子系统。
PS C:\Windows\system32>
```

建议不要装最新版本的Docker Desktop，目前最新版是Docker Desktop 4.25.0.0(126437)，而本文安装的是Docker Desktop 4.23.0.0(120376)。如何到Docker官网（[https://www.docker.com/](https://link.zhihu.com/?target=https%3A//www.docker.com/)）下载非最新版本方法如下：

_第一步_：

![](https://pic4.zhimg.com/80/v2-2f4c3c0f212eec507406bbd0fddecf83_1440w.webp)

选择Products中的Docker Desktop

_第二步_：

![](https://pic3.zhimg.com/80/v2-ceabee0caab988bae9d25b42712b586e_1440w.webp)

进入Docker Desktop界面后往下滑，找到Download Docker Desktop，点击该链接进入下载界面

_第三步_：

![](https://pic3.zhimg.com/80/v2-f4be567a0f2c8c9821c14a222537b42e_1440w.webp)

在Docker Desktop下载界面中滑到界面下方找到Download and Install，点击链接正式进入安装适配电脑系统选择界面

_第四步_：

![](https://pic2.zhimg.com/80/v2-5477ae147f022e5b1f5e635ee68b8ef1_1440w.webp)

本文选“Docker Desktop for Windows”，这个可根据自己电脑系统选择对应的链接，然后进入下载须知界面

_第五步_：

![](https://pic3.zhimg.com/80/v2-9cf054f4c59d5988ebb58ea6b469858e_1440w.webp)

点击“Release notes”链接进入下载版本选择界面

_第六步_：

![](https://pic2.zhimg.com/80/v2-4bd93ab1d2103a1a8905bfc9c724a91d_1440w.webp)

在界面右侧Contents中选择自己所需的Docker Desktop安装版本进入该版本下载界面

_第七步_：

![](https://pic2.zhimg.com/80/v2-8c3098002d82936f79d978edee390a59_1440w.webp)

本文选择4.23.0版，进入后点击Windows链接即可实现Docker Desktop 4.23.0下载。

在Ubuntu中输入`docker version`输出如下内容：

```shell
Client:
 Cloud integration: v1.0.35+desktop.10
 Version:           25.0.2
 API version:       1.44
 Go version:        go1.21.6
 Git commit:        29cf629
 Built:             Thu Feb  1 00:22:06 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Desktop
 Engine:
  Version:          25.0.2
  API version:      1.44 (minimum version 1.24)
  Go version:       go1.21.6
  Git commit:       fce6e0c
  Built:            Thu Feb  1 00:23:17 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.28
  GitCommit:        ae07eda36dd25f8a1b98dfbf587313b99c0190bb
 runc:
  Version:          1.1.12
  GitCommit:        v1.1.12-0-g51d5e94
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
## 解决在Windows11上新安装的Docker Desktop一直显示"starting the Docker Engine"登录不上去的问题

在Windows11上新安装Docker Desktop后登录时出现下图的问题，等了很久都没有登录进去。通过在全网进行一番查询，最终结合好几位大佬分享的解决方案成功解决了上述问题，在此记录一下。

![](https://pic1.zhimg.com/80/v2-b08753828367da42408bd2f798e20e20_1440w.webp)

## 解决要点：

**1.** 本文是在windows11家庭中文版上安装Docker Desktop 4.23.0.0(120376)，而该windows版本是没有Hyper-V这个内置虚拟机的，需要自己手动安装，安装方法请参考（[超详细Windows11家庭中文版系统安装Docker-20230401_win11 安装docker-CSDN博客](https://link.zhihu.com/?target=https%3A//blog.csdn.net/m0_37802038/article/details/129893827)）中8.1的方法——将一段代码复制在txt文本里，并重命名为Hyper.cmd（如何对文本重命名请参考[windows11怎么样将txt文件改成bat文件？_win11记事本转bat-CSDN博客](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_73859306/article/details/128304446)），然后右键以管理员方式运行，最后输入“Y”重启电脑后即可。

**2.** 另外，注意检查CPU是否开启虚拟化（检查方法参考[解决Docker 一直starting 的办法](https://link.zhihu.com/?target=https%3A//blog.csdn.net/bindandan/article/details/109167328)中点1，另外该链接中其它两点也很有价值，需按照要求操作一遍确认一下前面手动安装的Hyper-V虚拟机有没有被勾选（查看方法：找到控制面板--程序--程序和功能--启用或关闭windows功能 ，勾选Hyper-V），以及Docker服务有没有开启）。

![](https://pic3.zhimg.com/80/v2-94143a5f0ab5430f5e5469b954ed6f0e_1440w.webp)

勾选Hyper-V虚拟机

![](https://pic3.zhimg.com/80/v2-5c61ba90e4fd6123e268b3cd099b330e_1440w.webp)

启动Docker服务

**3.** 如果在cmd中按照上面的命令服务Docker启动不成功，很大可能是你不是以管理员权限打开的cmd，怎么以管理员权限运行cmd请参考[尚能西：Windows11系统中怎么以管理员权限运行CMD](https://zhuanlan.zhihu.com/p/663818549)。

**4.** 还需要安装wsl(适用于Linux的Windows子系统)。注意windows powershell也要以管理员权限打开（可参考[在 Windows 11 中以管理员身份运行 PowerShell 的 8 种方法 – Digitalixy.com](https://link.zhihu.com/?target=https%3A//digitalixy.com/windows/1060520.html)）

![](https://pic1.zhimg.com/80/v2-aa9faac1a4f3206bfe2805cab6411d10_1440w.webp)

安装wsl

**5.** 完成上面的操作之后还需在windows powershell中运行命令（.\DockerCli.exe -SwitchDaemon）把docker daemon启动。注意要先cd到Docker Desktop安装路径（一般默认Docker Desktop安装路径为C:\Program Files\Docker\Docker）下再运行上述命令。

![](https://pic3.zhimg.com/80/v2-d115902f608fef3a36e575113391e156_1440w.webp)

**6.** 以管理员身份运行Docker Desktop桌面图标，第一次登录需要一点时间（大概5min左右）；

![](https://pic4.zhimg.com/80/v2-648da6999162bb37bd1b42d9798c044f_1440w.webp)

然后看到下图界面就说明Docker Desktop启动成功了。在Docker Engine中设置镜像拉取的路径（设置方法可以参考[Docker在Window10系统上Docker Engine的配置](https://link.zhihu.com/?target=https%3A//blog.csdn.net/m0_56000832/article/details/120900633)）。

![](https://pic1.zhimg.com/80/v2-606a11cf43ecc5ad0a144f0f13b3ebf4_1440w.webp)

配置Docker Engine

**7.** 下面试着拉取一个Deepl翻译引擎的镜像，命令：docker pull kanikig/deepl-bk。

![](https://pic3.zhimg.com/80/v2-5fff2c5b125886b2279df87988ac9e36_1440w.webp)

##  配置 Docker Desktop 、迁移并测试

WSL2 安装完成后，进入 Docker Desktop：

可以看到已经能够使用 Docker Desktop 了

我们先进行一些设置

点击右上角的齿轮图标进入设置，完成以下操作：

还需配置一下阿里云镜像加速，可参考：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors 进行配置

Docker Desktop 中原先的配置：

配置了阿里云镜像加速后Docker Desktop 中的配置：

这里的地址参照上面给出参考文档换成自己的即可

### 迁移 docker-desktop-data 和 docker-desktop 安装位置

Docker Desktop 会创建两个发行版：docker-desktop-data 和 docker-desktop，它们的默认位置在 C:\Users\<你电脑的用户名>\AppData\Local\Docker\wsl，我们同样可以参考上面导入导出 Ubuntu-22.04 的过程将docker-desktop-data 和 docker-desktop 迁移到其他位置

wsl --export docker-desktop-data e:\docker-desktop-data.tar

wsl --export docker-desktop e:\docker-desktop.tar

wsl --unregister docker-desktop-data

wsl --unregister docker-desktop

wsl --import docker-desktop-data E:\wsl\docker-desktop-data E:\docker-desktop-data.tar

wsl --import docker-desktop E:\wsl\docker-desktop E:\docker-desktop.tar

以上步骤完成后，看到指定安装的新位置下有以下 .vhdx 文件：

说明迁移成功
### 拉取 Nginx 镜像并测试运行

打开 Docker Desktop 后，在 Windows 终端（Windows Terminal）中进入 Ubuntu-22.04 ：

拉取 Nginx 镜像

docker pull nginx

运行 Nginx

docker run -p 80:80 -d nginx

访问 http://localhost ，能够看到以下页面说明 Nginx 运行成功

可以看到 Docker Desktop 中也有了对应的容器

# 解决删除文件后 WSL2 磁盘空间不释放的问题

[# 解决删除文件后 WSL2 磁盘空间不释放的问题](https://zhuanlan.zhihu.com/p/521747491)

今天突然发现 `C` 盘快满了，想起来之前把 `Docker` 容器的数据持久化到了 `WSL2` 的某个目录下，于是就想着把不需要的文件清理了。但清理完毕之后我发现 `C` 盘的剩余空间并没有变大，非常的奇怪。后来我在网上搜索了很久，终于找到了原因和解决方法。

## 1 分析原因

不同于 `WSL1`，`WSL2` 本质上是虚拟机，所以 `Windows` 会自动创建 `vhdx` 后缀的虚拟磁盘文件作为存储。这个 `vhdx` 后缀的虚拟磁盘文件特点是可以自动扩容，但是一般不会自动缩容。一旦有很多文件把它“撑大”，即使把这些文件删除它也不会自动“缩小”。所以删除文件后还需要我们手动进行压缩才能释放磁盘空间。

## 2 如何操作

### 2.1 找到并确定要压缩的虚拟磁盘文件

首先，我们搜索并找到 `ext4.vhdx` 文件。

我的 `WSL2` 有如下的 Linux distributions：

```powershell
➜  wsl -l -v
  NAME                   STATE           VERSION
* Ubuntu-20.04           Running         2
  docker-desktop         Running         2
  docker-desktop-data    Running         2
```

我搜索到的 `ext4.vhdx` 文件路径如下：

- `C:\Users\richa\AppData\Local\Docker\wsl\data\ext4.vhdx`
- `C:\Users\richa\AppData\Local\Docker\wsl\distro\ext4.vhdx`
- `C:\Users\richa\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\ext4.vhdx`

由于我这里 `C` 盘空间不足主要是 `Ubuntu-20.04` 删除文件后 `ext4.vhd` 没有缩容引起的，所以只压缩了它的 `ext4.vhdx`。

如果出现删除 `Docker` 镜像、删除 `Docker` 容器后磁盘占用没有缩小，应该也可以类比操作。

### 2.2 备份虚拟磁盘文件所属的 Linux distribution（可选）

在 `PowerShell` 中执行：

```text
# 关闭 WSL2 中的 linux distributions
wsl --shutdown
# 备份指定的 Linux distribution 到指定的位置
wsl --export Ubuntu-20.04 D:\Ubuntu-20.04.tar
```

如果后续步骤出现错误，可以从备份的文件中恢复。本人后续步骤并没有出现错误，所以并没有实践恢复的操作。

有需要的读者可以参考：[wsl2-backup-and-restore-images-using-import-and-export](https://link.zhihu.com/?target=https%3A//www.virtualizationhowto.com/2021/01/wsl2-backup-and-restore-images-using-import-and-export/)

### 2.3 压缩虚拟磁盘文件

在 `PowerShell` 中执行：

```text
# 关闭 WSL2 中的 linux distributions
wsl --shutdown
# 运行管理计算机的驱动器的 DiskPart 命令
diskpart
```

在新打开的 `DiskPart` 命令窗口中执行：

```text
# 选择虚拟磁盘文件
select vdisk file="C:\Users\richa\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\ext4.vhdx"
# 压缩文件
compact vdisk
# 压缩完毕后卸载磁盘
detach vdisk
```

上述操作执行完毕，`WSL2` 删除文件后空出来的磁盘空间就被释放了。











