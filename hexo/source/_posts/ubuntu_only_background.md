---
title: ubuntu只显示桌面，没有菜单栏
toc: true
categories: 
    -others
tags:
    -ubuntu

---
## 前言
>在裝輸入法的時候好像刪了什麼東西導致電腦重啓的時候只能顯示桌面背景和文件，導航等都沒了，頓時嚇壞我了，找了好多教程終於成功了。

<!--more-->

##安裝unity
```markdown
sudo apt-get install unity
```
##刪除配置文件
```markdown
sudo rm -rf .conf
sudo rm -rf .gconfg
sudo rm -rf ~/.Xauthority
reboot
```
本教程不一定對其他情況也適合
