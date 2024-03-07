---
doc_type: weread-highlights-reviews
bookId: "730634"
author: 郝佳编著
cover: https://wfqqreader-1252317822.image.myqcloud.com/cover/634/730634/t7_730634.jpg
reviewCount: 0
noteCount: 0
readingStatus: 在读
progress: 97%
totalReadDay: 10
readingTime: 1小时27分钟
readingDate: 2021-06-09
isbn: 9787115325686
lastReadDate: 2022-11-08

---
# 元数据
> [!abstract] Spring源码深度解析
> - ![ Spring源码深度解析|200](https://wfqqreader-1252317822.image.myqcloud.com/cover/634/730634/t7_730634.jpg)
> - 书名： Spring源码深度解析
> - 作者： 郝佳编著
> - 简介： 　　《Spring源码深度解析》从核心实现和企业应用两个方面，由浅入深、由易到难地对Spring源码展开了系统的讲解，包括Spring的设计理念和整体架构、容器的基本实现、默认标签的解析、自定义标签的解析、bean的加载、容器的功能扩展、AOP、数据库连接JDBC、整合MyBatis、事务、SpringMVC、远程服务、Spring消息服务等内容。
　　《Spring源码深度解析》不仅介绍了使用Spring框架开发项目必须掌握的核心概念，还指导读者如何使用Spring框架编写企业级应用，并针对在编写代码的过程中如何优化代码、如何使得代码高效给出切实可行的建议，从而帮助读者全面提升实战能力。
　　《Spring源码深度解析》语言简洁，示例丰富，可帮助读者迅速掌握使用Spring进行开发所需的各种技能。《Spring源码深度解析》适合于已具有一定Java编程基础的读者，以及在Java平台下进行各类软件开发的开发人员、测试人员等。
> - 出版时间 2013-09-01 00:00:00
> - ISBN： 9787115325686
> - 分类： 计算机-计算机综合
> - 出版社： 人民邮电出版社
> - PC地址：https://weread.qq.com/web/reader/0e232b205b260a0e29f3fb5

# 高亮划线

## 封面

## 版权页

## 前言

## 作者简介

## 第一部分 核心实现

### 第2章 容器的基本实现

### 第3章 默认标签的解析

### 第4章 自定义标签的解析

### 第5章 bean的加载

### 第6章 容器的功能扩展

> 📌 registerBeanPostProcessors(beanFactory);
// 为上下文初始化Message源，即不同语言的消息体 ，国际化处理
initMessageSource();
// Initialize event multicaster for this context.
// 初始化应用消息广播器，并放入“applicationEventMulticaster”bean中
initApplicationEventMulticaster();
// Initialize other special beans in specific context subclasses.
// 留给子类来初始化其它的Bean
onRefresh();
// Check for listener beans and register them.
// 在所有注册的bean中查找Listener bean，注册到消息广播器中
registerListeners();
// Instantiate all remaining (non-lazy-init) singletons. 
> ⏱ 2022-11-08 02:42:05 ^730634-10-4527

### 第7章 AOP

## 第二部分 企业应用

### 第9章 整合MyBatis

### 第10章 事务

### 第11章 SpringMVC

### 第12章 远程服务

### 第13章 Spring消息

# 读书笔记

# 本书评论
