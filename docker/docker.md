
<!-- TOC -->

- [1. docker安装](#1-docker安装)
- [2. Docker的三个概念](#2-docker的三个概念)
- [3. 配置国内镜像](#3-配置国内镜像)
    - [3.1. Docker中国区官方](#31-docker中国区官方)
    - [3.2. 阿里云加速器](#32-阿里云加速器)
        - [3.2.1. 阿里云镜像库](#321-阿里云镜像库)
- [4. Docker中关于镜像的基本操作](#4-docker中关于镜像的基本操作)
- [5. DockerUI](#5-dockerui)
    - [5.1. 部署](#51-部署)
    - [5.2. 拉取镜像](#52-拉取镜像)
    - [5.3. 启动容器](#53-启动容器)

<!-- /TOC -->

# 1. docker安装
1、Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
 通过 uname -r 命令查看你当前的内核版本
```
$ uname -r
```
2、使用 root 权限登录 Centos。确保 yum 包更新到最新。
```
$ sudo yum update
```
3、卸载旧版本(如果安装过旧版本的话)
```
$ sudo yum remove docker  docker-common docker-selinux docker-engine
```
4、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
```
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
5、设置yum源
```
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
6、可以查看所有仓库中所有docker版本，并选择特定版本安装
```
$ yum list docker-ce --showduplicates | sort -r
```
7、安装docker
```
$ sudo yum install docker-ce  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0
$ sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-17.12.0.ce
```
8、启动并加入开机启动
```
$ sudo systemctl start docker
$ sudo systemctl enable docker
```
9、验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
```
$ docker version
```
# 2. Docker的三个概念  
1.镜像（Image）：类似于虚拟机中的镜像，是一个包含有文件系统的面向Docker引擎的只读模板。任何应用程序运行都需要环境，而镜像就是用来提供这种运行环境的。例如一个Ubuntu镜像就是一个包含Ubuntu操作系统环境的模板，同理在该镜像上装上Apache软件，就可以称为Apache镜像。 

2.容器（Container）：类似于一个轻量级的沙盒，可以将其看作一个极简的Linux系统环境（包括root权限、进程空间、用户空间和网络空间等），以及运行在其中的应用程序。Docker引擎利用容器来运行、隔离各个应用。容器是镜像创建的应用实例，可以创建、启动、停止、删除容器，各个容器之间是是相互隔离的，互不影响。注意：镜像本身是只读的，容器从镜像启动时，Docker在镜像的上层创建一个可写层，镜像本身不变。

3.仓库（Repository）：类似于代码仓库，这里是镜像仓库，是Docker用来集中存放镜像文件的地方。注意与注册服务器（Registry）的区别：注册服务器是存放仓库的地方，一般会有多个仓库；而仓库是存放镜像的地方，一般每个仓库存放一类镜像，每个镜像利用tag进行区分，比如Ubuntu仓库存放有多个版本（12.04、14.04等）的Ubuntu镜像。

# 3. 配置国内镜像
## 3.1. Docker中国区官方
  使用vi修改 /etc/docker/daemon.json 文件并添加上”registry-mirrors”: [“https://registry.docker-cn.com“]，如下：
```
vi /etc/docker/daemon.json 
{ 
“registry-mirrors”: [“https://registry.docker-cn.com“] 
}
```

## 3.2. 阿里云加速器
```
#您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json 
{
  "registry-mirrors:" [https://855rs5zo.mirror.aliyuncs.com]
}
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 3.2.1. 阿里云镜像库
> https://cr.console.aliyun.com/cn-qingdao/repositories  

# 4. Docker中关于镜像的基本操作
```
[root@xxx ~]# docker search centos    # 查看centos镜像是否存在
[root@xxx ~]# docker pull centos    # 利用pull命令获取镜像
[root@xxx ~]# docker images    # 查看当前系统中的images信息
REPOSITORY      TAG            IMAGE ID       CREATED        SIZE
centos          latest         0584b3d2cf6d   9 days ago     196.5 MB
```

```
#启动容器
[root@VM_81_77_centos ~]# docker run -it centos  #启动并进入容器
[root@73c69ffca57b /]#         #启动之后就进入docker的centos系统
[root@73c69ffca57b /]# exit    #exit可退出容器

#查看docker进程
docker ps      #查看docker启动中的容器
docker ps -a   #查看docker所有的容器
[root@VM_81_77_centos ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                            PORTS                                             NAMES
73c69ffca57b        centos              "/bin/bash"              6 minutes ago       Exited (130) About a minute ago                                                     gallant_brattain
5799a0d13298        centos              "/bin/bash"              7 hours ago         Up 4 hours                                                                          vigilant_brahmagupta
9c54a7a3220f        jenkins/jenkins     "/sbin/tini -- /usr/…"   23 hours ago        Up 23 hours                       0.0.0.0:50000->50000/tcp, 0.0.0.0:800->8080/tcp   festive_johnson
```


# 5. DockerUI

DockerUI基于Docker API，提供等同Docker命令行的大部分功能。
## 5.1. 部署
```
[root@localhost ~]# docker pull uifd/ui-for-docker 
Using default tag: latest
latest: Pulling from uifd/ui-for-docker
841194d080c8: Pull complete 
Digest: sha256:fe371ff5a69549269b24073a5ab1244dd4c0b834cbadf244870572150b1cb749
Status: Downloaded newer image for uifd/ui-for-docker:latest
[root@localhost ~]# docker run -it -d --name docker-web -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock docker.io/uifd/ui-for-docker
ac85040b8a9f2f203c22ba2e4d90a830e31d89a1d6ce77579a7b95de5da51e01
```

## 5.2. 拉取镜像
```
docker pull uifd/ui-for-docker 
```

## 5.3. 启动容器
```
docker run -it -d --name docker-web -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock docker.io/uifd/ui-for-docker 

```

打开ip:9000
