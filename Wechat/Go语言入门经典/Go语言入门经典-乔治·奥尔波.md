---
doc_type: weread-highlights-reviews
bookId: "26473034"
author: 乔治·奥尔波
cover: https://cdn.weread.qq.com/weread/cover/93/YueWen_26473034/t7_YueWen_26473034.jpg
reviewCount: 5
noteCount: 32
readingStatus: 读过
progress: 54%
totalReadDay: 19
readingTime: 6小时58分钟
readingDate: 2020-11-04
isbn: 9787115485038
lastReadDate: 2020-11-08

---
# 元数据
> [!abstract] Go语言入门经典
> - ![ Go语言入门经典|200](https://cdn.weread.qq.com/weread/cover/93/YueWen_26473034/t7_YueWen_26473034.jpg)
> - 书名： Go语言入门经典
> - 作者： 乔治·奥尔波
> - 简介： Go语言是谷歌推出的一种全新的编程语言，旨在不损失应用程序性能的情况下降低代码的复杂性，具有“部署简单、并发性好、语言设计良好、执行性能好”等优势，目前国内诸多IT公司均已采用Go语言开发项目。《Go语言入门经典》分为24章，讲解了使用Go语言编写高质量程序的方法，其内容涵盖了Go语言特性和标准库安装包，Go与JavaScript的对比，Go命令行工具，Go中的基本概念（比如类型、变量、函数、控制结构、指针、接口等）、错误处理、Goroutine和通道、Go代码测试、使用Go编写HTTP客户端与服务器、处理JSON和文件、部署Go代码等。《Go语言入门经典》适合想要掌握Go语言的零基础读者以及对Go语言感兴趣的程序员学习，还可作为高等院校教授Go语言课程的教材。
> - 出版时间 2018-08-01 00:00:00
> - ISBN： 9787115485038
> - 分类： 计算机-编程设计
> - 出版社： 人民邮电出版社
> - PC地址：https://weread.qq.com/web/reader/12132c407193f24a121a46b

# 高亮划线

## 封面

## 版权信息

## 内容提要

## 作者简介

## 献辞

## 致谢

## 前言

### 本书读者对象

### 为何要学习Go语言

### 组织结构

### 代码示例

## 资源与支持

### 配套资源

### 提交勘误

### 扫码关注本书

### 与我们联系

### 关于异步社区和异步图书

## 第1章 起步

### 1.1 Go简介

### 1.2 安装Go

### 1.3 设置环境

### 1.4 编写第一个Go程序——Hello World

### 1.5 小结

### 1.6 问与答

### 1.7 作业

### 1.8 练习

## 第2章 理解类型

### 2.1 数据类型是什么

### 2.2 区分静态类型和动态类型

### 2.3 使用布尔类型

### 2.4 理解数值类型

> 📌 声明数组时，必须指定其长度和类型 
> ⏱ 2020-11-04 12:47:57 ^26473034-31-2965-2981

### 2.5 检查变量的类型

### 2.6 类型转换

### 2.7 小结

### 2.8 问与答

### 2.9 作业

### 2.10 练习

## 第3章 理解变量

### 3.1 变量是什么

### 3.2 快捷变量声明

### 3.3 理解变量和零值

### 3.4 编写简短变量声明

> 📌 使用简短变量声明时，编译器会推断变量的类型，因此您无须显式地指定变量的类型。请注意，只能在函数中使用简短变量声明。 
> ⏱ 2020-11-04 19:28:21 ^26473034-42-1015-1072

### 3.5 变量声明方式

### 3.6 理解变量作用域

> 📌 每个内部块都可访问其外部块，但外部块不能访问内部块 
> ⏱ 2020-11-04 19:46:53 ^26473034-44-987-1012

> 📌 程序清单3.10显示了大括号定义的程序结构和变量作用域。每对大括号都表示一个块。➢ 代码的缩进程度反映了块作用域的层级。在每个块中，代码都被缩进。➢ 在内部块中，可引用外部块中声明的变量。 
> ⏱ 2020-11-04 19:48:58 ^26473034-44-1045-1199

> 📌 Go语言将文件也视为块 
> ⏱ 2020-11-04 19:55:26 ^26473034-44-1847-1858

### 3.7 使用指针

> 📌 变量传递给函数时，会分配新内存并将变量的值复制到其中 
> ⏱ 2020-11-04 20:02:12 ^26473034-45-1943-1969

> 📌 指针是Go语言中的一种类型 
> ⏱ 2020-11-04 20:02:45 ^26473034-45-2083-2096

### 3.8 声明常量

### 3.9 小结

### 3.10 问与答

### 3.11 作业

### 3.12 练习

## 第4章 使用函数

### 4.1 函数是什么

> 📌 函数的第一行中提供的，而这一行被称为函数签名 
> ⏱ 2020-11-04 20:11:13 ^26473034-52-759-781

> 📌 让每个函数只做一件事情并把这件事情做好。软件不可避免地要修改，通过结合使用大量简短的函数，可让软件更容易修改。这还有助于测试各个函数以及整个软件。➢ 维护。在团队合作开发中，您编写的函数易于阅读和理解吗？如果不是这样的，就说明它过于复杂或必须添加注释。别忘了，您可能在一年后的午夜时分回过头来阅读这个函数！ 
> ⏱ 2020-11-04 20:18:14 ^26473034-52-2061-2244

### 4.2 定义不定参数函数

### 4.3 使用具名返回值

> 📌 在终止语句return前给具名变量进行了赋值。使用具名返回值时，无须显式地返回相应的变量。这被称为裸（naked）return语句。 
> ⏱ 2020-11-04 21:14:23 ^26473034-54-762-828

### 4.4 使用递归函数

### 4.5 将函数作为值传递

### 4.6 小结

### 4.7 问与答

### 4.8 作业

### 4.9 练习

## 第5章 控制流程

### 5.1 使用if语句

### 5.2 使用else语句

### 5.3 使用else if语句

### 5.4 使用比较运算符

### 5.5 使用算术运算符

### 5.6 使用逻辑运算符

### 5.7 使用switch语句

### 5.8 使用for语句进行循环

### 5.9 使用defer语句

### 5.10 小结

### 5.11 问与答

### 5.12 作业

### 5.13 练习

## 第6章 数组、切片和映射

### 6.1 使用数组

### 6.2 使用切片

> 📌  [插图] ^26473034-77-4058-4059
- 💭 这里的'...'是go的一个语法糖，放在函数声明中说明这是一个不定参数，放在函数调用中数目这里的数组被打散成多个传递 - ⏱ 2020-11-05 10:45:01 

> 📌 在复制切片中的元素前，必须再声明一个类型与该切片相同的切片 
> ⏱ 2020-11-05 10:44:51 ^26473034-77-4939-4968

### 6.3 使用映射

### 6.4 小结

### 6.5 问与答

> 📌 不需要。使用内置函数make创建映射时，可使用第二个参数，但这个参数只是容量提示，而非硬性规定。映射可根据要存储的元素数量自动增大，因此没有必要指定长度 
> ⏱ 2020-11-05 14:13:07 ^26473034-80-816-892

### 6.6 作业

### 6.7 练习

## 第7章 使用结构体和指针

> 📌 结构体是由数据元素组成的结构 
> ⏱ 2020-11-05 18:40:46 ^26473034-83-878-892

### 7.1 结构体是什么

> 📌 在大括号内，使用名称和类型指定了一系列数据字段。请注意，此时没有给数据字段赋值。可将结构体视为模板。 
> ⏱ 2020-11-05 21:36:47 ^26473034-84-1334-1384

### 7.2 创建结构体

> 📌 ，可使用占位符%+v并将其传入结构体。[插图] 
> ⏱ 2020-11-05 21:38:09 ^26473034-85-664-692

> 📌 也可使用关键字new来创建结构体实例 
> ⏱ 2020-11-05 21:44:18 ^26473034-85-2425-2443

### 7.3 嵌套结构体

### 7.4 自定义结构体数据字段的默认值

> 📌  使用构造函数自定义默认值 ^26473034-87-1351-1363
- 💭 这里没有问题吗。Sound是bool值，哎，这作者真是来赚钱的吧 - ⏱ 2020-11-05 22:04:38 

### 7.5 比较结构体

> 📌 对结构体进行比较，要先看它们的类型和值是否相同。对于类型相同的结构体，可使用相等性运算符来比较 
> ⏱ 2020-11-05 22:06:49 ^26473034-88-424-471

> 📌 要判断两个结构体是否相等，可使用==；要判断它们是否不等，可使用!= 
> ⏱ 2020-11-05 22:07:02 ^26473034-88-472-506

> 📌 不能对两个类型不同的结构体进行比较，否则将导致编译错误 
> ⏱ 2020-11-05 22:10:42 ^26473034-88-1762-1789

### 7.6 理解公有和私有值

### 7.7 区分指针引用和值引用

### 7.8 小结

### 7.9 问与答

### 7.10 作业

### 7.11 练习

## 第8章 创建方法和接口

### 8.1 使用方法

### 8.2 创建方法集

> 📌 是一种封装功能和创建库代码的有效方式。 
> ⏱ 2020-11-06 08:38:31 ^26473034-97-524-543

### 8.3 使用方法和指针

> 📌 接收者参数声明为指针引用和值引用的差别 
> ⏱ 2020-11-06 19:45:22 ^26473034-98-1496-1515

> 📌 将指针作为接收者的方法能够修改原始结构体的数据字段，这是因为它使用的是指向原始结构体内存单元的指针，因此操作的不是原始结构体的副本。 
> ⏱ 2020-11-06 19:46:45 ^26473034-98-2998-3064

> 📌 如果需要修改原始结构体，就使用指针；如果需要操作结构体，但不想修改原始结构体，就使用值。 
> ⏱ 2020-11-06 19:48:15 ^26473034-98-4239-4283

### 8.4 使用接口

> 📌 从高级层面说，接口还有助于理解代码设计。在无须关心实现的情况下，很容易理解设计是什么样的。 
> ⏱ 2020-11-06 19:51:07 ^26473034-99-892-937

> 📌  这个实现很简单，但满足了接口Robot的要求，因为它包含方法PowerOn，且这个方法的函数签名与接口Robot要求的一致。接口的强大之处在于，它们支持多种实现。例如，您也可以像下面这样来实现接口Robot。 ^26473034-99-1184-1288
- 💭 这里不是重点，只是想说的是上面的方法抛出的错应该是error - ⏱ 2020-11-06 20:03:03 

> 📌 接口也是一种类型，可作为参数传递给函数 
> ⏱ 2020-11-06 20:19:06 ^26473034-99-1784-1803

> 📌 对结构体和方法有基本认识后，如果您熟悉其他语言，可能会问：Go是面向对象的吗？面向对象编程是这样一种编程范式：使用具有特定行为的对象来建立数据模型。通常，面向对象语言提供允许一种对象继承另一种对象的功能。虽然Go语言没有提供类和类继承等面向对象功能，但结构体和方法集弥补了这部分不足，提供了一些面向对象元素。由此可以说Go在不使用类和继承的情况下提供了类似于面向对象编程的功能，虽然这一点还存在争议。 
> ⏱ 2020-11-06 20:21:15 ^26473034-99-3588-3788

> 📌 如果一个方法集实现了一个接口，就可以说它与另一个实现了该接口的方法集互为多态 
> ⏱ 2020-11-06 20:58:43 ^26473034-99-4192-4230

### 8.5 小结

### 8.6 问与答

### 8.7 作业

### 8.8 练习

## 第9章 使用字符串

### 9.1 创建字符串字面量

### 9.2 理解rune字面量

### 9.3 拼接字符串

### 9.4 小结

### 9.5 问与答

### 9.6 作业

### 9.7 练习

## 第10章 处理错误

> 📌 Go语言处理错误的方式很有趣——将错误作为一种类型，这意味着可将错误传递给函数和方法。 
> ⏱ 2020-11-06 23:53:09 ^26473034-112-907-950

### 10.1 错误处理及Go语言的独特之处

> 📌 在Go语言中，有一种约定是，如果没有发生错误，返回的错误值将为nil。这让程序员调用方法或函数时，能够检查它是否像预期那样执行完毕。 
> ⏱ 2020-11-08 10:07:13 ^26473034-113-1979-2045

### 10.2 理解错误类型

### 10.3 创建错误

### 10.4 设置错误的格式

### 10.5 从函数返回错误

### 10.6 错误和可用性

### 10.7 慎用panic

### 10.8 小结

### 10.9 问与答

### 10.10 作业

### 10.11 练习

## 第11章 使用Goroutine

### 11.1 理解并发

### 11.2 并发和并行

### 11.3 通过Web浏览器来理解并发

### 11.4 阻塞和非阻塞代码

### 11.5 使用Goroutine处理并发操作

### 11.6 定义Goroutine

### 11.7 小结

### 11.8 问与答

### 11.9 作业

### 11.10 练习

## 第12章 通道简介

### 12.1 使用通道

### 12.2 使用缓冲通道

### 12.3 阻塞和流程控制

### 12.4 将通道用作函数参数

### 12.5 使用select语句

### 12.6 退出通道

### 12.7 小结

### 12.8 问与答

### 12.9 作业

### 12.10 练习

## 第13章 使用包实现代码重用

### 13.1 导入包

### 13.2 理解包的用途

### 13.3 使用第三方包

### 13.4 安装第三方包

### 13.5 管理第三方依赖

### 13.6 创建包

### 13.7 小结

### 13.8 问与答

### 13.9 作业

### 13.10 练习

## 第14章 Go语言命名约定

### 14.1 Go代码格式设置

### 14.2 使用gofmt

### 14.3 配置文本编辑器

### 14.4 命名约定

### 14.5 使用golint

### 14.6 使用godoc

### 14.7 工作流程自动化

### 14.8 小结

### 14.9 问与答

### 14.10 作业

### 14.11 练习

## 第15章 测试和性能

### 15.1 测试：软件开发最重要的方面

### 15.2 testing包

### 15.3 运行表格驱动测试

### 15.4 基准测试

### 15.5 提供测试覆盖率

### 15.6 小结

### 15.7 问与答

### 15.8 作业

### 15.9 练习

## 第16章 调试

### 16.1 日志

### 16.2 打印数据

### 16.3 使用fmt包

### 16.4 使用Delve

### 16.5 使用gdb

### 16.6 小结

### 16.7 问与答

### 16.8 作业

### 16.9 练习

## 第17章 使用命令行程序

### 17.1 操作输入和输出

### 17.2 访问命令行参数

### 17.3 分析命令行标志

### 17.4 指定标志的类型

### 17.5 自定义帮助文本

### 17.6 创建子命令

### 17.7 POSIX兼容性

### 17.8 安装和分享命令行程序

### 17.9 小结

### 17.10 问与答

### 17.11 作业

### 17.12 练习

## 第18章 创建HTTP服务器

### 18.1 通过Hello World Web服务器宣告您的存在

### 18.2 查看请求和响应

### 18.3 使用处理程序函数

### 18.4 处理404错误

### 18.5 设置报头

### 18.6 响应以不同类型的内容

### 18.7 响应不同类型的请求

### 18.8 获取GET和POST请求中的数据

### 18.9 小结

### 18.10 问与答

### 18.11 作业

### 18.12 练习

## 第19章 创建HTTP客户端

### 19.1 理解HTTP

### 19.2 发出GET请求

### 19.3 发出POST请求

### 19.4 进一步控制HTTP请求

### 19.5 调试HTTP请求

### 19.6 处理超时

### 19.7 小结

### 19.8 问与答

### 19.9 作业

### 19.10 练习

## 第20章 处理JSON

### 20.1 JSON简介

### 20.2 使用JSON API

### 20.3 在Go语言中使用JSON

### 20.4 解码JSON

### 20.5 映射数据类型

### 20.6 处理通过HTTP收到的JSON

### 20.7 小结

### 20.8 问与答

### 20.9 作业

### 20.10 练习

## 第21章 处理文件

### 21.1 文件的重要性

### 21.2 使用ioutil包读写文件

### 21.3 写入文件

### 21.4 列出目录的内容

### 21.5 复制文件

### 21.6 删除文件

### 21.7 使用文件来管理配置

### 21.8 小结

### 21.9 问与答

### 21.10 作业

### 21.11 练习

## 第22章 正则表达式简介

### 22.1 定义正则表达式

### 22.2 熟悉正则表达式语法

### 22.3 使用正则表达式验证数据

### 22.4 使用正则表达式来变换数据

### 22.5 小结

### 22.6 问与答

### 22.7 作业

### 22.8 练习

## 第23章 Go语言时间编程

### 23.1 时间元素编程

### 23.2 让程序休眠

### 23.3 设置超时时间

### 23.4 使用ticker

### 23.5 以字符串格式表示时间

### 23.6 使用结构体Time

### 23.7 时间加减

### 23.8 比较两个不同的Time结构体

### 23.9 小结

### 23.10 问与答

### 23.11 作业

### 23.12 练习

## 第24章 部署Go语言代码

### 24.1 理解目标

### 24.2 压缩二进制文件的大小

### 24.3 使用Docker

### 24.4 下载二进制文件

### 24.5 使用go get

### 24.6 通过包管理器发布代码

### 24.7 小结

### 24.8 问与答

### 24.9 作业

### 24.10 练习

# 读书笔记

## 5.8 使用for语句进行循环

### 划线评论
> 📌 语句

1:  package main
2:
3:  import (
4:       "fmt"
5:  )
6:
7:  func main() {
8:       for i := 0; i < 10; i++ {
9:            fmt.Println("i is", i)
10:       }
11:  }  ^14417636-7lCKylXKd
    - 💭 要注意这个程序的结果是0-9，上一个程序是1-10，作者的严谨态度值得推敲
    - ⏱ 2020-11-05 08:21:44

### 划线评论
> 📌 []int{1, 2, 3, 4}
9:       for i, n :=  ^14417636-7lCKs93NL
    - 💭 这里是切片吧
    - ⏱ 2020-11-05 08:20:13
   
## 6.2 使用切片

### 划线评论
> 📌 [插图]  ^14417636-7lCU1EQCe
    - 💭 这里的'...'是go的一个语法糖，放在函数声明中说明这是一个不定参数，放在函数调用中数目这里的数组被打散成多个传递
    - ⏱ 2020-11-05 10:46:23
   
## 7.4 自定义结构体数据字段的默认值

### 划线评论
> 📌 使用构造函数自定义默认值  ^14417636-7lDCukFpr
    - 💭 这里没有问题吗。Sound是bool值，哎，这作者真是来赚钱的吧
    - ⏱ 2020-11-05 22:05:16
   
## 8.4 使用接口

### 划线评论
> 📌 这个实现很简单，但满足了接口Robot的要求，因为它包含方法PowerOn，且这个方法的函数签名与接口Robot要求的一致。接口的强大之处在于，它们支持多种实现。例如，您也可以像下面这样来实现接口Robot。  ^14417636-7lF0QBGof
    - 💭 这里不是重点，只是想说的是上面的方法抛出的错应该是error
    - ⏱ 2020-11-06 20:03:53
   
# 本书评论
