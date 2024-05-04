## 第十二章：MySQL 高级应用



### 视图

##### 1. 问题

对于复杂的查询，往往是有多个数据表进行关联查询而得到，如果数据库因为需求等原因发生了改变，为了保证查询出来的数据与之前相同，则需要在多个地方进行修改，维护起来非常麻烦

解决办法：定义视图



##### 2. 视图是什么

通俗的讲，视图就是一条SELECT语句执行后返回的结果集。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。

视图是对若干张基本表的引用，一张虚表，查询语句执行的结果，不存储具体的数据（基本表数据发生了改变，视图也会跟着改变）；

方便操作，特别是查询操作，减少复杂的SQL语句，增强可读性



##### 3. 定义视图

建议以v_开头

```sql
create view 视图名称 as select语句;
```



##### 4. 查看视图

查看表会将所有的视图也列出来

```sql
show tables;
```



##### 5. 使用视图

视图的用途就是查询

```sql
select * from v_stu_score;
```



##### 6. 删除视图

```sql
drop view 视图名称;
例：
drop view v_stu_sco;
```



##### 7. 视图的实际使用

```sql
-- 查询陕西省之下所有的城市
select city.* from tb_areas as city inner join tb_areas as province on city.pid=province.aid where province.atitle='山西省';

-- 创建视图
create view v_city_shanxi as select city.* from tb_areas as city inner join tb_areas as province on city.pid=province.aid where province.atitle='山西省';

-- 使用视图
select * from v_city_shanxi;
```



##### 8. 视图的作用

1. 提高了重用性，就像一个函数
2. 对数据库重构，却不影响程序的运行
3. 提高了安全性能，可以对不同的用户
4. 让数据更加清晰



### 事务

##### 1. 为什么要有事务

事务广泛的运用于订单系统、银行系统等多种场景

例如：

> A用户和B用户是银行的储户，现在A要给B转账500元，那么需要做以下几件事：
>
> 1. 检查A的账户余额>500元；
> 2. A 账户中扣除500元;
> 3. B 账户中增加500元;

正常的流程走下来，A账户扣了500，B账户加了500，皆大欢喜。

那如果A账户扣了钱之后，系统出故障了呢？A白白损失了500，而B也没有收到本该属于他的500。

以上的案例中，隐藏着一个前提条件：A扣钱和B加钱，要么同时成功，要么同时失败。事务的需求就在于此

所谓事务,它是一个操作序列，这些操作要么都执行，要么都不执行，它是一个不可分割的工作单位。

例如，银行转帐工作：从一个帐号扣款并使另一个帐号增款，这两个操作要么都执行，要么都不执行。所以，应该把他们看成一个事务。事务是数据库维护数据一致性的单位，在每个事务结束时，都能保持数据一致性



**事务四大特性(简称ACID)**

- 原子性(Atomicity)
- 一致性(Consistency)
- 隔离性(Isolation)
- 持久性(Durability)

以下内容出自《高性能MySQL》第三版，了解事务的ACID及四种隔离级有助于我们更好的理解事务运作。

下面举一个银行应用是解释事务必要性的一个经典例子。假如一个银行的数据库有两张表：支票表（checking）和储蓄表（savings）。现在要从用户Jane的支票账户转移200美元到她的储蓄账户，那么至少需要三个步骤：

1. 检查支票账户的余额高于或者等于200美元。
2. 从支票账户余额中减去200美元。
3. 在储蓄帐户余额中增加200美元。

上述三个步骤的操作必须打包在一个事务中，任何一个步骤失败，则必须回滚所有的步骤。

可以用START TRANSACTION语句开始一个事务，然后要么使用COMMIT提交将修改的数据持久保存，要么使用ROLLBACK撤销所有的修改。事务SQL的样本如下：

1. start transaction;
2. select balance from checking where customer_id = 10233276;
3. update checking set balance = balance - 200.00 where customer_id = 10233276;
4. update savings set balance = balance + 200.00 where customer_id = 10233276;
5. commit;

一个很好的事务处理系统，必须具备这些标准特性：

- 原子性（atomicity）

> 一个事务必须被视为一个不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说，不可能只执行其中的一部分操作，这就是事务的原子性

- 一致性（consistency）

> 数据库总是从一个一致性的状态转换到另一个一致性的状态。（在前面的例子中，一致性确保了，即使在执行第三、四条语句之间时系统崩溃，支票账户中也不会损失200美元，因为事务最终没有提交，所以事务中所做的修改也不会保存到数据库中。）

- 隔离性（isolation）

> 通常来说，一个事务所做的修改在最终提交以前，对其他事务是不可见的。（在前面的例子中，当执行完第三条语句、第四条语句还未开始时，此时有另外的一个账户汇总程序开始运行，则其看到支票帐户的余额并没有被减去200美元。）

- 持久性（durability）

> 一旦事务提交，则其所做的修改会永久保存到数据库。（此时即使系统崩溃，修改的数据也不会丢失。）



##### 2. 事务命令

表的引擎类型必须是innodb类型才可以使用事务，这是mysql表的默认引擎

查看表的创建语句，可以看到engine=innodb

```sql
-- 选择数据库
use python_test_1;
-- 查看students表
show create table students;
```

开启事务，命令如下：

- 开启事务后执行修改命令，变更会维护到本地缓存中，而不维护到物理表中

```sql
begin;
或者
start transaction;
```

提交事务，命令如下

- 将缓存中的数据变更维护到物理表中

```sql
commit;
```

回滚事务，命令如下：

- 放弃缓存中变更的数据

```sql
rollback;
```

注意

1. 修改数据的命令会自动的触发事务，包括insert、update、delete
2. 而在SQL语句中有手动开启事务的原因是：可以进行多次数据的修改，如果成功一起成功，否则一起会滚到之前的数据



##### 3. 事务提交

- 为了演示效果，需要打开两个终端窗口，使用同一个数据库，操作同一张表



> 连接

- 终端1：查询班级信息

```sql
select * from classes;
```



> 增加数据

- 终端2：开启事务，插入数据

```sql
begin;
insert into classes(name) values('python_03期');
```

- 终端2：查询数据，此时有新增的数据

```sql
select * from classes;
```



> 查询

- 终端1：查询数据，发现并没有新增的数据

```sql
select * from classes;
```



> 提交

- 终端2：完成提交

```sql
commit;
```



> 查询

- 终端1：查询，发现有新增的数据

```sql
select * from classes;
```



##### 5. 事务回滚

- 为了演示效果，需要打开两个终端窗口，使用同一个数据库，操作同一张表



> 连接

- 终端1

```sql
select * from classes;
```



> 增加数据

- 终端2：开启事务，插入数据

```sql
begin;
insert into classes(name) values('python_04期');
```

- 终端2：查询数据，此时有新增的数据

```sql
select * from classes;
```



> 查询

- 终端1：查询数据，发现并没有新增的数据

```sql
select * from classes;
```



> 回滚

- 终端2：完成回滚

```sql
rollback;
```



> 查询

- 终端1：查询数据，发现没有新增的数据

```sql
select * from classes;
```



### PyMySQL 的使用

##### 1. Python数据库API

Python数据库API是为方便统一操作数据库而提出的一个标准接口，也称为DB-API。

在没有Python DB-API之前，各数据库之间的应用接口非常混乱，实现各不相同。如果项目需要更换数据库，就需要进行大量修改，非常不便。

Python DB-API的出现就是为了解决这些问题。Python所有数据库接口程序都在一定程度上遵守Python DB-API规范。

DB-API定义了一系列必需的对象和数据库存取方式，以便为各种各样的底层数据库系统和数据库接口程序提供一致的访问接口。由于DB-API为不同数据库提供了一致的访问接口，因此在不同的数据库之间移植代码成为一件轻松的事情。DB-API规范包括全局变量、异常、连接、游标和类型等基本概念，下面我们逐一进行介绍。



##### 2. 全局变量

DB-API规范规定数据库接口模块必须实现一些全局属性以保证兼容性。Python提供了3个描述数据库模块特性的全局变量。

| 变量名       |            用途             |
| ------------ | :-------------------------: |
| apilevel     | 所使用的Python DB-API的版本 |
| threadsafety |     模块的线程安全等级      |
| paramstyle   |  在SQL查询中使用的参数风格  |

apilevel指的是API级别，是一个字符串常量，表示这个DB-API模块所兼容的DB-API最高的版本号。例如，若版本号是1.0、2.0，则最高版本是2.0，如果未定义，就默认是1.0。



线程安全等级threadsafety是一个整数，取值范围如下：

- 0表示不支持线程安全，多个线程不能共享此模块。
- 1表示初级线程安全支持，线程可以共享模块，但不能共享连接。
- 2表示中级线程安全支持，线程可以共享模块和连接，但不能共享游标。
- 3表示完全线程安全支持，线程可以共享模块、连接及游标。



paramstyle（参数风格）表示执行多次类似查询时，参数如何被拼接到SQL查询中。值format表示标准字符串格式化（使用基本的格式代码），可以在参数中进行拼接的地方插入%s。值pyformt表示扩展的格式代码，用于字典拼接，如%（foo）。除了Python风格之外，还有3种接合方式：qmark的意思是使用问号，numeric表示使用:1或:2格式的字段（数字表示参数的序号），而named表示:foobar这样的字段。其中，foobar为参数名。



##### 3. 异常

为了能尽可能准确地处理错误，DB-API中定义了一些异常。这些异常被定义在层次结构中，可以通过一个except块捕捉多种异常。

|     异常      | 超类 |        描述        |
| :-----------: | :--: | :----------------: |
| StandardError |      | 所有异常的泛型基类 |
|Warning|StandardError|在非致命错误发生时引发|
|Error|StandardError|错误异常基类|
|InterfaceError|Error|数据库接口错误|
|DataBaseEroor|Error|与数据库相关的错误基类|
|DataError|DatabaseError|处理数据时出错|
|OperationalError|DatabaseError|数据库执行命令时出错|
|IntegrityError|DatabaseError|数据完整性错误|
|InternalError|DatabaseError|数据库内部出错|
|ProgrammingError|DatabaseError|SQL 执行失败|
|NotSupportedError|DatabaseError|试图执行数据库不支持的特性|



##### 4. 连接与游标

为了使用基础数据库系统，首先必须连接它。

连接数据库需要使用具有恰当名称的connect函数。

该函数有多个参数，具体使用哪个参数需要根据数据库类型进行选择。DB-API定义了下表所示的参数作为准则（建议将这些参数按表中给定的顺序传递），参数类型为字符串类型。



> connect 函数常用参数

| 参数名   | 描述                                 | 是否可选 |
| :------: | :----------------------------------: | :------: |
| dsn      | 数据源名称, 给出该参数表示数据库依赖 | 否       |
| user     | 用户名                               | 是       |
| password | 用户密码                             | 是       |
| host     | 主机名                               | 是       |
|database|数据库名称|是|



connect函数返回连接对象，这个连接对象表示目前和数据库的会话。连接对象支持的方法如下表所示。

> 连接对象支持的方法

|   方法名   |                   描述                   |
| :--------: | :--------------------------------------: |
|  close()   |  关闭连接后, 连接对象和它的游标均不可用  |
|  commit()  | 如果支持就提交挂起的事务, 否则不做任何事 |
| rollback() |        回滚挂起的事务(可能不可用)        |
|cursor()|返回链接的游标对象|

rollback方法可能不可用，因为不是所有数据库都支持事务。

commit方法总是可用的，不过如果数据库不支持事务，它就没有任何作用。

cursor方法指游标对象。通过游标执行SQL查询并检查结果。游标比连接支持更多方法，而且在程序中更好用。



> 游标对象方法

|          名称          |                       描述                       |
| :--------------------: | :----------------------------------------------: |
| callproc(func[, args]) | 使用给定的名称和参数(可选)调用已命名的数据库程序 |
|        close()         |              关闭游标后，游标不可用              |
|  execute(op[, args])   |           执行 SQL 操作, 可能使用参数            |
| executemany(op, args)  |         对序列中的每个参数执行 SQL 操作          |
|       fetchone()       |     把查询结果集中的下一行保存为序列 或 None     |
|   fetchmany([size])    |    获取查询结果集的多行, 默认尺寸为 arraysize    |
|       fetchall()       |           将所有 (剩余) 行作为结果序列           |
|       nextset()        |          调至下一个可用的结果集 (可选)           |
|  setinputsizes(sizes)  |              为参数预先定义内存区域							|
|setoutputsize(size[, col])|为获取大数据值设定缓冲区尺寸|



> 游标对象特性
|    名称     |             描述             |
| :---------: | :--------------------------: |
|  arraysize  | fetchmany 中返回的行数, 只读 |
| description |    结果列描述的序列, 只读    |
|  rowcount   |      结果中的行数, 只读      |

游标对象最重要的属性是execute\*()和fetch\*()方法。所有对数据库服务器的请求都由这两个方法完成。对fetchmany()方法来说，设置一个合理的arraysize属性很有用。当然，在不需要时最好关掉游标对象。



##### 5. 类型

每一个插入数据库中的数据都对应一个数据类型，每一列数据对应同一个数据类型，不同列对应不同的数据类型。

在操作数据库的过程中，为了能够正确与基础SQL数据库进行数据交互操作，DB-API定义了用于特殊类型和值的构造函数及常量，所有模块都要求实现下表所示的构造函数和特殊值。

|       构造函数和特殊值的名称        |                描述                |
| :---------------------------------: | :--------------------------------: |
|          Date(yr, mo, dy)           |             日期值对象             |
|         Time(hr, min, sec)          |             时间值对象             |
| Timestamp(yr, mo, dy, hr, min, sec) |             时间戳对象             |
|        DateFromTicks(ticks)         |     创建自新纪元以来秒数的对象     |
|        TimeFromTicks(ticks)         |  创建自新纪元以来秒数的时间值对象  |
|      TimestampFromTicks(ticks)      | 创建自新纪元以来秒数的时间戳值对象 |
|           Binary(string)            |      对应二进制长字符串值对象      |
|               STRING                |    描述字符串列对象, 如 VARCHAR    |
|               BINARY                |  描述二进制长列对象, 如 RAW, BLOB  |
|               NUMBER                |           描述数字列对象           |
|              DATETIME               |         描述日期时间列对象         |
|ROWID|描述 row ID 列对象|

注：新纪元指1970-01-01 00:00:01 utc时间



##### 6. 数据库操作

前面我们介绍了数据库的基本概念，本节具体介绍数据库的连接及增、删、改、查操作。

下面的示例数据库为TEST，表名为EMPLOYEE，EMPLOYEE表字段为FIRST_NAME、LAST_NAME、AGE、SEX、INCOME和CREATE_TIME。

连接数据库TEST使用的用户名为root，密码为root。

在系统上已经安装了Python PyMySQL模块。`pip install pymysql`



> 数据库连接

```python
import pymysql


def db_connect():
    # 打开数据库连接
    db = pymysql.connect(host="localhost", user="root", password="root", db="test")
    # 使用cursor()方法创建一个游标对象cursor
    cursor = db.cursor()
    # 使用execute()方法执行SQL查询
    cursor.execute("SELECT VERSION()")
    # 使用 fetchone() 方法获取单条数据
    data = cursor.fetchone()
    # data返回值为一个元组
    # print(data)
    print(f"Database version : {data[0]} ")
    # 关闭数据库连接
    db.close()


def main():
    db_connect()


if __name__ == "__main__":
    main()

```



> 创建数据库表

```python
import pymysql


def create_table():
    db = pymysql.connect(host="localhost", user="root", password="root", db="test")
    # 使用 cursor() 方法创建一个游标对象cursor
    cursor = db.cursor()
    # 使用 execute() 方法执行 SQL，如果表存在就删除
    cursor.execute("drop table if exists employee")
    # 使用预处理语句创建表
    sql = """
        create table employee (            
            first_name varchar(20) not null,            
            last_name varchar(20),            
            age int,            
            sex varchar(1),            
            income float,            
            create_time datetime
            );
            """
    try:
        cursor.execute(sql)
        print("CREATE TABLE SUCCESS.")
    except Exception as ex:
        print(f"CREATE TABLE FAILED,CASE:{ex}")
    finally:
        # 关闭数据库连接
        db.close()


def main():
    create_table()


if __name__ == "__main__":
    main()
```



> 数据库插入

```python
import pymysql
import datetime


def insert_record():
    db = pymysql.connect(host='localhost', user='root', password='root', db='test')

    # 获取游标
    cursor = db.cursor()

    # SQL 插入语句
    sql = "insert into employee (first_name, last_name, age, sex, income, create_time) values " \
          "('%s', '%s', %d, '%s', %d, '%s')" % ('小', '王', 22, '男', 30000, datetime.datetime.now())
    # 执行 SQL 语句
    try:
        cursor.execute(sql)
        # 提交到数据库执行
        db.commit()
        print('数据插入成功...')
    except Exception as e:
        print(f'数据插入失败: {e}')
        # 如果发生错误就回滚
        db.rollback()
    finally:
        # 关闭数据库连接
        db.close()


if __name__ == '__main__':
    insert_record()

```



> 数据库查询

Python查询MySQL使用fetchone()方法获取单条数据，使用fetchall()方法获取多条数据。

- fetchone()：该方法获取下一个查询结果集。结果集是一个对象。
- fetchall()：接收全部返回结果行。
- rowcount：这是一个只读属性，返回执行execute()方法后影响的行数。

```python
import pymysql


def query_data():
    # 打开数据库连接
    db = pymysql.connect(host='localhost', user='root', password='root', db='test')
    # 获取游标
    cursor = db.cursor()
    # 查询语句
    sql = "select * from employee where income > %d" % 10000
    try:
        cursor.execute(sql)
        # 获取所有记录列表
        results = cursor.fetchall()
        for row in results:
            first_name = row[0]
            last_name = row[1]
            age = row[2]
            sex = row[3]
            income = row[4]
            create_time = row[5]
            # 打印结果
            print(f'first_name: {first_name}, last_name: {last_name}, age: {age}, sex: {sex}, income: {income}, '
                  f'create_time: {create_time}')
    except Exception as e:
        print(f'查询错误: {e}')
    finally:
        db.close()


if __name__ == '__main__':
    query_data()

```



> 数据库更新

下面的示例将emplyee表中sex字段值为'男'的记录的age字段值增加1

```python
import pymysql


def update_table():
    db = pymysql.connect(host='localhost', user='root', password='root', db='test')
    cursor = db.cursor()
    sql = "update employee set age=age+1 where sex='%s'" % '男'
    try:
        cursor.execute(sql)
        db.commit()
        print('数据更新成功...')
    except Exception as e:
        print(f'更新失败: {e}')
        db.rollback()
    finally:
        db.close()


if __name__ == '__main__':
    update_table()

```



> 数据库删除

删除操作用于删除数据表中对应的数据。

- 下面演示删除数据表employee中age大于22的所有数据

```python
import pymysql


def delete_record():
    db = pymysql.connect(host='localhost', user='root', password='root', db='test')
    cursor = db.cursor()
    sql = "delete from employee where age > %d" % 22
    try:
        cursor.execute(sql)
        db.commit()
        print('数据删除成功...')
    except Exception as e:
        print(f'数据删除失败: {e}')
        db.rollback()
    finally:
        db.close()


if __name__ == '__main__':
    delete_record()

```



### 查询优化 - 索引

##### 1. 思考

> 在图书馆中是如何找到一本书的？

一般的应用系统对比数据库的读写比例在10:1左右(即有10次查询操作时有1次写的操作)，

而且插入操作和更新操作很少出现性能问题，

遇到最多、最容易出问题还是一些复杂的查询操作，所以查询语句的优化显然是重中之重



##### 2. 解决办法

当数据库中数据量很大时，查找数据会变得很慢

优化方案：索引



##### 3. 索引是什么

索引是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分)，它们包含着对数据表里所有记录的引用指针。

更通俗的说，数据库索引好比是一本书前面的目录，能加快数据库的查询速度



##### 4. 索引的使用

- 查看索引

    ```sql
    show index from 表名;
    ```

    

- 创建索引

    - 如果指定字段是字符串，需要指定长度，建议长度与定义字段时的长度一致
    - 字段类型如果不是字符串，可以不填写长度部分

    ```sql
    create index 索引名称 on 表名(字段名称(长度))
    ```

    

- 删除索引：

    ```sql
    drop index 索引名称 on 表名;
    ```



##### 5. 索引代码示例

> 创建测试表test_index

```sql
create table test_index(title varchar(10));
```



> 使用python程序（ipython也可以）通过pymsql模块 向表中加入十万条数据

```python
from pymysql import connect

def main():
    # 创建Connection连接
    conn = connect(host='localhost', user='root', password='root', database='test')
    # 获得Cursor对象
    cursor = conn.cursor()
    # 插入10万次数据
    for i in range(100000):
        cursor.execute("insert into test_index values('ha-%d')" % i)
    # 提交数据
    conn.commit()

if __name__ == "__main__":
    main()
```



> 查询

- 开启运行时间监测

```sql
set profiling=1;
```

- 查找第1万条数据ha-99999

```sql
select * from test_index where title='ha-99999';
```

- 查看执行的时间

```sql
show profiles;
```



- 为表title_index的title列创建索引

```sql
create index title_index on test_index(title(10));
```

- 执行查询语句

```sql
select * from test_index where title='ha-99999';
```

- 再次查看执行的时间

```sql
show profiles;
```



##### 6. 关于索引的使用注意点

要注意的是，建立太多的索引将会影响更新和插入的速度，因为它需要同样更新每个索引文件。

对于一个经常需要更新和插入的表格，就没有必要为一个很少使用的where字句单独建立索引了，对于比较小的表，排序的开销不会很大，也没有必要建立另外的索引。

建立索引会占用磁盘空间。