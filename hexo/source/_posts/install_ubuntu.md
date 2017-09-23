---
title: win10 安装ubuntu 16.04
toc: true
categories:
    -others
tags: [ubuntu,win 10]
---

## 前言
>在win10下安装ubuntu双系统。在笔记本安装ubuntu的时候遇到了很多的挫折，曾经也放弃过，但很不幸的是，在未来的今天又碰上了。有时候问题的答案很简单，但是需要大量的时间去得到它，并不是你的能力不行，而是网上的干扰答案实在是太多，无法分辨谁对谁假的时候，往往会一个个的试过去。我不认为这是个很笨的方法，因为多花点时间总能体会更多的东西。貌似废话有点多，下面直接上干货。

<!--more-->

## 一、版本
1. win10 企业版
2. ubuntu 16.04
3. UltraSO 9.6.6.3300
4. 显卡：GTX 965M
5. cup:i7-6700HQ


## 二、下载
1. [ubuntu 16.04](http://releases.ubuntu.com/16.04.2/ubuntu-16.04.2-desktop-amd64.iso?_ga=2.92867550.254780022.1497589112-1524410519.1497589112)
2. [UltraSO](http://172.19.251.251/files/510300000015EB65/dl.softmgr.qq.com/original/Compression/uiso9_cn_9.6.6.3300.exe)

## 三、安装配置

### 1.将镜像刻录到u盘（至少4g）
```markdown
打开UltraSO，点击试用。
1、文件->打开->镜像位置
2、启动->写如硬盘映像->写入
```
### 2、分配空闲分区
```markdown
1、右击我的电脑->管理->磁盘管理
2、选择非系统盘的主分区，右击->压缩卷，选择压缩大小，一般为50G,我的是100G,
根据自己磁盘情况分配。
```

### 3、将电脑设置为U盘启动
```markdown
插入U盘，进入BIOS将U盘设置为启动项
注：不同主板的BIOS大多都不会相同，所以根据自己电脑型号到网上查找。
```
### 4、安装系统
#### 1、重新启动电脑，会进入安装界面，先择安装系统，进行安装。
#### 2、卡在logo
```markdown
重新启动电脑，在选择系统安装的界面按e,进入grup界面，让后在splash后面加上：空格nomodeset空格，按F10执行。后面重启出了最后一次也是一样操作
```
#### 3、创建分区
```markdwon
1、选择自己分配的空闲的磁盘，进行分盘
2、分盘的注意点是：
    2.1、 /：存储系统文件，建议10GB ~ 15GB,我分配16G；
    2.2、 swap：交换分区，即Linux系统的虚拟内存，建议是物理内存的2倍,我分配16G；
    2.3、 /home：建议最后分配所有剩下的空间；
    2.4、 boot：包含系统内核和系统启动所需的文件，实现双系统的关键所在，建议200M,我分配400M。
3、其他的默认或者根据自己需求设置，点击安装
```

### 5、分辨率问题
一般比较新的N（英伟达）卡会出现没有安装驱动的问题，所以屏幕的分率很低，这时候就需要安装N卡驱动。直接安装会导致开机的时候卡在登陆界面进不去，所以必须借助于bumblebee(大黄蜂)，至于原因有兴趣的可以查一查，这里不多阐述。
```markdwon
sudo apt-get install bumblebee bumblebee-nvidia primus linux-headers-generic
Reboot
```
重新启动后：
```markdown
sudo apt-get purge nvidia-* #删除所有的N卡驱动
sudo add-apt-repository ppa:graphics-drivers/ppa  #添加第三方驱动源
sudo apt-get update #更新源
sudo  apt-cache search nvidia-*  #查询nvidia驱动可用版本，这里推荐到英伟达官网查看自己显卡驱动的版本，我的是375
sudo apt-get install nvidia-375 # 安装驱动

```
打开软件更新器，然后将附加驱动－>未知换成显卡的驱动<br>
最后重新启动，什么都不用做，等开机！


[1]: https://jchanji.github.io "主页"

