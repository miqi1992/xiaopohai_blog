---
doc_type: weread-highlights-reviews
bookId: "29126296"
author: 翟陆续
cover: https://wfqqreader-1252317822.image.myqcloud.com/cover/296/29126296/t7_29126296.jpg
reviewCount: 0
noteCount: 1
readingStatus: 在读
progress: 10%
totalReadDay: 2
readingTime: 0小时43分钟
readingDate: 2023-05-11
isbn: 9787121376931
lastReadDate: 2023-05-11

---
# 元数据
> [!abstract] 深度剖析ApacheDubbo核心技术内幕
> - ![ 深度剖析ApacheDubbo核心技术内幕|200](https://wfqqreader-1252317822.image.myqcloud.com/cover/296/29126296/t7_29126296.jpg)
> - 书名： 深度剖析ApacheDubbo核心技术内幕
> - 作者： 翟陆续
> - 简介： Dubbo是阿里巴巴开发的一个开源的高性能、高可用、可扩展的分布式RPC调用框架，致力于提供高性能和透明化的 RPC 远程调用服务解决方案。作为阿里巴巴 SOA 服务化治理方案的核心框架，目前已进入 Apache 孵化器项目。在单体应用时，不同业务模块部署在同一个JVM 进程内，这时通过本地调用就可以解决不同业务模块之间的相互引用；但在多体应用时，不同业务模块大多部署到不同的机器上，这时一个高效、稳定的RPC框架就显得特别重要了。Apache Dubbo 作为阿里巴巴开源的分布式RPC 框架，在进入Apache 孵化器项目后现已毕业，相信在开源社区的不断贡献下，它会成为RPC 框架中的佼佼者。本书是对Apache Dubbo 的使用以及内核原理的深度剖析，分为三部分：第一部分为基础篇，首先从整体上讲解使用Dubbo 搭建的系统由哪些模块组成，各模块相互之间的调用关系是怎么样的，然后基于本书的Demo 讲解如何使用Dubbo ；第二部分为高级篇，主要讲解Dubbo 框架内部实现原理，包含支撑Dubbo框架的适配器类原理、动态编译原理、增强SPI 原理、消费端的泛化调用实现原理、消费端异步调用与服务提供端的异步执行、Dubbo 框架的线程模型、消费端负载均衡策略、消费端集群容错策略、并发控制原理、Dubbo 网络协议等；第三部分为实践篇，主要探讨如何使用Arthas 和一些Demo 为研究Dubbo 框架原理提供方便，并且讲解如何基于CompletableFuture 和Netty 模拟RPC 同步与纯异步调用。本书将原理与实践相结合，由浅入深、通俗易懂地讲解了Dubbo 框架的使用及内核原理实现，适合Java 中高级研发工程师，以及对RPC 框架技术感兴趣，希望探究RPC 框架内部实现原理的人员阅读。
> - 出版时间 2019-12-01 00:00:00
> - ISBN： 9787121376931
> - 分类： 计算机-编程设计
> - 出版社： 电子工业出版社
> - PC地址：https://weread.qq.com/web/reader/ba53238071bc6e98ba52203

# 高亮划线

## 封面

## 版权信息

## 序

## 前言

> 📌 Dubbo框架从整体上分为了业务（Business）层、RPC层和远程调用（Remoting）层，其中业务层提供API，让使用者方便地发布与引用服务；RPC层则是对服务注册与发现、服务代理、路由、负载均衡等功能的封装，该层又可以被划分为很多层；远程调用层则是对网络传输与请求数据序列/反序列化等的抽象。使用分层架构可以保证下层的改变对上层不可见，并且可以实现关注点分离，比如使用者使用Dubbo时只关心如何使用业务层的API来发布与引用服务，而不需要关心RPC层的实现，当新版本Dubbo升级了RPC层的逻辑时，使用者只需要升级Dubbo的版本就可以了，这是因为RPC层的修改对业务层使用者来说是透明的。 
> ⏱ 2023-05-11 11:19:42 ^29126296-4-1055-1357

## 基础篇

### 第1章 Dubbo基础

#### 1.2 本书Demo详解

#### 1.3 小结

## 高级篇

### 第2章 Dubbo框架内核原理剖析

#### 2.2 Dubbo远程调用细节

#### 2.3 Dubbo的适配器原理

#### 2.4 Dubbo的动态编译原理

#### 2.5 Dubbo增强SPI

#### 2.6 Dubbo使用JavaAssist减少反射调用开销

#### 2.7 小结

### 第3章 远程服务发布与引用流程剖析

#### 3.2 Dubbo服务提供方如何处理请求

#### 3.3 Dubbo服务消费方启动流程剖析

#### 3.4 Dubbo服务消费端一次远程调用过程

#### 3.5 小结

### 第4章 Directory目录与Router路由服务

#### 4.2 RegistryDirectory的创建

#### 4.3 RegistryDirectory中invoker列表的更新

#### 4.4 小结

### 第5章 Dubbo消费端服务mock与服务降级策略原理

#### 5.2 本地服务mock原理

#### 5.3 小结

### 第6章 Dubbo集群容错与负载均衡策略

#### 6.2 Failfast Cluster策略源码分析

#### 6.3 Failsafe Cluster策略源码分析

#### 6.4 Failover Cluster策略源码分析

#### 6.5 Failback Cluster策略源码分析

#### 6.6 Forking Cluster策略源码分析

#### 6.7 Broadcast Cluster策略源码分析

#### 6.8 如何基于扩展接口自定义集群容错策略

#### 6.9 Dubbo负载均衡策略概述

#### 6.10 Random LoadBalance策略源码分析

#### 6.11 RoundRobin LoadBalance策略源码分析

#### 6.12 LeastActive LoadBalance策略源码分析

#### 6.13 ConsistentHash LoadBalance策略源码分析

#### 6.14 如何基于扩展接口自定义负载均衡策略

#### 6.15 小结

### 第7章 Dubbo线程模型与线程池策略

#### 7.2 AllDispatcher源码剖析

#### 7.3 DirectDispatcher源码剖析

#### 7.4 MessageOnlyDispatcher源码剖析

#### 7.5 ExecutionDispatcher源码剖析

#### 7.6 ConnectionOrderedDispatcher源码剖析

#### 7.7 线程模型的确定时机

#### 7.8 如何基于扩展接口自定义线程模型

#### 7.9 Dubbo的线程池策略

#### 7.10 FixedThreadPool源码剖析

#### 7.11 LimitedThreadPool源码剖析

#### 7.12 EagerThreadPool源码剖析

#### 7.13 CachedThreadPool源码剖析

#### 7.14 线程池的确定时机

#### 7.15 如何基于扩展接口自定义线程池策略

#### 7.16 小结

### 第8章 Dubbo如何实现泛化引用

#### 8.1 服务消费端GenericImplFilter源码分析

#### 8.2 服务提供端GenericFilter源码分析

#### 8.3 小结

### 第9章 Dubbo并发控制

#### 9.1 服务消费端并发控制

#### 9.2 服务提供端并发控制

#### 9.3 小结

### 第10章 Dubbo隐式参数传递

#### 10.1 服务消费端AbstractClusterInvoker原理剖析

#### 10.2 服务提供方ContextFilter原理剖析

#### 10.3 小结

### 第11章 Dubbo全链路异步

#### 11.2 服务提供端异步执行

#### 11.3 异步调用与执行引入的新问题

#### 11.4 小结

### 第12章 本地服务暴露与引用原理

#### 12.1 本地服务暴露流程

#### 12.2 本地服务引用启动流程

#### 12.3 本地服务一次引用流程

#### 12.4 小结

### 第13章 Dubbo协议与网络传输

#### 13.1 Dubbo协议

#### 13.2 服务消费方编码原理

#### 13.3 服务发布方解码原理

#### 13.4 小结

## 实践篇

### 第14章 Dubbo实践

#### 14.2 查看扩展接口适配器类的源码

#### 14.3 查看服务提供端Wrapper类的源码

#### 14.4 查询Dubbo启动后都有哪些Filter

#### 14.5 Demo验证RoundRobin LoadBalance负载均衡原理

#### 14.6 如何动态获取Dubbo服务提供方地址列表

#### 14.7 根据IP动态路由调用Dubbo服务

#### 14.8 基于CompletableFuture和Netty模拟RPC同步与纯异步调用

#### 14.9 小结

# 读书笔记

# 本书评论
