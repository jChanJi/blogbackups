---
title: CentOS 7 安装eclipse mars 2
toc: true
categories:
    -others
tags: [eclispe,CentOS 7]
---

#### 操作系统：CentOS 7
#### eclispe版本：Eclipse Mars 2

<!--more-->

## 下载安装
> ### 1.下载安装[eclipse][2]

> ### 2.解压 

```markdown
 sudo tar -zxvf [下载的安装包名称] -C [安装的目录]  
```

> ### 3.创建软链接

```markdown
 sudo ln -s /安装的目录/eclipse/eclipse  /usr/bin/eclipse
```

> ### 4.添加图标

```markdown
gedit /usr/share/applications/eclipse.desktop
```
将下面内容添加到文件中
```mrkdown
[Desktop Entry]
Encoding=UTF-8
Name=Eclipse
Comment=Eclipse Mar2
Exec=/usr/bin/eclipse
Icon=/[解压的目录]/eclipse/icon.xpm
Categories=Application;Development;Java;IDE
Version=1.0
Type=Application
Terminal=0
```


[2]: http://mirrors.ustc.edu.cn/eclipse/technology/epp/downloads/release/mars/2/eclipse-jee-mars-2-linux-gtk-x86_64.tar.gz "eclise下载"