---
layout: post
title:  "Anaconda：管理python包和软件的工具"
date:   2018-06-22 10:36:00
updated: 2019-08-30
categories: 生物信息
tags: anaconda
excerpt: Python渐渐成为最流行的编程语言之一，在数据分析、机器学习和深度学习等方向Python语言更是主流。Python的版本比较多，并且它的库也非常广泛，同时库和库之间存在很多依赖关系，所以在库的安装和版本的管理上很麻烦。Conda是一个管理版本和Python环境的工具，它使用起来非常容易。
---

![](https://raw.githubusercontent.com/HuaZou/HuaZou.github.io/master/_posts/img/anaconda.png)

### 命令集合

|    类别     | 功能         | 命令                                       |
| :-------: | :--------- | ---------------------------------------- |
| anaconda  | 集群登陆       | conda  install anaconda-client  anaconda login |
| conda环境管理 | 查看所有环境     | conda  info -e                           |
|           | 新建环境       | conda  create --name py35 python=3.5     |
|           | 使用自己环境     | source  activate name  conda  deactivate |
|           | 使用他人环境     | source  activate /anaconda3/envs/name  conda  deactivate |
|           | 使用它人base环境 | source  anaconda/etc/profile.d/conda.sh  source  activate base  conda  deactivate |
|           | 删除环境       | conda  remove --name py35 --all          |
|    库管理    | 查看已安装包     | conda  list                              |
|           | 查看某环境下已安装包 | conda  list -n py35                      |
|           | 搜索包        | conda  search numpy                      |
|           | 安装包        | conda  install module1 module2           |
|           | 安装包到指定环境   | conda  install -n py35 module=version    |
|           | 更新所有包      | conda  update --all                      |
|           | 更新包        | conda  update package                    |
|           | 卸载包        | conda  remove package                    |
|           | 卸载某个环境包    | conda  remove -n py35 package            |
|   特殊安装包   | 包安装失败情况下   | conda install  --override-channels -c <https://metachannel.conda-forge.org/defaults,bioconda,conda-forge/picrust2,--max-build-no>  picrust2 |

### conda打包软件

1. build.sh:编译软件需要运行的代码

2. meta.yaml:描述软件以及提供依赖环境等

   | 参数类别 | 参数                 | 含义                    | 实列                                |
   | ---- | ------------------ | --------------------- | --------------------------------- |
   | 包描述  | name               | 包名                    | name:  package1                   |
   |      | version            | 版本                    | version:  {{version}}             |
   | 包来源  | url/git_url/hg_url | tar.gz文件地址            | url:  package1.{{version}}.tar.gz |
   |      | md5/sha1/sha256    | 文件的唯一识别码              | md5:  md2sum                      |
   |      | path               | 文件来自本地相对路径            | path:  ../src                     |
   |      | folder             | 指定目录放置文件源             | folder:  my-destination/folder    |
   | 建立包  | number             | 建立次数默认是0              | number:  0                        |
   |      | script             | 代替build.sh。建立包        |                                   |
   | 依赖包  | build              | 包安装的依赖工具（tools）       | build:   - git  - cmake           |
   |      | host               |                       |                                   |
   |      | run                | 包安装的依赖项(dependencies) | run:   - numpy 1.4                |

