---
layout: post
title:  "杂货收藏"
date:   2018-04-12 16:10:00
updated: 2019-08-30
categories: 生活笔记
tags: 杂类
excerpt: 科研过程遇到过的一些有用的软件或者网址等。
---

### sci-hub网址

科研狗的福利网站，各种及时免费的文献下载

* [sci-hub 自动更新网址](http://tool.yovisun.com/scihub/)：每天自动更新可用网址
* [sci-hub 桌面助手](https://pan.baidu.com/s/1kVb2pJh#list/path=%2F ) : 桌面版sci-hub
* [Libgen]( <http://libgen.io)  选Scientific Article，显示可以用sci-hub链接
* [chrome插件](http://chromecj.com/productivity/2017-07/773.html) :方便快捷




### 修改jupyter notebook打开路径

jupyter是一个编辑器，可以生成各种你所需要的格式：md、html等等

* 修改配置文件

> 1. 打开 cmd 输入命令 jupyter notebook --generate-config
>
>    PS: 设置前，先将jupyter.notebook.exe加入PATH
>
> 2. 修改 .jupyter/jupyter_notebook_config.py文件，设置 c.NotebookApp.notebook_dir = 'E:\\hua\\github\\python'

* 修改快捷方式标签的目标栏


```
将路径加在 %USERPROFILE%前面，如：E:\hua\python %USERPROFILE%
```

* [使用方法](https://zhuanlan.zhihu.com/p/33105153)



### Anaconda

可以便捷获取包且对包能够进行管理，同时对环境可以统一管理的发行版本。Anaconda包含了conda、Python在内的超过180个科学包及其依赖项。

```bash
conda install -c r rstudio
```



### [Write paper](https://writing.wisc.edu/Handbook/SciRep/ScienceReport.html)

写一篇科研论文是多少科研狗的必经之路，要是写论文能在光锥之内多好啊，这样它就是历史了，历史意味着我学会了，可它偏偏在光锥之外，我要爬过去！！！

```
Introduction
Methods
Results
Discussion
```



### [shadowsocks](https://en.wikipedia.org/wiki/Shadowsocks)

Google因强大的搜索能力而著称，但国内现在不能访问Google，因为需要借用梯子，但VPN不适合科学上网，这个时候shadowsocks就是最佳的帮手了。

> shadowsocks是一种socks5的代理，而socks5的代理服务器则是把你的网络数据请求通过一条连接你和代理服务器之间的通道，由服务器转发到目的地。你没有加入任何新的网络，只是http/socks数据经过代理服务器的转发送出，并从代理服务器接收回应。
>
> VPN，虚拟专用网络，接入VPN就是接入了一个专有网络，那么你访问网络的所有流量都通过这个专有网络的出口出去，同时你的IP地址也变成了有这个VPN分配的IP地址。你与VPN之间的链接通信取决于这个VPN的连接协议总结：
>
> 就目前而言，为了数据安全可以选择VPN，而为了科学上网还是shadowsocks更加能抗GFW的干扰。其中VPN中的Openvpn协议已经被GFW攻破，一杀一个准

**1)**. [使用方法](https://blog.csdn.net/amoscn/article/details/79364599)：登陆[网址](https://github.com/shadowsocks)

* 下载[shadowsocks](https://github.com/shadowsocks/shadowsocks-windows/releases)
* 购买代理
* [chrome插件使用shadowsocks](http://www.111cn.net/sys/Ubuntu/76513.htm)

**2)**. [自建](https://blog.csdn.net/junbujianwpl/article/details/78639247)



### debug帮助网站

* [stackoverflow](http://stackoverflow.com/questions) : 几乎涉及到各种语言的debug
* [github](https://github.com/) :开源者的乐园




### R和python差异

* [数据IO](https://shiring.github.io/r_vs_python/2017/01/22/R_vs_Py_post)
* [数据类型](https://gigadom.wordpress.com/2017/05/22/r-vs-python-different-similarities-and-similar-differences/)






### 引用

1. [修改jupyter路径](https://www.zhihu.com/question/31600197)
2. [Anaconda使用指南](https://zhuanlan.zhihu.com/p/32925500)
3. [shadowsocks和VPN区别](https://blog.csdn.net/qq_30072293/article/details/78485789)

参考文章如引起任何侵权问题，可以与我[联系](https://github.com/HuaZou/)，谢谢。
