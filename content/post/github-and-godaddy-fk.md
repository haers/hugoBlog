---
title: github 关联 godaddy 域名碰到的坑
top: 100
date: 2018-09-24 22:43:35
urlname: github-and-godaddy-fk
categories: [工具]
tags: [github,godaddy]
---

故事起因，自己有一台服务器，原来博客是部署在上面的，这次打算公开并且开始真正的写博客了，想不公开服务器的ip，因为上面部署了好多东西，你懂的。所以后来就想还是用github吧，然后域名解析给我坑死了，下面说一下步骤吧，有问题可以联系我。

<!--more-->

## github 配置

我的项目已经部署在blog仓库中了，因此直接新建一个blog项目就ok了，网上好多教程，以前自己建立了io的仓库，这次拿来直接用，但是需要添加一个 CNAME 文件，自己添加了，但是每次hexo一编译就没了，后来查询了一下，放到source目录下就ok了（原来放在根目录下了），起初配置的 dddjava.com，网上好多教程都是这么说的，后来用了第二版，等会说。

## 我的域名是godaddy的，这个网站吧，坑

里面注释写的不好，所以我一直搞不懂 a记录和chame的区别，区别看后面，这里先说教程。

再说一点，好多教程用的第三方解析的，没有必要，出发国内外两套环境，可以用第三方解析

* 一条a记录，配置成服务器的ip地址，这个ip地址，需要自己ping自己的github的博客地址得到，不要随便抄袭别人的教程上的，没用的，因为这东西老变。
* 一条CNAME记录，配置的自己的	xxx.github.io
* 然后剩下的ns记录不动，我其余的都删除了

![-w1113](https://i.loli.net/2019/11/03/opAyZEibnXB1lJK.jpg)

然后等了一会，我也不知道多久吧，今天给这个搞的恼火，吃完月饼，躺了一会，起来就想通了。

## 验证

使用验证的时候发现，每次都是访问 www.dddjava.com 跳转到 dddjava.com 了，我一想，这不行啊，我原来的访问记录全丢失了啊（就是访问博客的人流量，其实没啥用，还有一点就是瞅着不喜欢），然后终于让我找到了教程，感谢大佬

[GitHub Pages 绑定二级域名](https://segmentfault.com/a/1190000005775893)

如何绑定二级域名，我就不献丑了，可以参考，感谢大佬的文章

## github的chame的作用，可以看这个文章

[个人博客记 —— Github pages 绑定个人域名](https://segmentfault.com/a/1190000011203711)

核心思想就是，服务器得有域名的配置，域名得有服务器的配置，要不怎么说github强大呢，啥都干！

## dns解析，a记录和chame的区别

![](https://i.loli.net/2019/11/03/GHBVn7KtJI4YPbM.jpg)

![](https://i.loli.net/2019/11/03/jQRCPiDxeXrlpha.jpg)

这两张图片用的别人的（侵权删，看看人家tx，都给你写的明明白白，看go啥的没写）

## 发现的好用的工具

![-w715](https://i.loli.net/2019/11/03/nbavPeGzxp831SI.jpg)

在验证的过程中吧，为了验证自己的猜测，我就去抓别人博客的ip地址，进行验证，然后就知道他们也是挂载到github了，然后再去github参考一下，就明白了。

[搜索iP地址的地理位置](http://www.ip138.com)

## 参考文章

* [Managing a custom domain for your GitHub Pages site](https://help.github.com/articles/setting-up-an-apex-domain/)
* [个人博客记 —— Github pages 绑定个人域名](https://segmentfault.com/a/1190000011203711)
* [从DNS到github pages自定义域名 -- 漫谈域名那些事](http://winterttr.me/2015/10/23/from-dns-to-github-custom-domain/)  这篇文章，讲解的不错，就是没写完整的方案


