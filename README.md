# Elasticsearch安装指南

## 概述

本指南在`CentOS7`下安装`elasticsearch-6.2.2`版本，其他环境及版本未做测试。

## 安装jdk

```bash
yum install http://mirrors.gfstack.geo/Oracle/JAVA/8/8u162/linux/jdk-8u162-linux-x64.rpm
```

## 创建目录下载文件

`elasticsearch-6.2.2.tar.gz`和`elasticsearch-analysis-ik-6.2.2.zip`可能存在下载速度慢的问题，可到项目的[download](download)目录下下载。

```bash
mkdir /elasticsearch
cd /elasticsearch
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.tar.gz
wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.2.2/elasticsearch-analysis-ik-6.2.2.zip
```

安装`unzip`命令

```bash
yum install unzip
```

解压程序包

```bash
tar -xvf elasticsearch-6.2.2.tar.gz
unzip elasticsearch-analysis-ik-6.2.2.zip
```

创建ik目录，拷贝文件

```bash
mkdir elasticsearch-6.2.2/plugins/ik
mv elasticsearch/* elasticsearch-6.2.2/plugins/ik/
```

删除没用的目录和程序包

```bash
rm -rf elasticsearch elasticsearch-6.2.2.tar.gz elasticsearch-analysis-ik-6.2.2.zip
```

## 配置文件修改

>```bash
>vi /elasticsearch/elasticsearch-6.2.2/config/elasticsearch.yml
>```
>
>`#cluster.name: my-application`：放开注释，修改为自定义的集群名称
>
>`#node.name: node-1`：放开注释，修改为自定义的节点名称
>
>`#network.host: 192.168.0.1`：释放注释，修改为服务器的ip地址



>```bash
>vi /etc/security/limits.conf
>```
>
>添加如下内容：
>
>```bash
>* soft nofile 65536
>* hard nofile 131072
>* soft nproc 2048
>* hard nproc 4096
>```



>```bash
>vi /etc/sysctl.conf
>```
>
>添加如下内容：
>
>```bash
>vm.max_map_count=655360
>```
>
>并执行命令：
>
>```bash
>sysctl -p
>```

## 关闭防火墙

```bash
systemctl stop firewalld.service
systemctl disable firewalld.service
```

## 创建并配置用户

```bash
# 添加一个用户：elasticsearch
useradd elasticsearch
# 给用户elasticsearch设置密码，连续输入2次
passwd elasticsearch
# 创建一个用户组 es
groupadd es
# 分配 elasticsearch 到 es 组
usermod -G es elasticsearch
# 将/elasticsearch目录授权给elasticsearch用户
chown -R elasticsearch /elasticsearch
```

## 切换到elasticsearch用户并启动程序

```bash
su elasticsearch
cd /elasticsearch/elasticsearch-6.2.2/bin/
./elasticsearch
```

或者加`-d`参数后台启动

```
./elasticsearch -d
```

