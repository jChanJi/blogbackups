---
title: ubuntu16.04 anaconda环境下安装tensoflow(GPU)
toc: true
categories:
    -machinelearning
tags: [deeplearning,tensorflow]
---
## 前言
>目前深度学习炙手可热的框架毫无疑问是tensorflow,在本教程主要介绍tensorflow在anaconda中的安装，在火车上实在是无聊，电脑又没有网络，只能打发一下时间。

<!--more-->

## 一、版本
1. anaconda 4.3.21
2. python 3.5
3. tensorflow 1.2.0(github上目前最新版本)
4. ubuntu 16.04
## 二、下载
1. [Anaconda3-4.4.0-Linux-x86_64.sh]()
2. [tensorflow_gpu-1.2.1-cp35-cp35m-linux_x86_64.whl]()
##三、注意事项
```markdown
1、电脑上已经安装了cadu8.0和cudnn5.1环境
2、tensorflow1.2.0版本支持cadu8.0,其他低版本的tensorflow会发生找不到依赖的错误。
３、安装后运行会出现CPU computations,cpu指令集优化的警告，目前没有很好的解决办法，不过影响不大，因为我们主要使用的是GPU.
```
## 四、安装配置

### 1、安装anaconda
```markdown
sudo bash Anaconda3-4.4.0-Linux-x86_64.sh
```

### 2、　安装python3.5环境
```markdown
conda create -n tensorflow python = 3.5
```
### 3、安装tensorflow
```markdown
source activate tensorflow #进入刚才安装好的环境
cd ~/下载　＃进入tensorflow　的pip安装文件的目录
pip install tensorflow_gpu-1.2.1-cp35-cp35m-linux_x86_64.whl #安装tensorflow
```

## 五、测试
进入python环境
```markdown
python
```
运行代码
```python
import tensorflow as tf
sess = tf.Session()

a = tf.constant(10)
b = tf.constant(20)

print(sess.run(a+b))
```
输出结果
```markdown
3
```


























