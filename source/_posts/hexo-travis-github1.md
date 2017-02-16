---
title: hexo+travis+github博客环境搭建（一）----本地hexo环境搭建  
date: 2017-02-14   
categories: web   
tags: hexo   
comments: true   

---

### 前言
作为一个不懂前端、不懂css、不懂git的人来说，搭建一个博客真的是太折腾，经过几天在一篇篇网文的指导和说明下，终于搭建完毕，第一篇博客当然要对这几天折腾进行一下总结梳理。

### 博客系统介绍
在github上，博客的源文件和静态页面放置在内不同的分支上，源程序放在sources分支，静态文件放在master分支，博客发布时，只需将写好的博客源文件（*.md）上传至sources分支，travis就会自动构建静态网页，然后再上传至master分支，这样做的好处就是随时都可以写博客，只需要push一个md文件就ok，不会束缚到某台电脑。

<!-- more -->

Hexo：是一个快速、简洁且高效的博客框架。

travis：自动构建静态页面，并push回github。 

github：放置博客源文件和静态页面文件



### 详细的搭建过程
共分3步走   
一、本地博客环境搭建   
二、上传源文件至github   
三、使用travis自动发布

### 本地博客环境搭建
git下载：https://git-scm.com/   
node.js下载：https://nodejs.org/en/


选定相应的版本安装即可    


安装完毕后，在D盘下创建文件夹（blog）存放博客源文件

在开始程序中找到Git Bash   
```
#进入blog目录
cd /d/blog

# 安装hexo
npm install -g hexo-cli

# 初始化
hexo init hexo

```
初始化结束后，会创建一个hexo的文件夹，进入文件
```
cd hexo

# 启动hexo server服务器
hexo server
```
![安装完毕](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/clipboard.png)

打开浏览器，输入http://localhost:4000

至此本地的hexo博客就可以了，但这是本地的，只有自己看，怎么给别人看呢，先别急，咱门再调整调整，默认的博客主题太丑，安装个next的主题

安装
```
cd /d/blog/hexo
git clone https://github.com/iissnan/hexo-theme-next themes/next

```
启动
修改d:/blog/hexo/_config.yml配置项theme：

```
# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: next
```

再次运行
```
hexo server
```

打开浏览器，输入http://localhost:4000，可以看到页面的主题已经发生了改变

next更加详细的设置参考：https://hexo.io/zh-cn/docs/configuration.html

将生成的博客源文件做成仓库，默认在本地的master分支，准备上传github
```
#初始化仓库，生成.git文件夹
git init

#将d:/blog/hexo文件添加至仓库
git add .

#提交仓库的第一个
git commit -v "Update1"
```

至此本地博客系统建立完毕。