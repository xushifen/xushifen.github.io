---
layout: post
title:  "如何简单快速地搭建个人博客站点"
date:   2019-08-29 10:36:00
categories: 其他
tags: 搭建博客
---

### 搭建个人网站和博客

> 搭建个人博客主要有两种方式
>
> - 第一种方法是使用**Giuthub Pages + jekyll** 搭建；
> - 第二种方法是使用**Giuthub Pages + Hexo**搭建
>
> 这里主要介绍第一种方法，并记录搭建过程遇到的坎坷。



### 准备工作

1. ##### 安装Jekeyll：Jekyll是一个生成静态网页的工具。**Windows**(上)和**mac os**(下)安装指南：

   ```markdown
   1. 下载[Ruby+Devkith](ttps://rubyinstaller.org/downloads/)，默认安装

   2. 下载[RubyGerms](https://rubygems.org/pages/download)，加压，进入管理员模式下命令行安装，输入ruby setup.rb。（该软件是Ruby的包管理器，也就是下载中心）

   3. 安装Jekyll，在cmd模式下，输入命令 gem install jekyll，使用jekyll -v 检查是否安装成功。
   ```

   ```markdown
   1. xcode-select --install : 编译模块

   2. ：安装ruby
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   brew install ruby 

   export PATH=/usr/local/opt/ruby/bin:$PATH

   3. 安装rbenv
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   brew install rbenv

   rbenv init

   curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash

   4. 安装Jekyll 
   gem install --user-install bundler jekyll

   export PATH=$HOME/.gem/ruby/X.X.0/bin:$PATH

   ```


2. ##### 安装git bash


### 搭建步骤

1. ##### 注册github账号，并且建立博客Github仓库。创建方法有两种：
   ```markdown
   1. 从头开始设置主题

   2. 直接fork他人的blog主题：
   	* [极简主题](https://github.com/poole/poole)
   	* [Jekyll 主题](http://jekyllthemes.org/)

   注意：仓库名字一定要和自己的账户名字一致：**Repository name** :  *githubname.github.io*
   ```
2. ##### 了解Jekyll博客的目录结构
   - **_config_yml**：全局配置文件，包含基础设置、侧边栏设置、社交账号和评论系统等
   - **_posts：**博客文章存放位置
   - **_layouts：**页面模板
   - **_includes**：html页面设计
   - **_sass：**存放样式表
   - **assets：**原生的资源文件，如js、css和image等文件夹
   - **categories：**文章分类
   - **index.html：**首页网页格式
   - ***自定义文件和目录***
3.  ##### 撰写博客
   ```markdown
   博客文章可以使用markdown或html来编写，因为markdown是轻量编辑语言，所以推荐使用markdown来书写文章。但需要注意几点：

   1. md文件需要包含描述文章类型的title，具体参照[Github md](https://help.github.com/en/articles/basic-writing-and-formatting-syntax)。

   2. md文件的命名需要符合[Permalinks](https://jekyllrb.com/docs/permalinks/)。

   3. md文件所包含的图片等，需要放置在_posts/img/目录，并且路径需要换成网页版绝对路径。如 “https://raw.githubusercontent.com/HuaZou/HuaZou.github.io/master/_posts/img/R.cbind-1.png”
   ```
4. ##### 本地渲染后上传至git仓库

   - 打开powershell，并进入到博客目录；
   - 输入 bundle exec jekyll serve进行渲染生成静态网页；
   - 本地查看静态网址：<http://127.0.0.1:4000/>



### Github Pages的局限性

Github Pages 并不是无限存储和无限流量的静态站点服务，一些限制如下：

- 内容存储不能超过1GB。
- 每个月100GB流量带宽。
- 每小时编译构建次数不超过10次。（在线修改重新编译并未发现这个限制）

### 注意事项
1. 文章内图片过大，可以通过压缩图片[图快好](https://www.tuhaokuai.com/)


### 安装过程遇到的问题

1. **Could not find gem 'github-pages x64-mingw32' in any of the gem sources listed in**
   **your Gemfile.**
   - 问题：无法渲染web网页。
   - 原因：可能版本太低或者依赖包缺失。
   - [解决办法](https://github.com/prose/starter/issues/44)：更新bundle后安装依赖包。
     - bundle update 
     - bundle install
2. **GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.**
   - 问题：无法查看静态网页。
   - 原因：连接github出错或者md文件名字包含中文
   - 解决办法：用英文重新命名md文件



### 引用

1. [Github Pages + jekyll 全面介绍极简搭建个人网站和博客](https://blog.csdn.net/tom_221x/article/details/84630283)

   ​

参考文章如引起任何侵权问题，可以与我[联系](https://github.com/HuaZou/)，谢谢。