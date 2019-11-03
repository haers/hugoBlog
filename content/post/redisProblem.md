---
title: redis在springboot 2版本下的使用的问题
date: 2019-01-21 20:14:05
urlname: redisProblem
categories: [redis]
tags: [redis,spring boot, spring session]
---

今天起的新项目使用spring session redis。
抛出了几个问题，问题异常不贴了
<!--more-->
# 初始化redis 工厂实例失败

异常如下：

```
Consider revisiting the conditions above or defining a bean of type 'org.springframework.data.redis.connection.jedis.JedisConnectionFactory' in your configuration.

```
自己使用了 

```
<dependency>
          <groupId>org.springframework.session</groupId>
          <artifactId>spring-session-data-redis</artifactId>
           <version>2.1.2.RELEASE</version>
</dependency>
```

只添加这一个是不行的，因为我们如果打开pom文件会发现，虽然引用了 spring-data-redis ，但是其中的两个driver 

![500f7dd1d623e0017c4044689ebe9925](https://i.loli.net/2019/11/03/uNI1l75VpcYzbyk.jpg)

都是option的，可选择的，所以在主要的项目中得指定一个redis客户端。
从2.0开始，spring 默认使用的 lettuce 客户端，我们可以添加上他就ok了。

![0ab3127c8689cbf40542219fd12643a8](https://i.loli.net/2019/11/03/Fn5WTOtGZMSmwIR.jpg)

如图所示，commons包也是option的，所以也得添加。
如果想使用jedis客户端，那么就直接引用jedis客户端就行了，去掉截图中的两个引用。

ps：
顺便再提及一点，如果同时引用了lettuce，jedis两个jar包的话，那么就会产生jedis初始化失败的问题，这是因为，源码中 LettuceConnectionConfiguration 初始化比redis的工厂类早。

# 如果使用lettuce碰到连接超时的问题，RedisCommandTimeoutException
那么将截图中的timeout配置项去掉就可以了。

![28145be2d53fc6d26afe3d705e163369](https://i.loli.net/2019/11/03/K7nqGLWjU5aQO1S.jpg)

# 说明一下
最近比较忙，写了不少笔记，总结了很多东西，不少干活。近一年会主键开发。
起了个新项目，今年开源，用到了不少东西，对新手比较友好。

