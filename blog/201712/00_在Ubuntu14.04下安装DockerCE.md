## 在Ubuntu14.04下安装Docker CE

从2017年3月开始，Docker开始分为社区版本和企业版，也就是Docker CE和Docker EE, 原来Ubuntu14.04下，通过`sudo apt-get install docker.io`来进行安装的方式已经过时了。在这里，会详细介绍如何在ubuntu14.04 LTS下安装Docker社区版，也就是Docker CE。

在开始之前，请确保你先做好一些[前期准备工作](#前期准备工作)，然后在开始[安装Docker](#)。

### 前期准备工作

#### 操作系统

安装Docker CE，你需要以下其中一种64位的Ubuntu操作系统：

- Artful 17.10 (Docker CE 17.11 Edge only)
- Zesty 17.04
- Xenial 16.04 (LTS)
- Trusty 14.04 (LTS)

Docker CE 被允许安装上在 `x86_64`，`armhf`， 以及 `s390x` (IBM z Systems) 架构之上的Ubuntu操作系统。

#### 卸载过期的Docker版本

旧版本的Docker，通常叫做`docker`或是`docker-engine`，如果他们之前被安装在操作系统中，请先进行卸载：

```shell
$ sudo apt-get remove docker docker-engine docker.io
```

