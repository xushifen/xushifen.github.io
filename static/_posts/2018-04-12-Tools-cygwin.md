---
layout: post
title:  "Linux模拟软件"
date:   2018-04-11 23:10:00
categories: 生信工具
tags: 辅助软件
---



### [Cygwin](https://en.wikipedia.org/wiki/Cygwin)：生信分析必备工具之一

在window下运行的一款Linux模拟软件，对学习Linux操作命令很方便。与此同时，对我们这种生物信息分析狗，既要在windows下工作，又希望能像linux bash 样可以使用命令行操作文件，该软件是个很友好的工具。



####  安装Cygwin

**1)**. 登陆[Cygwin官网](https://cygwin.com/)，选择与windows系统相同位数的版本

**2)**. 打开下载好软件安装：一般选择默认设置

* Install from Internet

* Root Install Directory：根目录所在位置（默认）

* Local Package Directory：Cygwin安装包路径，可以自己选择

* Direct Connection

* Select a download site：可以选择添加[清华镜像](https://link.jianshu.com/?t=http://mirrors.tuna.tsinghua.edu.cn/cygwin/)

* Select Package：一般的Vim，openssh，cur, wget, gcc等等包一起选择安装

* finish the installation




#### 安装apt-cyg包

因为Cygwin坑爹的设计，每次安装新的包总需要重新安装该软件一次，因此有开发者设计了apt-cyg包，该包功能上类似Linux的[apt-get](https://baike.baidu.com/item/apt-get/2360755?fr=aladdin)：

* 功能： 搜索、安装、升级、卸载软件

**1)** apt-cyg的依赖软件，安装方法同上，选择全部包一起安装

```
wget
tar
gawk
bzip2
make
```

**2)** 安装apt-cyg包

```bash
lynx -source rawgit.com/transcode-open/apt-cyg/master/apt-cyg > apt-cyg
install apt-cyg /bin
```

**3)** apt-cyg用法：安装，删除，更新

```bash
apt-cyg install packagename
apt-cyg remove packagename
apt-cyg update packagename
```

**4)** [fatty](https://github.com/juho-p/fatty)包使用：Cygwin只能有一个窗口，这个设计真是有点坑。因此有人开发了fatty，功能是可以create many tabs

> To create new tab, press ctrl+shift+T
> ctrl+shift+W closes the tab
> To change active tab, click it with mouse or press shift+(left arrow|right arrow)
> To move tab, press ctrl+shift+(arrow direction)

```bash
git clone https://github.com/juho-p/fatty.git
cd fatty
make
cp src/fatty.exe /bin
```



#### 免密码登陆

通过Cygwin登陆大型机，需要用户名和密码，但可以通过[RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))生成公钥形式解决

```bash
ssh-keygen -t rsa -C "username@genomics.cn"
cat ~/.ssh/id_rsa.pub
```

* 将公钥复制粘贴到大型机的.ssh目录下authorized_keys（若没有，则创建）




#### 本地与大型机传输文件

使用[scp](https://www.cnblogs.com/likui360/p/6011769.html)命令，在使用期先设置大型机的别名信息，方便使用scp命令

```bash
echo -e "Host  alias1 \n HostName 192.114.23.43 \n User username" > ~/.ssh/config 
echo -e "Host  alias2 \n HostName 172.23.22.323 \n User username" >> ~/.ssh/config 

scp -r alias1:/hwfssz1/filedir/  ./  # 大型机传输到本地
scp file.txt  alias2:/hwfssz1/filedir/  # 本地传输到大型机
```



#### 小技巧

* 命令行打开文件或者文件夹：cygstart: start a program or open a file or url

```bash
cygstart file.txt
cygstart dir/
```

* windows cmd下使用链接


> - mklink /d /h /j     <Link>   <Target>
>
>
> - /d : creates a     directory symbolic link. 
> - /h : creats a     hard link instead of a symbolic link
> - /j create a     Directory junction
> - eg : mklink /d     c:Users\Desktop\AAA      c:\Document\ABC   在桌面建立名为AAA的软连接，连接至文件的ABC目录

* 修改find：Cygwin默认使用windows的find

```bash
mv usr/bin/find.exe  find.cyg.exe
echo 'alias find=\'find.cyg\'' >> ~/.bash_profile
```





#### Reference

1. [安装过程](http://allthingsmarked.com/2006/08/17/how-to-set-up-a-windows-ssh-server-for-vnc-tunneling/)


参考文章如引起任何侵权问题，可以与我[联系](https://github.com/HuaZou/)，谢谢。
