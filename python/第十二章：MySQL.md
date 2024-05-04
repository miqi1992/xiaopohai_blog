# 数据库 - MySQL

- 一个数据库就是一个完整的业务单元，可以包含多张表，数据被存储在表中
- 在表中为了更加准确的存储数据，保证数据的正确有效，可以在创建表的时候，为表添加一些强制性的验证，包括数据字段的类型、约束



### 数据类型

- 可以通过查看帮助文档查阅所有支持的数据类型
- 使用数据类型的原则是：够用就行，尽量使用取值范围小的，而不用大的，这样可以更多的节省存储空间
- 常用数据类型如下：

- - 整数：int，bit
    - 小数：decimal
    - 字符串：varchar,char
    - 日期时间: date, time, datetime
    - 枚举类型(enum)

- 特别说明的类型如下：

- - decimal表示浮点数，如decimal(5,2)表示共存5位数，小数占2位
    - char表示固定长度的字符串，如char(3)，如果填充'ab'时会补一个空格为'ab '
    - varchar表示可变长度的字符串，如varchar(3)，填充'ab'时就会存储'ab'
    - 字符串text表示存储大文本，当字符大于4000时推荐使用
    - 对于图片、音频、视频等文件，不存储在数据库中，而是上传到某个服务器上，然后在表中存储这个文件的保存路径

- 更全的数据类型可以参考http://blog.csdn.net/anxpp/article/details/51284106



### 约束

- 主键primary key：物理上存储的顺序
- 非空not null：此字段不允许填写空值
- 惟一unique：此字段的值不允许重复
- 默认default：当不填写此值时会使用默认值，如果填写时以填写为准
- 外键foreign key：对关系字段进行约束，当为关系字段填写值时，会到关联的表中查询此值是否存在，如果存在则填写成功，如果不存在则填写失败并抛出异常
- 说明：虽然外键约束可以保证数据的有效性，但是在进行数据的crud（增加、修改、删除、查询）时，都会降低数据库的性能，所以不推荐使用，那么数据的有效性怎么保证呢？答：可以在逻辑层进行控制



### 数据库操作

- 查看所有数据库

```sql
show databases;
```

- 使用数据库

```sql
use 数据库名;
```

- 查看当前使用的数据库

```sql
select database();
```

- 创建数据库

```sql
create database 数据库名 charset=utf8;

-- 例：
create database python charset=utf8;
```

- 删除数据库

```sql
drop database 数据库名;

-- 例：
drop database python;
```



### 数据表操作

- 查看当前数据库中所有表

```sql
show tables;
```

- 查看表结构

```sql
desc 表名;
```

- 创建表
- auto_increment表示自动增长

```sql
CREATE TABLE table_name(
    column1 datatype contrai,
    column2 datatype,
    column3 datatype,
    .....
    columnN datatype,
    PRIMARY KEY(one or more columns)
);
```

例：创建班级表

```sql
create table classes(
    id int unsigned auto_increment primary key not null,
    name varchar(10)
);
```

例：创建学生表

```sql
create table students(
    id int unsigned primary key auto_increment not null,
    name varchar(20) default '',
    age tinyint unsigned default 0,
    height decimal(5,2),
    gender enum('男','女','人妖','保密'),
    cls_id int unsigned default 0
)
```

- 修改表-添加字段

```sql
alter table 表名 add 列名 类型;
例：
alter table students add birthday datetime;
```

- 修改表-修改字段：重命名版

```sql
alter table 表名 change 原名 新名 类型及约束;
例：
alter table students change birthday birth datetime not null;
```

- 修改表-修改字段：不重命名版

```sql
alter table 表名 modify 列名 类型及约束;
例：
alter table students modify birth date not null;
```

- 修改表-删除字段

```sql
alter table 表名 drop 列名;
例：
alter table students drop birthday;
```

- 删除表

```sql
drop table 表名;
例：
drop table students;
```

- 查看表的创建语句

```sql
show create table 表名;
例：
show create table classes;
```



### 增删改查



##### 查询

- 查询所有列

```sql
select * from 表名;
例：
select * from classes;
```

- 查询指定列
- 可以使用as为列或表指定别名

```sql
select 列1,列2,... from 表名;
例：
select id,name from classes;
```



##### 增加

> 格式: INSERT [INTO] tb**_**name [(col**_**name,...)] {VALUES | VALUE} ({expr | DEFAULT},...),(...),...

- 说明：主键列是自动增长，但是在全列插入时需要占位，通常使用0或者 default 或者 null 来占位，插入成功后以实际数据为准
- 全列插入：值的顺序与表中字段的顺序对应

```sql
insert into 表名 values(...)
例：
insert into students values(0,’郭靖‘,1,'蒙古','2016-1-2');
```

- 部分列插入：值的顺序与给出的列顺序对应

```sql
insert into 表名(列1,...) values(值1,...)
例：
insert into students(name,hometown,birthday) values('黄蓉','桃花岛','2016-3-2');
```

- 上面的语句一次可以向表中插入一行数据，还可以一次性插入多行数据，这样可以减少与数据库的通信
- 全列多行插入：值的顺序与给出的列顺序对应

```sql
insert into 表名 values(...),(...)...;
例：
insert into classes values(0,'python1'),(0,'python2');


insert into 表名(列1,...) values(值1,...),(值1,...)...;
例：
insert into students(name) values('杨康'),('杨过'),('小龙女');
```



##### 修改

> 格式: **UPDATE** ***tbname\*** **SET col1={expr1|DEFAULT} [,col2={expr2|default}]...[where 条件判断]**

```sql
update 表名 set 列1=值1,列2=值2... where 条件
例：
update students set gender=0,hometown='北京' where id=5;
```



##### 删除

> **DELETE FROM tbname [where 条件判断]**

```sql
delete from 表名 where 条件
例：
delete from students where id=5;
```

- 逻辑删除，本质就是修改操作

```sql
update students set isdelete=1 where id=1;
```
