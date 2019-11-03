---
title: 如何使用U盘安装centos
date: 2018-11-17 16:29:17
categories: [centos]
tags: [system,centos]
---
如何使用U盘安装centos
自从换了mac本之后，Windows本就很少用了，本来便宜卖给兄弟的，但是嫂子嫌弃太沉了。就一直搁置着，现在因为需要学习些东西，需要将他搞成Linux，因为自己的服务器也是centos的，所以就装的centos，然后再搞一个内网穿透外网就ok了。踩了不少坑，下面把步骤列出来
<!--more-->
## 下载centos，去官网下载就ok了，用的dvd版本。那个版本都行，最好不要用迅雷啥的下载，那样子还得校验md5，网速快的的话使用浏览器默认的下载工具几分钟的事情
## 使用u深度，老毛桃都行，直接制作iso模式，不要使用智能模式！！！
这一步有坑，使用ios刻录到U盘中，相当于u盘就是iso了，如果使用的智能模式，那么还得拷贝，主要是有一步识别的问题！
## 安装
插入u盘
华硕电脑选择F2
然后调整boot顺序
然后到如下页面后
![](https://i.loli.net/2019/11/03/mjHtOw854Zxpl2W.jpg)

这一步的时候，网上教程说是按着e编辑，我这按了好久不行，结果发现是table键，有英文提示，然后将
vmlinuz initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 quiet
改成
vmlinuz initrd=initrd.img linux dd quiet

然后按回车就行
这里显示的硬盘的信息
![](https://i.loli.net/2019/11/03/Kpq1XzO79BV2gjH.jpg)

网上的图，我的是sdb4
使用ctrl alt del重启不行，然后我就关了电源，继续进入，然后按住table，编辑如下
将
vmlinuz initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 quiet
改成
vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sda4（上一步查看的你自己的U盘盘符） quiet
然后按回车就ok了。
![](https://i.loli.net/2019/11/03/Y5KpiVlrajkR6UI.jpg)

1，2，3都默认好了，中文的，
4的话，我选择了gnome桌面，毕竟方便看一些
![](https://i.loli.net/2019/11/03/WqYJdy4cfPZHoUC.jpg)

![](https://i.loli.net/2019/11/03/g4wrv72SnjtHVXa.jpg)

5的话，我选择的自动分区，比较懒。。。
![](https://i.loli.net/2019/11/03/S51TngPbpxsLlwV.jpg)

6这里的话，选择一下wifi或者有线
![](https://i.loli.net/2019/11/03/OjuQm7grcZsSbil.jpg)

开始安装吧
然后下一步设置一下密码就ok了。最好别用大写，有时间会坑，因为这个切换大小写忘了之后就完了。我就是找了好久发现一个本来应该小写的结果大写了，密码都搞错了，后来修改了。
![](https://i.loli.net/2019/11/03/Zmzl1kPWEpL7CIs.jpg)

记得！！！拔掉u盘，然后就ok了。

## 开始ssh远程连接
默认已经安装了ssh，如果是dvd版本的话
步骤如下：
第一步

###### 查看本机是否安装SSH软件包
[root@localhost ~]# rpm -qa | grep ssh
openssh-server-6.6.1p1-12.el7_1.x86_64
openssh-clients-6.6.1p1-12.el7_1.x86_64
libssh2-1.4.3-8.el7.x86_64
openssh-6.6.1p1-12.el7_1.x86_64

#####如果没有，则需要安装
[root@localhost /]# yum install openssh-server
第二步
#####开启 SSH 服务
[root@localhost ~]# service sshd start
Redirecting to /bin/systemctl start  sshd.service

#####查看TCP 22端口是否打开
[root@localhost ~]# netstat -ntpl | grep 22
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      17816/sshd          
tcp6       0      0 :::22                   :::*                    LISTEN      17816/sshd
第三步

#####接下来便可使用终端仿真程序（例如putty）去登陆远程主机
如果你在客户端不能连接SSH服务的话，那可能是防火墙的原因，终端命令行中输入 iptables -nL 来看是否开放了ssh tcp 22 端口：
[root@localhost ~]# iptables -nL
你可以将防火墙中的规则条目清除掉：
[root@localhost ~]# iptables -F

##### 客户端使用
ssh username@192.168.2.194 
或者mac本使用secureCrt

## 问题
碰到了刚开始重启不成功，一直显示watchdog watchdog0: watchdog did not stop!
然后更新了系统，然后重启还是有这个日志，不过会重启成功了。不行就直接按电源！

## centos7 笔记本关系合盖睡眠
由于是笔记本，经常需要关闭盖子，但是不能让他休眠！
参考 https://blog.csdn.net/qq_34911465/article/details/70246193

```
动作包括：
HandlePowerKey：按下电源键后的动作
HandleSleepKey：按下挂起键后的动作
HandleHibernateKey: 按下休眠键后的动作
HandleLidSwitch：合上笔记本盖后待机

这些动作的值可以是
ignore（什么都不做）
poweroff（关机）
reboot（重新启动）
halt（关机，和poweroff有什么区别，需要手动断开电源？）
suspend（待机挂起）
hibernate（休眠）
默认情况是，当我合上笔记本屏幕的时候，系统会待机。
如果我不想让系统在我合上笔记本的时候待机，怎么办呢？

用vi编辑器打开 /etc/systemd/logind.conf


去掉HandleLidSwitch前面的注释符号#，并把它的值从suspend修改为ignore。
[Login]
#NAutoVTs=6
#ReserveVT=6
#KillUserProcesses=no
#KillOnlyUsers=
#KillExcludeUsers=root
#InhibitDelayMaxSec=5
#HandlePowerKey=poweroff
#HandleSuspendKey=suspend
#HandleHibernateKey=hibernate
HandleLidSwitch=ignore
#HandleLidSwitchDocked=ignore
#PowerKeyIgnoreInhibited=no
#SuspendKeyIgnoreInhibited=no
#HibernateKeyIgnoreInhibited=no
#LidSwitchIgnoreInhibited=yes
#IdleAction=ignore
#IdleActionSec=30min
#RuntimeDirectorySize=10%
#RemoveIPC=no
~         
```
然后systemctl restart systemd-logind，使更改生效。再合上笔记本盖子，也不会待机了。
