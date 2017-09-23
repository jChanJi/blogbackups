---
title: 伪分布式hbase安装配置
toc: true
categories:
    -bigdata
tags: [hbase,big data]

---

## 前言
>网上有很多的教程，大体流程都差不多，但是在很多细节配置方面有点区别，本教程适用于伪分布式环境下（一般自己电脑上练习伪分布式够了）的hbase的基本安装配置。hadoop伪分布式环境已经搭建好,如果没有搭建好，推荐教程 [hadoop伪分布式教程](http://www.powerxing.com/install-hadoop-in-centos/),hbase官方[中文文档](http://abloz.com/hbase/book.html)

<!--more-->

## 一、版本

1. CentOS7
2. jdk:openjdk1.7.0_141
2. hadoop：2.6.0
3. hbase:0.98.13
4. 一定要注意jdk,hadoop和hbase的版本匹配问题,可到官网查看！

## 二、下载
1.[hbase-0.98.13-hadoop2-bin.tar.gz](http://archive.apache.org/dist/hbase/0.98.13/hbase-0.98.13-hadoop2-bin.tar.gz)

## 三、安装配置

### 1、解压文件到指定目录
```markdown
tar -zxvf hbase-0.98.13-hadoop2-bin.tar.gz -C /usr/local
```
### 2、重命名
```markdown
cd /usr/local
sudo mv [解压后的文件名] [hbase]
```
### 3、修改hbase-site.xml
```markdown
cd /hbase/conf
sudo vim hbase-site.xml
```
将内容改为
```markdown

<configuration>
<property>
    <name>hbase.rootdir</name>
    <value>hdfs://localhost:9000/hbase</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/usr/local/hbase/data/zkData</value>
  </property>
<property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
</configuration>
```
说明：<br>
1、很多教程的hbase.rootdir的hdfs的端口都和官网配置一样是8020，这里根据你自己的实际端口号配置，我的默认的为9000（一般都是），如果端口配置错误的话，之后的进程都能启动，但是在hdfs中没有创建hbase文件，也不能通过60010端口访问web UI.<br>
2、dataDir的目录可以自己定义，不需要预先创建，hbase会根据配置自动生成。

### 3、修改hbase-env.sh
```markdown
sudo vim hbase-env.sh
```
添加自己的JAVA_HOME路径
```markdown
export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk/
```
### 4、修改regionservers
在/etc/hosts文件中添加主机名映射，再regionservers中默认的localhost改为主机名
```mardown
sudo vim etc/hosts
```
在最后一行添加 127.0.0.1 master
```mardown
sudo vim regionservers
```
将localhost改为mater</br>
说明：如果ip映射出现问题后面的regionserver会启动不了

### 5、启动服务
首先先启动hadoop
```markdown
start-all.sh
```
再启动hbase
```markdown
cd /usr/local/hbase/bin
./hbase-daemon.sh start zookeeper
./hbase-daemon.sh start regionserver
./hbase-daemon.sh start master
```
### 6、查看web UI
在浏览器中输入localhost:60010<br>
如果能正常显示页面说明配置成功<br>
说明：刚开启服务后由于hadoop处于安全模式导致不能访问，可以等几十秒再次访问或者通过命令
```markdown
hadoop dfsadmin -safemode leave 
```
解除保护
## 四、常用的一些命令

### 1、从hdfs导入导出表
```markdown
1）导入
./hbase org.apache.hadoop.hbase.mapreduce.Driver import 表名    数据文件位置

2)导出
./hbase org.apache.hadoop.hbase.mapreduce.Driver export 表名    数据文件位置
```
注意：直接操作会报没有jar包的错误，根据提示将hbase的jar包put进提示的hdfs路径中即可
## 五、遇到的错误和解决办法

### 1、无法启动HRegionServer和HMaster
报错日志
```markdown
2017-06-13 19:10:12,458 ERROR [main] master.HMasterCommandLine: Master exiting
java.lang.RuntimeException: Failed construction of Master: class org.apache.hadoop.hbase.master.HMaster
  at org.apache.hadoop.hbase.master.HMaster.constructMaster(HMaster.java:3033)
  at org.apache.hadoop.hbase.master.HMasterCommandLine.startMaster(HMasterCommandLine.java:193)
  at org.apache.hadoop.hbase.master.HMasterCommandLine.run(HMasterCommandLine.java:135)
  at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:70)
  at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
  at org.apache.hadoop.hbase.master.HMaster.main(HMaster.java:3047)
Caused by: java.net.BindException: 无法指定被请求的地址
  at sun.nio.ch.Net.bind0(Native Method)
  at sun.nio.ch.Net.bind(Net.java:463)
  at sun.nio.ch.Net.bind(Net.java:455)
  at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
  at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
  at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2488)
  at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:590)
  at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1956)
  at org.apache.hadoop.hbase.master.HMaster.<init>(HMaster.java:507)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
  at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
  at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
  at org.apache.hadoop.hbase.master.HMaster.constructMaster(HMaster.java:3028)
  ... 5 more

```
解决办法
```markdown
我们可以看到Caused by: java.net.BindException: 无法指定被请求的地址，所以有可能是外网的的影响，所以先关闭网络连接，再启动服务，发现成功了，然后再开启网络。
```
### 2、启动hbase服务时找不到pid文件
问题原因
```markdown
1. hbase进行大量的插入时region server 所分配的内存堆过小
2. pid文件保存在tmp目录下容易丢失。
```
解决办法
```markdown
1. 在hb的hbase-env.sh中
# The maximum amount of heap to use, in MB. Default is 1000.
# export HBASE_HEAPSIZE=1000
将1000改成30720

2. 在hbase-env.sh中修改pid文件的存放路径：
在hbase-env.sh中下面的文字默认是注释掉的，放开即可，也可以自己指定存放位置：
# The directory where pid files are stored. /tmp by default.  
 export HBASE_PID_DIR=/var/hadoop/pids  
```
