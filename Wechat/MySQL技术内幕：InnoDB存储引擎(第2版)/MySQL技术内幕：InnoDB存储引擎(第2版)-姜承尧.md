---
doc_type: weread-highlights-reviews
bookId: "603246"
author: 姜承尧
cover: https://cdn.weread.qq.com/weread/cover/55/YueWen_603246/t7_YueWen_603246.jpg
reviewCount: 1
noteCount: 2
readingStatus: 在读
progress: 52%
totalReadDay: 7
readingTime: 1小时36分钟
readingDate: 2021-08-08
isbn: 9787111422068
lastReadDate: 2022-07-07

---
# 元数据
> [!abstract] MySQL技术内幕：InnoDB存储引擎(第2版)
> - ![ MySQL技术内幕：InnoDB存储引擎(第2版)|200](https://cdn.weread.qq.com/weread/cover/55/YueWen_603246/t7_YueWen_603246.jpg)
> - 书名： MySQL技术内幕：InnoDB存储引擎(第2版)
> - 作者： 姜承尧
> - 简介： 本书从源代码的角度深度解析了InnoDB的体系结构、实现原理、工作机制，并给出了大量最佳实践，能帮助你系统而深入地掌握InnoDB，更重要的是，它能为你设计管理高性能、高可用的数据库系统提供绝佳的指导。
> - 出版时间 2013-05-30 00:00:00
> - ISBN： 9787111422068
> - 分类： 计算机-数据库
> - 出版社： 机械工业出版社
> - PC地址：https://weread.qq.com/web/reader/611329b059346e611427f1c

# 高亮划线

## 封面

## 版权信息

## 推荐序

## 前言

## 第1章 MySQL体系结构和存储引擎

## 第2章 InnoDB存储引擎

## 第3章 文件

## 第4章 表

## 第5章 索引与算法

> 📌 。当通过辅助索引来寻找数据时，InnoDB存储引擎会遍历辅助索引并通过叶级别的指针获得指向主键索引的主键，然后再通过主键索引来找到一个完整的行记录 
> ⏱ 2022-07-06 08:32:14 ^603246-9-16965-17038

> 📌 堆表的表类型，即行数据的存储按照插入的顺序存放。这与MySQL数据库的MyISAM存储引擎有些类似。 
> ⏱ 2022-07-07 08:25:17 ^603246-9-17234-17284

## 第6章 锁

## 第7章 事务

## 第8章 备份与恢复

## 第9章 性能调优

## 第10章 InnoDB存储引擎源代码的编译和调试

# 读书笔记

## 第5章 索引与算法

### 划线评论
> 📌 有的Microsoft SQL Server数据库DBA问过我这样的问题，为什么在Microsoft SQL Server数据库上还要使用索引组织表？堆表的书签使非聚集查找可以比主键书签方式更快，并且非聚集可能在一张表中存在多个，我们需要对多个非聚集索引进行查找。而且对于非聚集索引的离散读取，索引组织表上的非聚集索引会比堆表上的聚集索引慢一些  ^14417636-7Az7ojO4C
    - 💭 这里概念比较杂乱
    - ⏱ 2022-07-07 08:27:25
   
# 本书评论
