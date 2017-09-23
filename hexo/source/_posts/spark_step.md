---
title: 伪分布式spark安装配置
toc: true
categories:
    -bigdata
tags: [spark,big data]

---
## 前言
>本教程为spark的伪分布式教程，学生党条件有限所以伪分布式应该是比较好的选择。其中要注意的是版本匹配的问题。

<!--more-->

## 一、版本
1. CentOS7
2. jdk:jdk1.8.0_131
2. hadoop：2.6.0
3. scala:2.11.11
4. spark:2.1.1

## 二、下载
1. [spark2.1.1](http://spark.apache.org/downloads.html)
2. [scala2.11.11](http://www.scala-lang.org/download/all.html)

## 三、安装配置

### 1、解压,修改权限
```markdown
sudo tar -zxvf spark-2.1.1-bin-hadoop2.6.tgz -C /usr/local
cd /usr/local
sudo chown -R hadoop:hadoop ./spark
```
### 2、在解压的spark目录下新建文件/test/hellospark,写上内容

### 3、进入scala模式

```markdown
   cd /spark/bin
   ./spark-shell
```

### 4、运行代码 

```markdown
5、val lines = sc.textFile(“../test/hellospark”)
   lines.count()
   lines.first()
```

### 5、修日志级别
```markdown
   cd /usr/local/spark/conf
   cp  log4j.properties.template  log4j.properties   
   sudo vim log4j.properties 
```
   将其中的rootCategory=INFO 改为 WARN
