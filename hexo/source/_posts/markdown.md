---
title: sumlime text3 配置Markdown
toc: true
categories:
    -others
tags: [sublime text3,markdown]
---
## 一、sumlime text3 配置Markdown和常用快捷键
### 1、sumlime text3 配置Markdown
>#### 1、安装package Control<br>

>#### 2、安装Markdown Preview<br>
>&ensp; 2.1、 按Shif + Alt + P打开<br>
>&ensp; 2.2、输入pcip,回车（进入install package）<br>

<!--more-->

>#### 3、安装Markdown  Editing<br>
>&ensp;3.1、进入 install package<br>
>&ensp;3.2、输入 Markdown Editing // Markdown编辑和语法高亮支持<br>

>#### 4、安装Markdown  Previewer<br>
>&ensp; 4.1、进入 install package<br>
>&ensp; 4.2、Markdown  Previewer  //Markdown导出html预览支持<br>

>#### 5、安装OmniMarkup Previewer<br>
>&ensp; 5.1、进入 install package<br>
>&ensp; 5.2、OmniMarkup Previewer //在浏览器中实时预览

### 2、常用快捷键
> 1. Ctrl + Alt + O //在浏览器中打开
> 2. Alt + M  //生成html文件
> 3. Ctrl+Alt+O: Preview Markup in Browser.
> 4. Ctrl+Alt+X: Export Markup as HTML.
> 5. Ctrl+Alt+C: Copy Markup as HTML.

## 二、使用Cmd markdown在线编辑
> 1. 在线[编辑] [1] 网址
> 2. 也可以下载客户端离线编辑[客户端] [2]
> 3. 如果想转成html等其他功能需要付费，不过基础功能已经差不多够用了


## 遇到的问题

### 1、Ctrl+Alt+O后打开了浏览器但是不能够预览ｍａｒｋｄｏｗｎ页面

```markdown
    在 Preferences > Package Settings > OmniMarkupPreviewer > Settings - User 中粘贴以下代码即可

    {
    "renderer_options-MarkdownRenderer": {
        "extensions": ["tables", "fenced_code", "codehilite"]
    }
    }
```

### 2、在sublime text3 中切换不了中文
```markdown
sudo apt-get update && sudo apt-get upgrade
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
cd ~/sublime-text-imfix
sudo ./ sublime-imfix
然后重启sublime text3
```


[1]: https://www.zybuluo.com/mdeditor "Cmd Markdown"
[2]: https://www.zybuluo.com/cmd/ "下载"

