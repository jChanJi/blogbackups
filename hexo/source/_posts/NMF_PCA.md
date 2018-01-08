---
title: NMF和PCA算法对人脸进行特征提取并且进行对比
categories:
        -machinelearning
tags: [python,machinelearning,NMF]
date: 2018-1-9
---

## 前言：
>此篇笔记主要根据南京大学礼欣老师的[《Python机器学习应用》](http://www.icourse163.org/learn/BIT-1001872001?tid=1001965001#/learn/announce)整理而成，详细内容请看礼欣老师的mooc课程。

## 数据介绍：
分别使用NMF和PCA算法对人脸进行特征提取并且进行对比


## 代码
```python
from sklearn import decomposition


n_row, n_col = 2, 3
n_components = n_row * n_col
image_shape = (64, 64)


###############################################################################
# Load faces data
dataset = fetch_olivetti_faces(shuffle=True, random_state=RandomState(0))
faces = dataset.data

###############################################################################
def plot_gallery(title, images, n_col=n_col, n_row=n_row):
    plt.figure(figsize=(2. * n_col, 2.26 * n_row))
    plt.suptitle(title, size=16)

    for i, comp in enumerate(images):
        plt.subplot(n_row, n_col, i + 1)
        vmax = max(comp.max(), -comp.min())

        plt.imshow(comp.reshape(image_shape), cmap=plt.cm.gray,
                   interpolation='nearest', vmin=-vmax, vmax=vmax)
        plt.xticks(())
        plt.yticks(())
    plt.subplots_adjust(0.01, 0.05, 0.99, 0.94, 0.04, 0.)


plot_gallery("First centered Olivetti faces", faces[:n_components])
###############################################################################

estimators = [
    ('Eigenfaces - PCA using randomized SVD',
         decomposition.PCA(n_components=6,whiten=True)),

    ('Non-negative components - NMF',
         decomposition.NMF(n_components=6, init='nndsvda', tol=5e-3))
]

###############################################################################

for name, estimator in estimators:
    print("Extracting the top %d %s..." % (n_components, name))
    print(faces.shape)
    estimator.fit(faces)
    components_ = estimator.components_
    plot_gallery(name, components_[:n_components])

plt.show()

```

## 结果
```markdown
downloading Olivetti faces from http://cs.nyu.edu/~roweis/data/olivettifaces.mat to C:\Users\ChanJi\scikit_learn_data
Extracting the top 6 Eigenfaces - PCA using randomized SVD...
(400, 4096)
Extracting the top 6 Non-negative components - NMF...
(400, 4096)
```
![图1](https://raw.githubusercontent.com/jChanJi/static_resource/master/img/face_ori.PNG)
![图2](https://raw.githubusercontent.com/jChanJi/static_resource/master/img/face_PCA.PNG)
![图3](https://raw.githubusercontent.com/jChanJi/static_resource/master/img/face_NMF.PNG)
