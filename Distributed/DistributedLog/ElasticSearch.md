# ElasticSearch

## 目录

[TOC]

## 一、个人总结

### 1. 安装配置
- 下载地址：[https://www.elastic.co/cn/downloads/elasticsearch
](https://www.elastic.co/cn/downloads/elasticsearch
)
- 进入bin目录
- 双击 elasticsearch.bat 启动 elasticsearch 服务

注:第一次启动时, 要注意此时的 ip 地址(注意下自己的虚拟机或者vpn, 它们的ip可能会产生干扰), 该 ip 地址会被绑定到 enrollment token 中, 在安装 Kibana 时有用
- 启动后第一次会显示一些配置信息,包括默认的用户密码 elastic 和 2xcXm+1k+km7nk6flNRN
- 第一次启动会打印一下内容, 要好好记录下来
- 验证安装结果：

在浏览器输入链接、用户名和密码
```java
https://localhost:9200/
```
- 将 elasticsearch 以服务的方式安装

添加系统环境变量：
```java
Elasticsearch_Server 
E:\ElasticSearch\elasticsearch-8.13.2
```
在系统环境变量 Path 中添加如下路径：
```java
%Elasticsearch_Server%\bin
```
- 安装ElasticSearch服务

打开cmd
```java
elasticsearch-service.bat install
```

## 二、八股问题整理