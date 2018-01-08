---
title: 对四维的鸢尾花数据使用PCA进行降维
categories:
      -machinelearning
tags: [python,machinelearning,PCA]
date: 2018-1-9
---

## 前言：
>此篇笔记主要根据南京大学礼欣老师的[《Python机器学习应用》](http://www.icourse163.org/learn/BIT-1001872001?tid=1001965001#/learn/announce)整理而成，详细内容请看礼欣老师的mooc课程。

## 数据介绍：
对四维的鸢尾花数据使用PCA进行降维并且可视化，数据格式如下：
![图1](https://raw.githubusercontent.com/jChanJi/static_resource/master/img/iris.PNG)
<!--more-->
## 代码
```python
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
from sklearn.datasets import load_iris

data = load_iris()
y = data.target
X = data.data
pca = PCA(n_components=2)
reduced_X = pca.fit_transform(X)

red_x, red_y = [], []
blue_x, blue_y = [], []
green_x, green_y = [], []

for i in range(len(reduced_X)):
    if y[i] == 0:
        red_x.append(reduced_X[i][0])
        red_y.append(reduced_X[i][1])
    elif y[i] == 1:
        blue_x.append(reduced_X[i][0])
        blue_y.append(reduced_X[i][1])
    else:
        green_x.append(reduced_X[i][0])
        green_y.append(reduced_X[i][1])

plt.scatter(red_x, red_y, c='r', marker='x')
plt.scatter(blue_x, blue_y, c='b', marker='D')
plt.scatter(green_x, green_y, c='g', marker='.')
plt.show()

```

## 结果
![图1](https://raw.githubusercontent.com/jChanJi/static_resource/master/img/iris_res.PNG)
