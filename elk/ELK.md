<!-- TOC -->

- [1. ELK地址](#1-elk地址)
- [2. Elasticsearch的安装](#2-elasticsearch的安装)
- [3. 安装Docker ELK](#3-安装docker-elk)

<!-- /TOC -->

# 1. ELK地址
> https://www.elastic.co/cn/downloads/logstash  
> https://www.elastic.co/downloads/kibana

# 2. Elasticsearch的安装
elasticsearch需要暴露两个端口，9200和9300，我们直接使用docker run命令运行一个elasticsearch容器。
```
docker run --name elk-es -d -p 9200:9200 -p 9300:9300 elasticsearch
```

# 3. 安装Docker ELK

1、执行命令：docker pull sebp/elk 将镜像pull到本地来；

2、执行命令：
```
docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -e ES_MIN_MEM=128m  -e ES_MAX_MEM=1024m -it --name elk sebp/elk 
```
将镜像运行为容器，由于我本机内存不符合安装要求，为了保证ELK能够正常运行，加了-e参数限制使用最小内存及最大内存。

3、打开浏览器，输入：http://<your-host>:5601，看到如下界面说明安装成功