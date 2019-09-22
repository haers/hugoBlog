---
title: idea搭建zookeeper源码服务器
date: 2019-07-18 19:21:47
categories: [oss]
tags: [开源]
---

## 背景
今年的一个小目标就是看一个java的开源软件，首先从zookeeper入手，毕竟比别的”简单“些
<!--more-->
## 搭建环境
* GitHub下载源码
* 不要使用master分支，自己使用的时候，有实现类冲突，不知道后续会解决没有，因此选择了稳定版本3.5.5
* 去掉 zookeeper-server项目中pom文件中的两个jetty的scope，因为咱们运行的时候，还是使用的jetty服务器，所以得引入进来，要不然会报错

![](https://i.loli.net/2019/07/18/5d3057eabaa9a31488.png)

* 添加resources编译，因为要不然resources目录不会编辑进入class文件夹，日志就出不来，会有警告

![](https://i.loli.net/2019/07/18/5d30586e058da63807.png)

* copy 根目录下的conf下的log4j文件到zookeeper-server项目下的resources下
* copy 根目录下的conf下的 zoo_sample.cfg到自己的文件目录下，修改端口最好，因为防止和brew搭建zookeeper冲突，可以更好的校验

* 配置 zoo_sample.cfg 修改名字为 zoo.cfg，修改日志路径和端口如下：

![](https://i.loli.net/2019/07/18/5d3059730262189597.png)

* 配置idea启动，就ok了，有日志显示

![](https://i.loli.net/2019/07/18/5d3059bbf09f994251.png)

* 测试

```
zkCli -server localhost:2189
```

自己添加个对象，然后获取下，说明成功了，停用服务器，客户端再获取，会抛出异常，说明成功啦！

# 后记
网上的18年3月份的教程都是ant编译的，但是现在maven已经支持了，所以省事了不少，有问题可以及时反馈我的邮箱和评论。
