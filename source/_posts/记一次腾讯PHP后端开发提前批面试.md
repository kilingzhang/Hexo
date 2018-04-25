---
title: 记一次腾讯PHP后端开发提前批面试
date: 2018-04-10 20:11:31
tags:
---





![](http://markdown-1252847423.cos.ap-beijing-1.myqcloud.com/20180411012304743/20180411012351371.png)



<!--more-->



## [PHP底层结构 运行机理](https://blog.csdn.net/lili0710432/article/details/47816365)

1. PHP的设计理念及特点
    - 多进程模型：由于PHP是多进程模型，不同请求间互不干涉，这样保证了一个请求挂掉不会对全盘服务造成影响，当然，时代发展，PHP也早已支持多线程模型。
    1. 弱类型语言：和C/C++、Java、C#等语言不同，PHP是一门弱类型语言。一个变量的类型并不是一开始就确定不变，运行中才会确定并可能发生隐式或显式的类型转换，这种机制的灵活性在web开发中非常方便、高效。
    2. 引擎(Zend)+组件(ext)的模式，降低内部耦合。
    3. 中间层(sapi)，隔绝web server和PHP。
    4. 语法简单灵活，没有太多规范。
2. PHP的核心架构
![](http://markdown-1252847423.cos.ap-beijing-1.myqcloud.com/20180411012304743/20180411012351371.png)
    1. Zend引擎：纯C实现，是PHP的内核部分，它将PHP代码翻译（词法、语法解析等一系列编译过程）为可执行opcode的处理并实现相应的处理方法、实现了基本的数据结构（如hashtable、oo）、内存分配及管理、提供了相应的api方法供外部调用，是一切的核心，所有的外围功能均围绕Zend实现。
    2. Extensions：围绕着Zend引擎，extensions通过组件式的方式提供各种基础服务，我们常见的各种内置函数（如array 系列）、标准库等都是通过extension来实现。
    3. Sapi :全称是Server Application Programming Interface服务端应用编程接口，Sapi通过一系列钩子函数，使得PHP可以和外围交互数据，这是PHP非常优雅和成功的一个设计，通过 sapi成功的将PHP本身和上层应用解耦隔离，PHP可以不再考虑如何针对不同应用进行兼容，而应用本身也可以针对自己的特点实现不同的处理方式。 
常见的一些sapi有： 
apache2handler：这是以apache作为webserver，采用mod_PHP模式运行时候的处理方式，也是现在应用最广泛的一种。 
cgi：这是webserver和PHP直接的另一种交互方式，也就是大名鼎鼎的fastcgi协议，在最近今年fastcgi+PHP得到越来越多的应用，也是异步webserver所唯一支持的方式。 
cli：命令行调用的应用模式
    4. 上层应用：这就是我们平时编写的PHP程序，通过不同的sapi方式得到各种各样的应用模式，如通过webserver实现web应用、在命令行下以脚本方式运行等。


        struct _zval_struct {
            zvalue_value value;     /* value */
            zend_uint refcount__gc;  /* variable ref count */
            zend_uchar type;          /* active type */
            zend_uchar is_ref__gc;    /* if it is a ref variable */
        };
        typedef struct _zval_struct zval;
        
        typedef union _zvalue_value {
            long lval;                  /* long value */
            double dval;                /* double value */
            struct {                    /* string */
                char *val;
                int len;
            } str;
            HashTable *ht;              /* hash table value,used for array */
            zend_object_value obj;      /* object */
        } zvalue_value;

## [浏览器输入网址到返回数据 都经历了什么](https://www.jianshu.com/p/29936050b6a1)
1. [what-really-happens-when-you-navigate-to-a-url](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)
2. [细说浏览器输入URL后发生了什么](https://zhuanlan.zhihu.com/p/31311964)
## static、public、private、protected、final 区别　([PHP面向对象](http://php.net/manual/zh/language.oop5.php))
1. final
    - 属性不能被定义为 final，只有类和方法才能被定义为 final。
    - 如果父类中的方法被声明为 final，则子类无法覆盖该方法。如果一个类被声明为 final，则不能被继承。
2. static
    - 声明类属性或方法为静态，就可以不实例化类而直接访问。静态属性不能通过一个类已实例化的对象来访问（但静态方法可以）。
    - 由于静态方法不需要通过对象即可调用，所以伪变量 ```$this``` 在静态方法中不可用。
    - 静态属性不可以由对象通过 ```->``` 操作符来访问。
    - 静态属性只能被初始化为文字或常量，不能使用表达式。所以可以把静态属性初始化为整数或数组，但不能初始化为另一个变量或函数返回值，也不能指向一个对象。
    - 变量范围的另一个重要特性是静态变量（static variable）。静态变量仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不丢失。
    - 静态声明是在编译时解析的。
    - 可以用一个变量来动态调用类。但该变量的值不能为关键字 self，parent 或 static。
    - 用静态方式调用一个非静态方法会导致一个 ```E_STRICT``` 级别的错误。
    - 后期静态绑定 self::、parent::、static:: （[拓展](http://php.net/manual/zh/language.oop5.late-static-bindings.php)）  
3. ```public（公有）```，```protected（受保护）```或 ```private（私有）``` 来实现的。被定义为公有的类成员可以在任何地方被访问。被定义为受保护的类成员则可以被其自身以及其子类和父类访问。被定义为私有的类成员则只能被其定义所在的类访问。
4. 同一个类的对象即使不是同一个实例也可以互相访问对方的私有与受保护成员。这是由于在这些对象的内部具体实现的细节都是已知的。
## [GET、POST、PUT、HEAD、DELETE 之间的区别](http://www.cnblogs.com/machao/p/5788425.html)
[Http:Get、Post、Put、Delete、Head、Options详解](https://my.oschina.net/Clarences/blog/1529241)


## [面向对象](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E6%80%9D%E6%83%B3.md)

## [写出用过的设计模式及应用场景](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/设计模式.md)
### 应用场景

## 常见web攻击及防范措施
[Web 安全入门之常见攻击](https://zhuanlan.zhihu.com/p/23309154)
[常见web攻击及防范措施](http://www.hollischuang.com/archives/2101)

## 如何优化网页访问速度(高并发/流量优化)

### 前端优化

- 减少请求数

  1. 图片地图
  2. 雪碧图
  3. css,js文件合并
  4. 图片使用Base64编码

- 异步请求数据

- 启用浏览器缓存和文件压缩

  - HTTP缓存机制
    1. 200 from cache (直接从本地请求缓存)
    2. 304 Not Modifued (协商缓存,返回基本响应头信息)
       1. Last-Modified 通知浏览器资源的最后修改时间
       2. ETag 文件指纹标识符(文件改变标识符改变)
    3. 200 OK (没有任何缓存)
    4. Pragma > Cache-Control > Expires
  - Nginx配置缓存策略
  - 前端代码和资源的压缩
    1. Gzip
    2. 工具压缩代码
    3. tinyPNG 等 压缩图片

- CDN加速 (Content Delivery Network 内容分发网络)

  - 优势

    1. 本地Cache加速
    2. 跨运营商的网络加速
    3. 可生成远程镜像,减少访问的带宽,分担网络流量,减轻真实服务器的负载
    4. 广泛的cnd节点智能调控,防范黑客攻击

    - 原理

      > 用户发起请求 -> 智能dns解析(根据ip判断地理位置,接入类型,路由最短,负载最轻) -> 取得缓存服务器ip -> 把缓存内容给用户 -> 源站点取内容 -> 返回用户 -> 结果缓存

    - 场景
      1. 静态资源的加速分发
      2. 大文件下载
      3. 直播网站
    - 实现
      1. LVS四层负载均衡
      2. nginx,Varnish,Squid,Apache 等反向代理 七层负载均衡和Cache

- 建立独立图片服务器,对象储存服务

### 服务端优化

- 页面静态化
  - Smarty
  - ob 
        ob_start();
        ob_get_contents();
        ob_end_flush();
        filectime();
        put_contents_file();
- 并发处理(队列异步处理)
  - 进程,线程,协成
  - 多线程,多进程
  - 同步阻塞模型
  - 异步非阻塞模型
  - PHP并发编程实践
    - 接口并发请求 curl_multi_init()
  - swoole 扩展
  - 队列
    - activeMQ Redis

### 数据库缓存

1. memcache 
2. Redis
3. 区别  
   - 性能区别不是特别大
   - Redis持久化数据保存 
   - Redis 支持复杂数据结构 
   - Redis2.0后  增加 VM 特性  突破物理内存限制
   - memcache 修改最大可用内存 LRU 算法
   - Redis 依赖客户端实现分布式读写
   - Memcache 本身没有数据冗余机制
   - Redis 支持 (快照,AOF) ,依赖快照进行持久化,AOF增强了可靠性同时,对性能有所影响. 

### 数据库优化

- 数据表数据类型优化

  1. 字段用什么样的数据类型更合适
  2. 字段用什么样的数据类型性能更快

  - 索引优化

  1. 索引并不是越多越好,合适的字段创建合适的索引.索引会影响写操作.
  2. 复合索引的前缀原则 (a,b,c)
  3. like 查询 % 的问题
  4. 全表扫描的优化
  5. ​

- SQL语句优化

  1. 优化查询过程中的数据访问
     1. limit
     2. 尽量不要使用 * 
  2. 优化长难句的查询
     1. 变复杂为简单
     2. 切分查询   
     3. 分解关联查询
  3. 优化特定类型的查询语句
     1. 优化count()  count(*) 最快
     2. 优化关联查询
     3. 优化子查询
     4. 优化Group by 和 distinct
     5. 优化 limit 和  union

- 存储引擎优化

  1. 尽量使用 innoDB 
     1. 支持实务
     2. 支持行级锁(不是表锁)
     3. 支持外建
     4. 独立表空间

- 数据表结构设计优化

  1. 分区操作
     1. 通过特定的策略对数据表进行物理拆分
        - 地区分区
        - 热点分区
        - 对用户透明
        - partition by
  2. 分库分表
     1. 水平拆分 (行级拆分 union 合并)
     2. 垂直拆分 (列级拆分 jion in 合并)

- 数据库服务器架构的优化

  1. 主从复制
  2. 读写分离
  3. 双主热备
  4. 负载均衡

### web服务器优化

- 负载均衡

  1. 七层负载均衡实现 (nginx)

     - nginx 负载均衡策略

       1. 内置策略 扩展策略
       2. 内置策略 IP Hash 加权轮询
       3. 扩展策略 fair策略 通用hash 一致性hash

     - 加权轮询

       > 根绝权值高低一次轮训

     - IP Hash 

     - fair策略

       > 根绝响应时间判断负载情况,选出负载最轻的服务器

     - 通用hash

     - 一致性hash

  2. 四层负载均衡实现 (lvs)


## [引用和传值的区别](http://www.cnblogs.com/xiaochaohuashengmi/archive/2011/09/10/2173092.html)

## [原生 ajax 兼容ie](https://blog.csdn.net/u012643122/article/details/79688215)

## 正则匹配邮箱
[正则](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F.md)
[常见正则](http://www.cnblogs.com/zxin/archive/2013/01/26/2877765.html)
[在线正则](https://www.regexpal.com)

    ^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$

## [js 闭包及其应用](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)（好多个公司都喜欢问）

## TCP三次挥手四次握手

[通俗大白话来理解TCP协议的三次握手和四次分手](https://github.com/jawil/blog/issues/14)

## PHP常见函数　（count）

***看手册***

## 抽象类和接口的区别
[抽象类和接口的区别](https://www.jianshu.com/p/038f0b356e9a)
1. 使用接口（interface），可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。
2. 接口是通过 interface 关键字来定义的，就像定义一个标准的类一样，但其中定义所有的方法都是空的。
3. 接口中定义的所有方法都必须是公有，这是接口的特性。
4. 实现多个接口时，接口中的方法不能有重名。
5. 接口也可以继承，通过使用 extends 操作符。
6. 类要实现接口，必须使用和接口中所定义的方法完全一致的方式。否则会导致致命错误。
7. 定义为抽象的类不能被实例化。任何一个类，如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。被定义为抽象的方法只是声明了其调用方式（参数），不能定义其具体的功能实现。

## 协程的特点在于是一个线程执行，那和多线程比，协程有何优势？

1. 最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。
2. 第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。
3. 因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。


## 介绍自己

## 说说自己最得意的项目 技术点 技术难点

## 怎么理解后端，后端应该掌握什么


