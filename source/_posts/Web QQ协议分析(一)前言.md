---
title: Web QQ协议分析(一)前言
tags:
  - WebQQ协议
  - QQ机器人
  - PHP
id: 126
categories:
  - Python
  - QQRobot
  - SmartQQ
  - 后端
date: 2016-08-04 17:44:57
---

## 写在前面的话

首先感谢[ScienJus ](http://www.scienjus.com)的梳理，使我在爬的时候少走了很多弯路。他的博客上介绍的是的介绍是ruby的语法。并且在他的`github`上挂出的是[java](https://github.com/ScienJus/qqbot%20JavaSmartQQ)和[ruby](https://github.com/ScienJus/smartqq%20RubySmartQQ)版本的接口。但是自认为`java`才是刚开始起步，`ruby`更是不会了。所以我没有办法直接使用前辈提供的接口，索性我就用最近新学的`Python`写了个自己的模拟登陆的接口。

<!--more-->

说些题外话，总觉得自己是一个写`bug`的人。可能是跟自己在学习的时候基础不够扎实有关系。即使跟别人走同样的道路却能做出不同的效果来。所以我也在这期间走了许多的弯路。不过这些弯路可以说并不白走。至少在这期间巩固了我的基本功。可以说我的基本功都是从找`bug`中学来的。

其实在决定用`Python`写接口之前用了好几种语言去尝试。其实有我最擅长的`php`，也有正在学习的`java`，还有我最终确定下来的`Python`。至于为什么最终选择了`Python`，那就说说我各自使用后的感受吧。

### php

说起`php`，这应该是我最熟悉的一门语言了，我兴致满满的准备用他去完成我的QQRobot的计划。当时使用的是Snoopy爬虫类。之前觉得这个爬虫类挺好用的，简单方便强大。直到我用他来爬QQ后我发现，他仍有自己无法处理的事情。

> 首先他无法保持长会话，这是一个很让我头疼的问题。因为QQ的鉴权真的是做到了极致，不仅需要各种请求获取参数，还要提交每次返回的cookies。这就使无法保持长会话的Snoopy很难过。每次提交cookies的时候都需要正则获取之前返回的cookies。然后在提交。这增加了很多的代码量。
> 
> 其次有些参数并不是很好获取。需要请求多个页面方可获取。但是Snoopy的超时时间并不给力。如果稍微遇到网络波动就会超时请求失败。虽然官方提供修改超时时间，但亲测无用。不知为何。
>   最后用于实在处理cookies不方便，我放弃了`php` 转站`Python`

### Python

说道`Python` 我不得不说，不愧是专门的模拟登陆的语言，胶水语言。爬起东西来真心的好用。不仅解决了`php` 的不足，而且`Python` 的宗旨就是减少代码量。而且支持多线程。嗯，虽然我现在也是才接触多线程这个东西。

> 不过`Python` 有一点让我很头疼的就是他的各种类型之间的转换，各种格式之间的区别。`Python` 自带的 `list`  字典等都是别的语言中所没有的。但是却和其余语言的类型相似。如何处理好之间的关系个人认为是个难题。

### JAVA

一句话

> 有点高深，一时半会弄不熟。所以放弃了。

虽然说这`java`和 `C++` `php` 都挺像的，但是一只没有真正的好好去琢磨面向对象编程。虽然名义上用了些面向对象的东西，但都是些皮毛。所以面对着前辈写的代码我就有点蒙蔽了，看得懂，但却无法更改。所以我果断放弃了`java` 。容我日后慢慢研究。