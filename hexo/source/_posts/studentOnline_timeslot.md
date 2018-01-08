---
title: 聚类算法DBSCAN实现大学生上网时长分类
categories:
      -machinelearning
tags: [python,machinelearning,DBSCAN]
date: 2018-1-9
---

## 前言：
>此篇笔记主要根据南京大学礼欣老师的[《Python机器学习应用》](http://www.icourse163.org/learn/BIT-1001872001?tid=1001965001#/learn/announce)整理而成，详细内容请看礼欣老师的mooc课程。

<!--more-->

## 数据介绍：
现有大学校园网的日志数据，290条大学生的校园网使用情况数据，数据包
括用户ID，设备的MAC地址，IP地址，开始上网时间，停止上网时间，上
网时长，校园网套餐等。利用已有数据，分析学生上网的模式。数据下载[点击我](https://github.com/jChanJi/jchanji.github.com/tree/master/meterial/data/clustering)



## 主要参数
### eps: 两个样本被看作邻居节点的最大距离
### min_samples: 簇的样本数
### metric：距离计算方式

## 上网时间段
### 代码

```python
import numpy as np
import sklearn.cluster as skc
from sklearn import metrics
import matplotlib.pyplot as plt

mac2id=dict()
onlinetimes=[]
f=open('F:\data\clustering\TestData.txt',encoding='utf-8')
for line in f:
    mac=line.split(',')[2]
    onlinetime=int(line.split(',')[6])
    starttime=int(line.split(',')[4].split(' ')[1].split(':')[0])
    if mac not in mac2id:
        mac2id[mac]=len(onlinetimes)
        onlinetimes.append((starttime,onlinetime))
    else:
        onlinetimes[mac2id[mac]]=[(starttime,onlinetime)]
real_X=np.array(onlinetimes).reshape((-1,2))

X=real_X[:,0:1]

db=skc.DBSCAN(eps=0.01,min_samples=20).fit(X)
labels = db.labels_

print('Labels:')
print(labels)
raito=len(labels[labels[:] == -1]) / len(labels)
print('Noise raito:',format(raito, '.2%'))

n_clusters_ = len(set(labels)) - (1 if -1 in labels else 0)

print('Estimated number of clusters: %d' % n_clusters_)
print("Silhouette Coefficient: %0.3f"% metrics.silhouette_score(X, labels))

for i in range(n_clusters_):
    print('Cluster ',i,':')
    print(list(X[labels == i].flatten()))


plt.hist(X,24)
plt.show()
```

### 结果

```markdown
Labels:
[ 0 -1  0  1 -1  1  0  1  2 -1  1  0  1  1  3 -1 -1  3 -1  1  1 -1  1  3  4
 -1  1  1  2  0  2  2 -1  0  1  0  0  0  1  3 -1  0  1  1  0  0  2 -1  1  3
  1 -1  3 -1  3  0  1  1  2  3  3 -1 -1 -1  0  1  2  1 -1  3  1  1  2  3  0
  1 -1  2  0  0  3  2  0  1 -1  1  3 -1  4  2 -1 -1  0 -1  3 -1  0  2  1 -1
 -1  2  1  1  2  0  2  1  1  3  3  0  1  2  0  1  0 -1  1  1  3 -1  2  1  3
  1  1  1  2 -1  5 -1  1  3 -1  0  1  0  0  1 -1 -1 -1  2  2  0  1  1  3  0
  0  0  1  4  4 -1 -1 -1 -1  4 -1  4  4 -1  4 -1  1  2  2  3  0  1  0 -1  1
  0  0  1 -1 -1  0  2  1  0  2 -1  1  1 -1 -1  0  1  1 -1  3  1  1 -1  1  1
  0  0 -1  0 -1  0  0  2 -1  1 -1  1  0 -1  2  1  3  1  1 -1  1  0  0 -1  0
  0  3  2  0  0  5 -1  3  2 -1  5  4  4  4 -1  5  5 -1  4  0  4  4  4  5  4
  4  5  5  0  5  4 -1  4  5  5  5  1  5  5  0  5  4  4 -1  4  4  5  4  0  5
  4 -1  0  5  5  5 -1  4  5  5  5  5  4  4]
Noise raito: 22.15%
Estimated number of clusters: 6
Silhouette Coefficient: 0.710
Cluster  0 :
[22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22, 22]
Cluster  1 :
[23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23, 23]
Cluster  2 :
[20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20]
Cluster  3 :
[21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21]
Cluster  4 :
[8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8]
Cluster  5 :
[7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7]

```

![图一](https://raw.githubusercontent.com/jChanJi/static_resource/master/img/stuonline1.PNG)
## 上网时长
### 代码
```python
import numpy as np
import sklearn.cluster as skc
from sklearn import metrics
import matplotlib.pyplot as plt

mac2id=dict()
onlinetimes=[]
f=open('F:\data\clustering\TestData.txt',encoding='utf-8')
for line in f:
    mac=line.split(',')[2]
    onlinetime=int(line.split(',')[6])
    starttime=int(line.split(',')[4].split(' ')[1].split(':')[0])
    if mac not in mac2id:
        mac2id[mac]=len(onlinetimes)
        onlinetimes.append((starttime,onlinetime))
    else:
        onlinetimes[mac2id[mac]]=[(starttime,onlinetime)]
real_X=np.array(onlinetimes).reshape((-1,2))

X=np.log(1+real_X[:,1:])
db = skc.DBSCAN(eps=0.14,min_samples=10).fit(X)
labels = db.labels_

print('Labels:')
print(labels)
ratio=len(labels[labels[:] == -1])/len(labels)
print('Noise raito:',format(ratio,'.2%'))

n_clusters_ = len(set(labels))-(1 if -1 in labels else 0)

print('Estimated number of clusters: %d' % n_clusters_)
print("Silhouette Coefficient :%0.3f"% metrics.silhouette_score(X, labels))

for i in range(n_clusters_):
    print('Cluster',i,':')
    count = len(X[labels ==i])
    mean = np.mean(real_X[labels == i][:,1])
    std=np.std(real_X[labels ==i][:,1])
    print('\t number of sample : ',count)
    print('\t mean of sample: ',format(mean,'.1f'))
    print('\t std of sample: ',format(std,'.1f'))

plt.hist(X,24)
plt.show()
```
### 结果
```markdown
Labels:
[ 0  1  0  4  1  2  0  2  0  3 -1  0 -1 -1  0  3  1  0  3  2  2  1  2  0  1
  1 -1 -1  0  0  0  0  1  0 -1  0  0  0  2  0  1  0 -1 -1  0  0  0  3  2  0
 -1  1  0  1  0  0 -1  2  0  0  0  1  3  3  0  2  0 -1  3  0  0  2  0  0  0
  2  1 -1  0  0  0  0  0  0  1 -1  0  3  1  0  1  1  0  1  0  1  0  0 -1  1
  1  0  0  2  0  0  0  2  2  0  0  0 -1  0  0  4  0  1  2 -1  0  1  0  2  0
 -1 -1 -1  0  1  1  3 -1  0  1  0  2  0  0  2  1  1  0  0  0  0  4 -1  0  0
  0  0  2  0  0  0  0 -1  2  0  0  0  0  4  0  0 -1  0  2  0  0 -1  0  1  4
  0  0 -1  1  1  0  0  2  0  0  3 -1 -1 -1  1  0  0  2  1  0 -1 -1  3  2  2
  0  0  3  0  1  0  0  0  3  2  0 -1  0  1 -1 -1  0  2  2  1  4  0  0  1  0
  2  0  0  0  0  1  1  0  0  1  0  4 -1 -1  0  0  0 -1 -1  1 -1  4 -1  0  2
  2 -1  2  1  2 -1  0 -1  0  2  2  1 -1  0  1  2 -1 -1  1 -1  2 -1 -1  1  4
  2  3  1  0  4  0  0  4  2  4  0  0  2 -1]
Noise raito: 16.96%
Estimated number of clusters: 5
Silhouette Coefficient :0.227
Cluster 0 :
     number of sample :  128
     mean of sample:  5864.3
     std of sample:  3498.1
Cluster 1 :
     number of sample :  46
     mean of sample:  36835.1
     std of sample:  11314.1
Cluster 2 :
     number of sample :  40
     mean of sample:  843.2
     std of sample:  242.9
Cluster 3 :
     number of sample :  14
     mean of sample:  16581.6
     std of sample:  1186.7
Cluster 4 :
     number of sample :  12
     mean of sample:  338.4
     std of sample:  31.9
```

![图二](https://raw.githubusercontent.com/jChanJi/static_resource/master/img/stuonline2.PNG)
