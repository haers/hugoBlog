---
title: 解决图床问题-坑爹的“免费”七牛云
date: 2018-10-26 10:21:16
categories: [工具]
tags: [图床,新浪图床]
---

## 背景

原来按照好多教程，使用了七牛云的图床，不错，但是正因为是免费的，域名失效了！！！然后图片全都不行了，只能寻找新的图床，历经挫折。下面将我的解决方案分享出来；

<!--more-->

![](https://i.loli.net/2019/11/03/n5HS9BTsqkX37gi.jpg)

亏我当时还起的名字我爱七牛

![](https://i.loli.net/2019/11/03/BhMmWgEPfRcdVFk.jpg)

我这没有beian的域名绑定这个干啥，很麻烦。。。图片还能访问（不知道是否因为cdn的关系），就是上传不了。 

环境：

* mac
* mweb

## 解决方案

### 新浪图床

本来找了一个github的代码，自己本地起个服务，但是，后来坐着没更新了，npm编译失败，版本的问题，js又不太熟悉，只能另寻办法，php代码不少，但是也看不懂，最后终于用关键词在github上找了一个java代码，2018版本的，然后试了一下ok【源码的方案，我会改进的，由于新浪没有公开提供api，都是抓取的】。不过后来又找了一下，有了新的解决方案；

![](https://i.loli.net/2019/11/03/ThqYPWS5MpIaRZe.jpg)

使用ipic软件，下载下来之后，安装mover插件，不付费版本只有新浪图床，够用了，其他的图床有别的解决方案（因为都api都开发，很好写）

使用ipicMover扫描之后，可以将我这七牛的迁移过来

![-w681](https://i.loli.net/2019/11/03/O2aQly4fPn7TvsB.jpg)

因此，对于我来说，在mweb写完博客之后，使用mweb上传到新浪（可以使用代码），然后copy包含新浪地址的md到hexo中（因为自己都是保留原图到Dropbox中的）。

![-w747](https://i.loli.net/2019/11/03/Cdkb9ItligU6G4o.jpg)

然后直接提交代码就ok了。（上图就是替换七牛的地址）
    
如果没有本地新浪api上传的话，可以copy这个文章，然后使用ipic扫描，这样的话就会替换地址，然后copy到hexo中就ok了。

### 第三方图床

网上很多

## 如何选择图床

因为自己的服务器是github，因此尽量内网和外网都能访问

[评测网址](http://bangumi.tv/group/topic/343056)

这个网址是作者评测，就是没有小电视的国内访问不了。所以，想自己定义api的可以使用这些图床

![](https://i.loli.net/2019/11/03/bJ4xfYQojutZeGD.jpg)

像上图好多都是免费的，极简图床，api是收费的。如果懂代码的话，建议自己开发一下。不算难。
    
## 好用的一些网址，以及自己怎么使用这些图床

catbox.moe 这个自己实现了文件上传，最笨的方法，就是Chrome开启开发组模式，然后使用postman模拟，然后copypostman的code（postman是可以根据语言生成代码的），后来发现对方开放了api；

![](https://i.loli.net/2019/11/03/eWDcsxtCkwjq5TI.jpg)

## 参考的网址
* [里面有Windows的和ipic类似的工具](https://sspai.com/post/40499)
* [Python的登录微博参考，不过自己对Python不熟悉](http://www.cnblogs.com/fengwbetter/fengwbetter/p/9107742.html)

* [php版本，使用session上传图片的
](https://cloud.tencent.com/developer/article/1152353)
* [新浪图床java代码](https://github.com/xx13295/sina-picbed)
* [新浪图床Chrome扩展](https://github.com/Suxiaogang/WeiboPicBed)
* [v站的图床推荐](https://www.v2ex.com/t/473771)
* [七牛图床域名过期的问题](https://github.com/qiniu/qshell/issues/188)
* [js的新浪图床，有大佬懂js的可以改改支持一下现在最新的npm啥的](https://invisprints.wordpress.com/2017/12/10/uploadpicinweibo/)



## 常见问题

* 如果使用自己的微博账号，微博上传失败的话，需要关闭微博地址验证；
* 如果只是简单的一两张图片上传，可以使用Chrome的扩展，搜索图床，有不少微博的插件。
* 图床想找总能找到的，就是麻烦，不行就自己搞个服务器。

## 结论

自己使用上面的java代码，本地启动服务，然后使用mweb上传到图床服务器，copy代码，在hexo中新建博客。然后commit，部署，自动继承编译就ok了。

已经开放源码和第一版

[源码地址](https://github.com/RMzcq/dddjava)

下载源码自己编译，或者从如下地址下载：

[下载地址](https://u3492574.ctfile.com/fs/3492574-319064523)

* 首先配置bootstrap.yml中的新浪用户名密码，记得关闭新浪双重验证
* 运行jar包 java -jar jar名称
* 配置地址 http://127.0.0.1:8089/file/uploadFileToSina
  参数为file 类型为file，可以使用postman或者mweb测试一下。
  
然后就能使用啦！如果使用mweb的话，返回的参数取data，这个就是上传后的url路径
