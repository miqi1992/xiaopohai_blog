## 第十四章：MongoDB 数据库



- "NoSQL"⼀词最早于1998年被⽤于⼀个轻量级的关系数据库的名字
- 随着web2.0的快速发展， NoSQL概念在2009年被提了出来
- NoSQL在2010年⻛⽣⽔起， 现在国内外众多⼤⼩⽹站， 如facebook、 google、 淘宝、 京东、 百度等， 都在使⽤nosql开发⾼性能的产品
- 对于⼀名程序员来讲， 使⽤nosql已经成为⼀条必备技能
- NoSQL最常⻅的解释是“non-relational”， “Not Only SQL”也被很多⼈接受， 指的是⾮关系型的数据库



### mongoDB 的优势

- 易扩展： NoSQL数据库种类繁多， 但是⼀个共同的特点都是去掉关系数据库的关系型特性。 数据之间⽆关系， 这样就⾮常容易扩展



- ⼤数据量， ⾼性能： NoSQL数据库都具有⾮常⾼的读写性能， 尤其在⼤数据量下， 同样表现优秀。 这得益于它的⽆关系性， 数据库的结构简单



- 灵活的数据模型： NoSQL⽆需事先为要存储的数据建⽴字段， 随时可以存储⾃定义的数据格式。 ⽽在关系数据库⾥， 增删字段是⼀件⾮常麻烦的事情。 如果是⾮常⼤数据量的表， 增加字段简直就是⼀个噩梦



### 关于 DadaBase 的基础命令

- 查看当前数据库: db

- 查看所有的数据库: show dbs / show databases

- 切换数据库: use db_name

- 删除当前数据库: db.DropDatabase()

在mongodb数据库中, 没有新建数据库的指令，可以直接use需要新建的数据库，mongo会自动创建。

```js
// 当前mongo不存在test1 但是照样可以切换
use test1

//注意点：在数据库没有数据时，数据库并不会真正创建 当插入数据时，就可以使用show dbs查看到test1了
```



### 关于集合的基础命令

在mongodb数据库中没有表的概念，数据都是存储在集合中。

集合创建：

```js
// 自动创建集合
// 向不存在的集合中第一次加入数据时，集合会被创建出来

// 手动创建集合 语法
db.createCollection(set_name, options)

db.createCollection("stu")
db.createCollection("sub", {capped: true, size: 10})

// 参数capped: 默认值为false表示不设置上限，值为true表示设置上限

// 参数size: 当capped值为true时，需要指定此参数，表示上限大小。
// 当文档达到上限时，会将之前的数据覆盖，单位为字节。

// 查看集合
show collections

// 删除集合
db.集合名称.drop()
```



### 数据类型

- Object ID：文档ID

- String：字符串 该属性是最常用的数据类型，并且为一个有效的UTF-8字符集

- Boolean：存储一个布尔值，true或者false

- Integer：整数 可以是32位或64位，取决于服务器

- Double：浮点值

- Arrays：数组或列表，多个值存储到一个键

- Object：用于嵌入式的文档，即一个值为一个文档

- Null：Null值

- Timestamp: 时间戳 表示1970-1-1到现在的总秒数

- Date：存储当前日期或时间的UNIX时间格式



每个文档都有自己的属性，为_id。保证每个文档的唯一性

可以自己去设置\_id插入文档，如果没有提供，那么mongodb为每个文档提供一个独特的\_id，类型为object ID



object ID是一个12字节的十六进制数：

- 前4个字节为当前时间戳
- 接下来3个字节为机器ID
- 后2个字节为MongoDB的服务进程ID
- 最后3个字节是简单的增量值



### 数据操作

##### 数据插入

```js
//db.集合名称.insert(docment)
// 例如
db.stu_test.insert({"name": "xiaoming", "age": 10})

// 查询当前集合数据 并返回_id
db.stu_test.find()

// 在终端中插入的数据为json 并且数据中的键可以省略双引号
db.stu_test.insert({name: "xiaohong", age: 18})
db.stu_test.find()
```



##### 数据保存

```js
// db.集合名称.save(document)
// 如果文档的_id已经存在则修改，如果文档的_id不存在则添加数据

db.stu_test.insert({_id: 10010, name: "xiaoming", age: 30})

// 尝试插入数据 如果当前_id相同，则报错[当前id值重复]
db.stu_test.insert({_id: 10010, name: "xiaoming", age: 40})
// 调用save保存则更新数据
db.stu_test.save({_id: 10010, name: "xiaoming", age: 40})
```



##### 数据更新

```js
// db.集合名称.update(<query>, <update>, {multi: <boolean>})
// query: 查询条件

// update: 更新操作符

// multi: 可选参数，默认为false。
// 表示只更新找到的第一条记录，值为true则更新满足条件的全部数据

// 当前这条语句会替换之前的记录，则原数据中的age会消失
db.stu_test.update({name: "xiaowang"}, {name: "xiaozhao"})
// 为了不影响除了name之外的其他数据 需要使用$set语法
db.stu_test.update({name: "xiaowang"}, {$set: {name: "xiaozhao"}})

// 更新所有符合条件的数据 multi参数必须和$符配合使用
db.stu_test.update({name: "xiaowang"}, {$set: {name: "xiaozhao"}}, {multi: true})
```



##### 数据删除

```js
// db.集合名称.remove(<query>, {justOne: <boolean>})
// query: 删除指定文档的条件
// justOne: 可选参数，如果设置为true或1，删除一条。默认false，表示删除多条

// 删除符合条件的全部数据
db.stu_test.remove({name: "xiaozhao"})
// 删除一条
db.stu_test.remove({name: "xiaozhao"}, {justOne: true})
```



### 学习数据查询之前的数据准备

```js
db.getCollection("products").insert([ {
    _id: 100,
    sku: "abc123",
    description: "Single line description"
} ]);
db.getCollection("products").insert([ {
    _id: 101,
    sku: "abc789",
    description: "First line\nSecond line"
} ]);
db.getCollection("products").insert([ {
    _id: 102,
    sku: "xyz456",
    description: "Many spaces before    line"
} ]);
db.getCollection("products").insert([ {
    _id: 103,
    sku: "xyz789",
    description: "Multiple\nline description"
} ]);
db.getCollection("products").insert([ {
    _id: 104,
    sku: "abc123",
    description: "Single line description"
} ]);



db.getCollection("stu_info").insert([ {
    _id: ObjectId("626ba04fce0a56f10342b6f3"),
    name: "郭靖",
    hometown: "蒙古",
    age: 20,
    gender: true
} ]);
db.getCollection("stu_info").insert([ {
    _id: ObjectId("626ba07cce0a56f10342b6f4"),
    name: "黄蓉",
    hometown: "桃花岛",
    age: 18,
    gender: false
} ]);
db.getCollection("stu_info").insert([ {
    _id: ObjectId("626ba0acce0a56f10342b6f5"),
    name: "华筝",
    hometown: "蒙古",
    age: 18,
    gender: false
} ]);
db.getCollection("stu_info").insert([ {
    _id: ObjectId("626ba0d2ce0a56f10342b6f6"),
    name: "黄药师",
    hometown: "桃花岛",
    age: 40,
    gender: true
} ]);
db.getCollection("stu_info").insert([ {
    _id: ObjectId("626ba0f1ce0a56f10342b6f7"),
    name: "段誉",
    hometown: "大理",
    age: 16,
    gender: true
} ]);
db.getCollection("stu_info").insert([ {
    _id: ObjectId("626ba10bce0a56f10342b6f8"),
    name: "段王爷",
    hometown: "大理",
    age: 45,
    gender: true
} ]);
db.getCollection("stu_info").insert([ {
    _id: ObjectId("626ba12cce0a56f10342b6f9"),
    name: "洪七公",
    hometown: "华山",
    age: 18,
    gender: true
} ]);

```



### 数据查询

##### find 方法

```js
// 普通查询: find()
db.集合名称.find({条件文档})

// 进入到stu_info集合中进行查询  数据源使用js文件进行录入
db.stu_info.find({age:20})

// 查询一个: findOne()
// db.集合名称.findOne({条件文档})
db.stu_info.findOne({age:18})

// pretty(): 将结果格式化
// db.集合名称.find({条件文档}).pretty()
db.stu_info.find().pretty()
```



##### 比较运算符

- 等于：默认是等于判断，没有运算符
- 小于：$lt
- 小于等于：$lte
- 大于：$gt
- 大于等于：$gte
- 不等于：$ne

```js
db.stu_info.find({age: {$gte: 18}})
```



##### 范围运算符

```js
// 使用"$in"进行返回查询 符合18或28的返回
db.stu_info.find({age: {$in: [18, 28]}})
```



##### 逻辑运算符

```js
// 并且查询
db.stu_info.find({age: 18, hometown: "桃花岛"})

// 或者查询
// db.集合名称.find({$or: [{查询条件}， {查询条件}]})
db.stu_info.find({$or: [{age: 18}, {hometown: "桃花岛"}]})

// 将之前所学的查询进行整合使用
// 如果语句过长，可以使用编辑器写上查询语句 并且换行不影响
db.stu_info.find({$or: [{age: {$gte: 45}}, {hometown: {$in:["桃花岛", "华山"]}}]})
```



##### 正则表达式查询

```js
// 使用//或$regex编写正则表达式
db.products.find({sku:/^abc/})
db.products.find({sku: {$regex: "789$"}})
```



##### limit 与 skip

```js
// ⽅法limit()： ⽤于读取指定数量的⽂档
// db.集合名称.find().limit(NUMBER)

// 查询两条信息
db.products.find().limit(2)

// 查询除了前两条之外的所有记录
db.products.find().skip(2)

// 组合使用
db.products.find().skip(2).limit(2)
```



##### 自定义查询

```js
// 使⽤$where后⾯写⼀个函数， 返回满⾜条件的数据

// 查询年龄⼤于30的学⽣
db.stu_1.find({$where: function(){return this.age<=18}})
```



##### 投影

```js
// 在查询到的返回结果中，只选择必要的字段
// 如果不想显示_id，需要单独设置_id: 0
// db.集合名称.find({条件}, {字段名称1, 字段名称2...})

db.stu_info.find({age: {$gt: 18}}, {name: 1, _id: 0})

// 无条件进行投影查询
db.stu_info.find({}, {name: 1, _id: 0})
```



##### 排序

```js
// 方法sort() 用于对集合进行排序
// db.集合名称.find().sort({字段: 1})

// 1为升序 -1为降序
db.stu_info.find().sort({age: -1})

// 多条件排序
db.stu_info.find().sort({age: -1, gender: -1})

// 筛选年龄大于18并使用age进行正序排序
db.stu_info.find({age: {$gt: 18}}).sort({age: 1})
```



##### 记录统计

```js
// 统计学生个数
db.stu_info.find().count()

// 统计年龄大于18的学生个数
db.stu_info.find({age: {$gt: 18}}).count()

// 第二种写法
db.stu_info.count()
db.stu_info.count({age: {$gt: 18}})
```



##### 数据去重

```js
// 对地址进行去重
db.stu_info.distinct("hometown")

// 结合条件查询去重
db.stu_info.distinct("hometown", {age: {$gt: 20}})
```



### 聚合操作

聚合(aggregate)是基于数据处理的聚合管道，每个文档通过一个由多个阶段（stage）组成的管道，可以对每个阶段的管道进行分组、过滤等功能，然后经过一系列的处理，输出相应的结果。 

```js
db.集合名称.aggregate({管道:{表达式}})
```



##### 常用管道

```js
$group： 
  将集合中的⽂档分组, 可⽤于统计结果
  
$match： 
  过滤数据, 只输出符合条件的⽂档
  
$project： 
  修改输⼊⽂档的结构, 如重命名、增加、删除字段、创建计算结果
  
$sort： 
  将输⼊⽂档排序后输出
  
$limit： 
  限制聚合管道返回的⽂档数
  
$skip： 
  跳过指定数量的⽂档， 并返回余下的⽂档
  
$unwind： 
  将数组类型的字段进⾏拆分
```



##### 表达式

```js
$sum： 
  计算总和， $sum:1 表示以⼀倍计数
  
$avg： 
  计算平均值
  
$min： 
  获取最⼩值
  
$max： 
  获取最⼤值
  
$push： 
  在结果⽂档中插⼊值到⼀个数组中
  
$first： 
  根据资源⽂档的排序获取第⼀个⽂档数据
  
$last： 
  根据资源⽂档的排序获取最后⼀个⽂档数据
```



##### $group

- 将集合中的文档分组，可用于统计结果

- _id表示分组的依据，使用某个字段的格式为"$字段"



案例：统计男生、女生的总人数

```js
// $sum: 1 可以理解成统计文档中分组之后的每一条数据的结果 * 1
db.stu_info.aggregate(
  {
    $group: {_id: "$gender", counter: {$sum: 1}}
  }
)
```

案例：按照 gender 进行分组，获取不同组数据的个数和平均年龄

```js
db.stu_info.aggregate(
  {
    $group: {_id: "$gender", count: {$sum: 1}, avg_age: {$avg: "$age"}}
  }
)
```



##### group by null

- 将集合中所有的文档分为一组
    - 案例：求学生总人数、平均年龄


```js
db.stu_info.aggregate(
  {
    $group: {_id: null, counter: {$sum: 1}, avg_age: {$avg: "$age"}}
  }
)
```



##### group的注意点

- $group 对应的字典中有几个键，结果中就有几个键
- 分组依据需要放在_id后面
- 取不同的字段需要在字段前加$，例如：$gender、$age



##### $project

- 修改文档的结构。如：重命名、增加字段、删除字段、创建计算结果



案例：查询学生的姓名、年龄

```js
// 类似于投影
db.stu_info.aggregate(
  {
    $project: {_id: 0, name: 1, age: 1}
  }
)
```



案例：查询性别数据、人数统计、平均年龄，并以中文显示

```js
db.stu_info.aggregate(
  {
    $group: {_id: "$gender", count: {$sum: 1}, avg_age: {$avg: "$age"}}
  },
  {
    $project: {"性别": "$_id", "人数统计": "$count", "平均年龄": "$avg_age", _id: 0}
  }
)
```



##### $match

- 用于过滤数据，只输出符合条件的文档
    - match是管道命令，能将结果交给下一个管道，find()无法实现




案例： 查询年龄大于20的男生

```js
db.stu_info.aggregate(
  {
    $match: {age: {$gt: 20}}
  }
)
```



案例： 查询年龄大于20的男生人数、女生人数，并以中文字段输出
```js
db.stu_info.aggregate(
  {
    $match: {age: {$gt: 20}},
  },
  {
    $group: {_id: "$gender", counter: {$sum: 1}}
  },
  {
    $project: {"性别": "$_id", "统计人数": "$counter", _id: 0}
  }
)


# 拓展：查询年龄大于20或者归属地在蒙古或大理的人数
db.stu_info.aggregate(
  {
    $match: 
    {
      $or: 
        [
          {
            age: {$gt: 20}
          },
          {
            hometown: {$in: ["蒙古", "大理"]}
          }
        ]
    },
  },
  
  {
    $group: {_id: "$gender", counter: {$sum: 1}}
  },
  
  {
    $project: {"性别": "$_id", "统计人数": "$counter", _id: 0}
  }
)
```



##### $sort

- 将输入文档排序后输出



案例：查询学生信息，按年龄升序

```js
db.stu_info.aggregate(
  {
    $sort: {age: 1}
  }
)
```



案例：查询男生人数、女生人数。按人数降序

```js
db.stu_info.aggregate(
  {
    $group: {_id: "$gender", counter: {$sum: 1}}
  },
  {
    $sort: {counter: -1}
  }
)
```



##### $limit 与 $skip

- $limit
    - 限制聚合函数返回的文档数



案例：查询两条学生信息

```js
db.stu_info.aggregate(
  {$limit: 2}
)
```



- $skip
    - 跳过指定数量的文档，并返回余下的文档



案例：查询从第三条数据开始的学生信息

```js
db.stu_info.aggregate(
  {$skip: 2}
)
```



案例：取出当前从第一位学生之后的一条学生信息

```js
db.stu_info.aggregate({$skip: 1}, {$limit: 1})
```



### 索引 - 查询优化

##### 创建测试数据

```js
for(i = 0; i < 100000; i++){
  db.test_data.insert({name: "test" + i, age: i})
}

// 查询数据
db.test_data.find({name: "test10000"})
// 获取查询数据所花费的时间
db.test_data.find({name: "test10000"}).explain("executionStats")
```



##### 建立索引

```js
// 语法示例
// 1代表升序 -1代表降序
// db.集合名称.ensureIndex({属性: 1})

db.test_data.ensureIndex({name: 1})
db.test_data.find({name: "test10000"}).explain("executionStats")

// 查看索引
db.test_data.getIndexes()
```



##### 索引使用

> 在默认情况下创建的索引不是唯一索引

###### 创建唯一索引

```js
db.test_data.ensureIndex({"name": 1}, {"unique": true})
```

###### 创建唯一索引并消除重复

```js
db.test_data.ensureIndex({"name": 1}, {"unique": true, "dropDups": true})
```

###### 建立联合索引

```js
db.test_data.ensureIndex({name: 1, age: 1})
```



### Python 对接 MongoDB

##### 安装 pymongo
pip install pymongo -i https://pypi.douban.com/simple



##### 代码演示

```js
# 导入模块
from pymongo import MongoClient

# 实例化client建立连接并指定集合 当前mongo演示为本地
client = MongoClient(host='127.0.0.1', port=27017)
collection = client['mongo_data_info']['test_python']

# 插入一条数据
res = collection.insert_one({"name": "xiaoming", "age": 10})
print(res)

# 插入多条数据
data_list = [{"name": "test{}".format(i)} for i in range(10)]
collection.insert_many(data_list)

# 查询一条数据
t_1 = colltction.find_one({"name": "xiaowang"})
print(t_1)

# 查询所有记录
t_2 = collection.find({"name": "xiaowang"})
# 当前返回的是一个游标对象
print(t_2)

for i in t_2:
    print(i)
    
# 对游标对象进行列表类型转换
print(list(t_2))

# 更新一条数据
collection.update_one({"name": "test1"}, {"$set": {"name": "new_test1"}})
# 更新全部数据
collection.update_many({"name": "test1"}, {"$set": {"name": "new_test1"}})

# 删除一条数据
collection.delete_one({"name": "test10"})
# 删除全部数据
collection.delete_many({"name": "test9"})
```

