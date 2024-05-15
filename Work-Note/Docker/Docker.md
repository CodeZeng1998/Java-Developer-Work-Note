

# Docker

[TOC]



# 一、基本概念

1.镜像、容器、仓库



Docker 镜像：Docker仓库类似于代码仓库，是Docker集中存放镜像文件的场所。

Docker 容器：Docker容器类似于一个轻量级的沙箱，Docker利用容器来运行和隔离应用。容器是从镜像创建的应用运行实例。它可以启动、开始、停止、删除，而这些容器都是彼此相互隔离、互不可见的。可以把容器看作一个简易版的Linux系统环境（包括root用户权限、进程空间、用户空间和网络空间等）以及运行在其中的应用程序打包而成的盒子。

Docker 仓库：Docker仓库类似于代码仓库，是Docker集中存放镜像文件的场所。











# 二、基本命令

## 1.获取镜像 

```shell
#NAME是镜像仓库名称（用来区分镜像）, TAG是镜像的标签（往往用来表示版本信息）,不指定TAG的话默认拉取最新版
docker [image] pull NAME[:TAG]

# （默认使用的是官方Docker Hub服务）下面两个命令等价
docker pull mysql:5.7.22
docker pull registry.hub.docker.com/mysql:5.7.22

```



##  2.查看镜像信息

### （1）使用images命令列出镜像

```shell
# REPOSITORY:仓库、TAG：版本信息、MAGE ID： 镜像的ID（唯一标识镜像），如果两个镜像的ID相同，说明它们实际上指向了同一个镜像，只是具有不同标签名称而已、CREATED：创建时间、SIZE：大小
docker images
docker image ls
```



### （2）使用tag命令添加镜像标签

```shell
# （mymysql:latest镜像的ID跟mysql:latest是完全一致的，它们实际上指向了同一个镜像文件，只是别名不同而已。docker tag命令添加的标签实际上起到了类似链接的作用）
docker tag mysql:latest mymysql:latest
```



### （3）使用inspect命令查看详细信息

```shell
docker [image] inspect NAME[:TAG]

docker inspect jrottenberg/ffmpeg:latest
```

### （4）使用history命令查看镜像历史

```shell
# 既然镜像文件由多个层组成，那么怎么知道各个层的内容具体是什么呢？这时候可以使用history子命令，该命令将列出各层的创建信息。
docker history NAME[:TAG]

docker history jrottenberg/ffmpeg:latest
```





## 3.搜寻镜像











































------

参考内容：

《Docker技术入门与实战 (第3版)》杨保华 戴王剑 曹亚仑

