## 在Ubuntu14.04下安装Docker CE(1) - repository篇

从2017年3月开始，Docker开始分为社区版本和企业版，也就是Docker CE和Docker EE, 原来Ubuntu14.04下，通过`sudo apt-get install docker.io`来进行安装的方式已经过时了。在这里，会详细介绍如何在ubuntu14.04 LTS下安装Docker社区版，也就是Docker CE。

在开始之前，请确保你先做好一些[前期准备工作](#前期准备工作)，然后开始[安装Docker](#)。

### 前期准备工作

#### 操作系统

安装Docker CE，你需要以下其中一种64位的Ubuntu操作系统：

- Artful 17.10 (Docker CE 17.11 Edge only)
- Zesty 17.04
- $Xenial$ 16.04 (LTS)
- Trusty 14.04 (LTS)

Docker CE 被允许安装上在 `x86_64`，`armhf`， 以及 `s390x` (IBM z Systems) 架构之上的Ubuntu操作系统。

#### 清理过期的Docker版本

旧版本的Docker，通常叫做`docker`或是`docker-engine`，如果他们之前被安装在操作系统中，请先进行卸载：

```shell
$ sudo apt-get remove docker docker-engine docker.io
```

使用`apt-get`查看是否还有相关包，如果没有则表示清理成功。

### 安装Docker CE

Docker CE有多种不同的安装模式：

- 建议大部分的用户通过[设置Docker存储库(Docker's repositories)](#)，再由储存库进行安装，这样方便安装，且容易升级。
- 一些用户可以采用下载debian包的方式来进行[手动安装](#)和手动升级。这种做法在某些情况下会很有用，例如离线环境下安装Docker CE。
- 在某些开发或测试环境下，部分用户可以选择采用[自动化脚本](#)来安装Docker CE。

#### 通过repositories安装

当你第一次在一台全新机器上安装Docker CE之前，你需要先设置一下Docker的repositories。然后才可以从repositories中进行安装或升级Docker的操作。

##### 设置repositories

1. 升级`apt`源的索引信息：

```shell
$ sudo apt-get update
```

2. 安装指定包，这些包可以允许`apt`通过HTTPS来访问repository：

```shell
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

3. 添加Docker官方提供的GPG key：

```shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

通过使用`apt-key finger print`搜索指纹码最后8位，可以验证你在上一步所生成的指纹码应该为`9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`

```shell
$ sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

4. 使用下面的命令来设置一个stable的repository，以下命令只基于X86_64平台，如需使用其他平台安装，请参阅[官方文档](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#set-up-the-repository):

```shell
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

##### 安装Docker CE

1. 升级`apt`源的索引信息：

```shell
$ sudo apt-get update
```

2. 安装最新版本的Docker CE，或者跳过这一步，按照第三步的方法指定安装的版本：

```shell
sudo apt-get install docker-ce
```

> 当有多个Docker的repository库的时候？
> 当你的机器上有多个可用的Docker repository。如果使用`apt-get install`或是`apt-get update`命令进行安装或更新操作的时候，没有指定安装的Docker版本，将总是安装这些repository中能支持的**最高版本**的Docker，这时请注意这是否你真正需要的版本。如果不是，请在安装时指定版本。

3. 在真实的产品环境系统中，你可能会需要安装指定版本的Docker CE，而不是总是安装默认版本。使用下面的命令可以列出允许使用的相关版本信息：

```shell
$ apt-cache madison docker-ce

docker-ce | 17.09.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
```

命令返回结果中列举的内容依赖于系统中存在的repository，可以从返回结果中选取指定版本号来安装Docker CE。第二列是版本信息。第三列是repository的名字，标识了安装包来自于那个repository，以及当前版本的稳定性。如果要安装指定版本，把版本号添加到包名字(这里包名就是docker-ce)的后面，用(`=`)分隔开：

```shell
$ sudo apt-get install docker-ce=<VERSION>
```

安装完成后，Docker daemon(Docker服务端守护进程)会自动开始运行。

4. 运行一个`hello-world` image来验证一下安装是否正确：

```shell
$ sudo docker run hello-world
```

运行这个命令，自动下载一个测试image，然后运行在一个container中。运行后，会打印一个消息，然后自动退出。

```shell

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
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
 
```

Docker CE安装完成后，`docker` group会被自动创建，但是默认没有user在里面。所以你仍然需要使用`sudo` 来启动和运行Docker命令。可以配置[Linux postinstall](https://docs.docker.com/engine/installation/linux/linux-postinstall)来允许非授权用户运行和配置Docker CE。

#### 升级 Docker CE

如果需要升级Docker CE，首先请运行`sudo apt-get update`，然后安装上文所述的安装步骤，重新选择新的Docker CE版本来进行安装。

通过repository来安装Docker CE的方法到这里就结束了，下一篇会继续介绍通过package和脚本来安装Docker CE。