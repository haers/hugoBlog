---
title: concurrent包解析，翻译
date: 2018-10-18 22:45:50
urlname: concurrentTranslate
categories: [concurrency]
tags: [并发,concurrency]
---
版本记录
### 刚开始第一版使用了ps注解翻译者（我）的一些疑问，标记
### 第二版使用mark这个标记，为了更好的区分

我本人比较笨，学并发也比较恼火，想了一下，还是翻译一下吧，欢迎大家指正，监督。
邮箱：jingtingchangqing@163.com 
jdk版本:1.8
工具:idea
<!--more-->
翻译:google，我还是会尽量按照原文翻译，不会打乱顺序，挺喜欢倒装句的，有阅读能力的建议看原文，真的很赏心悦目。翻译的东西，原作版权归原作者所有。其他版权详情见文章末尾。
## 结构
基于类的注解翻译
基于各个方法的翻译
产生了很大的疑虑，就是怎么做这个东西，这个系列讲解的就是材料，而我们经常看到的实际上是工具，是如何使用它们，这样的话就会形成一系列规范。
现在想从这里了解一下。



## 准备工作
找到对应的concurrent包，单击右键，选择![](http://ws2.sinaimg.cn/large/006tNbRwly1fwkh0c5opmj30aw0hnabl.jpg)
下面的show diagram，然后生成了一个uml图
![](http://ws1.sinaimg.cn/large/006tNbRwly1fwkh0d60n8j31390nyaff.jpg)

有一个比较有毒的点，就是，内部的atomic和locks没有包含在里面，所以到时候单看，看的时候很清晰明了，比较大的几个“根”，executor，blockingqueue，delayed，future，concurrentmap。剩下的都比较单了，现在看起来就很清晰明了，是不。
因此第一次做一个系列，有不足之处见谅。

