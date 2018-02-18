---
title: PHP知识大纲
tags:
  - PHP
categories:
  - 面试
  - 面试题   
date: 2017-12-20 17:44:57
---

# 准备知识点    

>知识点复习大纲,持续更新 

<!--more-->

## 版本控制软件

- 集中式
	- CVS
	- SVN
- 分布式
  - Git
  
[从零开始学git.pdf](http://markdown-1252847423.file.myqcloud.com/learn-github-from-zero.pdf)


## PHP     

### 运行原理

#### nginx

[Nginx工作原理和优化](https://www.cnblogs.com/linguoguo/p/5511293.html)

#### PHP

> [nginx与PHP通信以及PHP-FPM工作原理
](https://www.futaovip.vip/archives/122)

- `CGI`
> 不同语言解析器与webserver的通信桥梁.一种通信协议. 一个进程处理一个请求.

- `fastCGI`
> `CGI` 的改良版本,一个进程处理多个请求.

- `PHP-FPM`
> `fastCGI` 进程管理器. 其中进程包括 `master` 进程与 `worker` 进程. `master` 进程只有一个,负责监听端口,监听来自webserver的请求.默认端口为 `9000`  . `worker` 进程可以多个,在fpm配置中设置.

- PHP常见配置项

![PHP常见配置项](http://markdown-1252847423.file.myqcloud.com/PHP%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE.png)



## 会话机制

垃圾回收机制： 

    session.gc_probability = 1
    session.gc_divisor = 100
    session.gc_maxlifetime = 1440
    
每100次调用session_start一次清除时间超过1440秒的session文件。

Session文件储存位置（分布式储存）
session_handler   session文件存储句柄位置
session_set_save_handler();
Mysql  Radis  memacahe
http://blog.csdn.net/u013707844/article/details/26475265



## 网络协议

![网络协议考点](http://markdown-1252847423.file.myqcloud.com/%E7%BD%91%E7%BB%9C%E5%8D%8F%E8%AE%AE%E8%80%83%E7%82%B9.png)

### HTTP协议

[HTTP协议](http://www.runoob.com/http/http-tutorial.html)

### HTTP协议状态码
![](http://markdown-1252847423.file.myqcloud.com/HTTP%E5%8D%8F%E8%AE%AE%E7%8A%B6%E6%80%81%E7%A0%81.png)
[HTTP协议状态码](http://www.runoob.com/http/http-status-codes.html)

### HTTP 响应头信息

![响应头信息](http://markdown-1252847423.file.myqcloud.com/%E8%AF%B7%E6%B1%82%E5%A4%B4%E4%BF%A1%E6%81%AF.png)

[HTTP 响应头信息](https://www.jianshu.com/p/6e86903d74f7)

### OSI七层模型

- 物理层
- 数据链路层
- 网络层
- 传输层 (TCP/UDP)
- 会话层
- 表示层
- 应用层

### 常见网络协议含义及端口

### HTTPS协议的工作原理

## 面向对象

1. 类名与方法名相同时，此方法也是此类的构造方法。
2. parent::function() 调用父类的方法  重写父类方法进行拓展
3. PHP单一继承
4. 面向对象的多态
5. 抽象类的定义
6. 接口的定义
7. 魔术方法
![魔术方法](http://markdown-1252847423.file.myqcloud.com/PHP%E9%AD%94%E6%9C%AF%E6%96%B9%E6%B3%95.png)
8. 设计模式    
https://www.imooc.com/video/4848
9. 单一职责：一个类只需要做好一件事情。
10. 开放封闭：一个类，应该是可扩展的，而不可修改的。
11. 依赖倒置：一个类，不应该强依赖另外一个类。每个类对于另外一个类都是可替换的。
12. 配置化：尽可能地使用配置，而不是硬编码。
13. 面向接口编程：只需要关心接口，不需要关心实现。

## 正则




## Mysql
- MySQL数据库基础考察点
- MySQL创建高性能的索引考察点
- MySQL的SQL语句编写考察点
- MySQL的查询优化考察点
- MySQL的高可扩展和高可用考察点
- MySQL的安全性考察点
## Linux

## Nosql
### Redis
- 特性
	1. 速度快
	2. 持久化
	3. 多种数据结构
		1. 字符串
		2. hash Tables
		3. Linked lits (列表)
		3. sets (集合)
		4. Sorted sets (有序集合)
		3. bitmaps(位图)
		4. HyperLogLog(超小内存唯一值技术)
		5. GEO(地理位置定位)
	4. 支持多种客户端语言 (java php python)
	5. 功能丰富
		1. 发布订阅
		2. lua脚本
		3. 简单事务
	6. 简单
		1. 不依赖外部库
		2. 单线程
			- 单线程为啥还能变快
              - 纯内存
              - 非阻塞IO
              - 避免线程切换和竟态消耗
		3. 代码量精简
	7. 高可用,分布式(Redis3.0正式支持)
- 使用场景
	1. 缓存系统
	2. 计数器
	3. 发布订阅
	3. 消息队列系统
	4. 排行榜
	5. 社交网络
	6. 实时系统(垃圾邮件处理,消息队列缓冲)
- 命令
	- pexpire
	- persist
	- ttl
	- get
	- set (无论存不存在)
	- exists
	- setnx (不存在添加)
	- set key value xx (存在更新)
	- mset
	- mget
	- getset (更新值返回旧值)
	- append (讲字符串追加到旧值当中返回新值)
	- strlen (注意中文字符串长度)
	- del
	- incr 
	- decr
	- incrby 
	- decrby 
	- incrbyfloat 
	- getrange key start end
	- setrange key start end
	- hset
    - hget
    - hdel
    - hgetall
    - hexists
    - hlen
    - hvals
    - jkeys
	
- 优点
	- 集群伸缩
		- 原理
		> 原理:
		- 集群扩容
		- 集群缩容
- 注意
	1. 一次执行一条命令
	2. 拒绝长命令
	3. 其并不完全是单线程
- 客户端路由
	- moved重定向
	- ask重定向
	- smart客户端
		
## 高并发

### 流量优化
- 防盗链处理
	1. Referer
		> ngx_http_referer_module 模块
		> valid_referers 指令配置允许的域名 				none | blocked | server_names | 正则  
		> 全局变量 $invalid_referer 判断是否不合法 
	2. 签名
		> httpAccessKeyModule 模块
		> accesskey on|off 
		> accesskey_hashmethod md5 | sha-1
		> accesskey_arg GET参数名称键值
		> accesskey_signature 加密规则

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
  -　索引优化
  1. 索引并不是越多越好,合适的字段创建合适的索引.索引会影响写操作.
  2. 复合索引的前缀原则 (a,b,c)
  3. like 查询 % 的问题
  4. 全表扫描的优化
  5.  
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


## 数据结构与算法




# 项目整理总结




