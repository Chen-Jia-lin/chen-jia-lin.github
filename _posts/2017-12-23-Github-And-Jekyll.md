---
title: 采用jekyll和github构建个人博客
layout: article
thread: 184
date: 2017-12-23
author: 浪漫双鱼
categories: posts rwd
excerpt: 参考自浪漫双鱼的个人博客
header:
  image: ../assets/in-post/2017-12-23-Github-And-Jekyll.jpg
---

# 步骤

 1. 配置信息
 2. github
 3. 安装Devkit
 4. 安装Ruby
 5. 安装jekyll
 6. 问题集锦

# 配置信息

win10x64

# github

github的操作不再多说，下载git客户端，生成密钥对，将公钥保存在github上，然后本机就可以直接clone项目啦

#### 提交修改主要是以下几步
```python
git add .  //如果没有新加文件可以不用运行此句
git commit -am '描述内容'
git push (-u origin master)  //括号中内容可加可不加
```

# 安装Devkit

这玩意我其实也不造有啥用，因为我第一次搭的时候没装，jekyll也跑的起来~~

官方地址：http://rubyinstaller.org/downloads/

下载适合自己pc的版本，安装的时候尽量安装在根目录下，且路径中不能含空格（这是人家的经验，借来用用）
![](http://bianergege.github.io/img/post926-devkit.jpg)

# 安装Ruby

Ruby是安装jekyll必需的，jekyll需要gem，gem需要Ruby

官方地址：同样是http://rubyinstaller.org/downloads/

下载最新的版本，同样在路径中不能含空格，并且勾选add to path

![](http://bianergege.github.io/img/post926-ruby1.jpg)

安装好后进cmd运行如下代码：
```
cd devkit  //将当前目录转移到devkit解压路径
ruby dk.rb init  //初始化Ruby
ruby dk.rb install
```
![](http://bianergege.github.io/img/post926-ruby2.jpg)

然后运行ruby -v，出现版本号则安装成功

![](http://bianergege.github.io/img/post926-ruby3.jpg)

# 安装gem

可以到https://rubygems.org/下载合适的gem安装包及所需的功能包

- gem -v 检查gem版本 gem1

![](http://bianergege.github.io/img/post926-gem1.jpg)
- gem update --system 更新gem gem2

![](http://bianergege.github.io/img/post926-gem2.jpg)
- gem -v 检查gem版本 gem3
![](http://bianergege.github.io/img/post926-gem3.jpg)

# 安装jekyll   
最关键也是摔跤最多的步骤！！   

- gem install jekyll 安装jekyll
- jekyll -v 安装成功之后，查看版本号
！[](http://bianergege.github.io/img/post926-jekyll.jpg)

至此为止jekyll已经安装完毕，cmd切换至git项目目录，运行jekyll serve，即可在本地localhost:4000进行编译预览

# [问题集锦](http://bianergege.github.io/2016/09/26/jekyll/)

当然没这么简单，实际操作会遇到各种各样的问题~
