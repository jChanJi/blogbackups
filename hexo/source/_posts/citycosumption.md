---
title: 机器学习-无监督学习-聚类K-means算法-对31省消费水平分类
toc: true
categories: 
    -machinelearning
tags: [python,machinelearning,K-means]

---
## 前言：
>此篇笔记主要根据南京大学礼欣老师的[《Python机器学习应用》](http://www.icourse163.org/learn/BIT-1001872001?tid=1001965001#/learn/announce)整理而成，详细内容请看礼欣老师的mooc课程。

<!--more-->

## 数据介绍：
现有1999年全国31个省份城镇居民家庭平均每人全年消费性支出的八个主
要变量数据，这八个变量分别是：食品、衣着、家庭设备用品及服务、医疗
保健、交通和通讯、娱乐教育文化服务、居住以及杂项商品和服务。利用已
有数据，对31个省份进行聚类。。数据下载[点击我](https://github.com/jChanJi/static_resource/blob/master/clustering/TestData.txt)

## 主要参数
1. n_clusters：用于指定聚类中心的个数
2. init：初始聚类中心的初始化方法
3. max_iter：最大的迭代次数
4. 一般调用时只用给出n_clusters即可，init
默认是k-means++，max_iter默认是300
5. data：加载的数据
6. label：聚类后各数据所属的标签
7. axis: 按行求和
8. fit_predict()：计算簇中心以及为簇分配序号

## 代码
```python
import numpy as np
from sklearn.cluster import KMeans
 
 
def loadData(filePath):
    fr = open(filePath,'r+')
    lines = fr.readlines()
    retData = []
    retCityName = []
    for line in lines:
        items = line.strip().split(",")
        retCityName.append(items[0])
        retData.append([float(items[i]) for i in range(1,len(items))])
    return retData,retCityName
 
     
if __name__ == '__main__':
    data,cityName = loadData('F:/data/clustering/city.txt')
    km = KMeans(n_clusters=4)
    label = km.fit_predict(data)
    expenses = np.sum(km.cluster_centers_,axis=1)
    #print(expenses)
    CityCluster = [[],[],[],[]]
    for i in range(len(cityName)):
        CityCluster[label[i]].append(cityName[i])
    for i in range(len(CityCluster)):
        print("Expenses:%.2f" % expenses[i])
        print(CityCluster[i])
```

## 结果
```markdown
Expenses:4441.04
['安徽', '湖南', '湖北', '广西', '海南', '四川', '云南']
Expenses:7754.66
['北京', '上海', '广东']
Expenses:5567.33
['天津', '江苏', '浙江', '福建', '重庆', '西藏']
Expenses:3788.76
['河北', '山西', '内蒙古', '辽宁', '吉林', '黑龙江', '江西', '山东', '河南', '贵州', '陕西', '甘肃', '青海', '宁夏', '新疆']

```

## 注：当改变簇n_clusters为8(CityCluster长度也设置为8)时结果
```python
Expenses:3497.85
['山西', '内蒙古', '黑龙江', '河南', '宁夏']
Expenses:5311.98
['天津', '江苏', '重庆', '云南']
Expenses:7010.02
['北京', '浙江']
Expenses:7517.80
['广东']
Expenses:4357.67
['安徽', '湖南', '湖北', '广西', '海南', '四川']
Expenses:5287.90
['福建', '西藏']
Expenses:8247.69
['上海']
Expenses:3934.21
['河北', '辽宁', '吉林', '江西', '山东', '贵州', '陕西', '甘肃', '青海', '新疆']

```
我们发现簇多所分的层次就越多
