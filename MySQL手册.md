<center> <h1>数据库操作</h1> </center>
## 创建数据库
##### 语法

```SQL
create database [if not exists] <数据库名>
[[default] character set <字符集名> [default] collate <校对规则名>];
```

##### 备注
- `[]` 可选
- `[if not exists]` 如果数据库不存在，就创建它，否则不创建
- `[default] character set <字符集名>` 指定数据库的默认字符集。
- `[default] collate <校对规则名>` 指定字符集的默认校对规则。

> MySQL 的字符集（character）和校对规则（collate）两个不同的概念：字符集是用来定义 MySQL 存储字符串的方式，校对规则定义了比较字符串的方式，解决排序和字符分组的问题。
字符集和校对规则是一对多的关系，每个字符集至少对应一个校对规则，MySQL 支持 39 种字符集的将近 200 种校对规则。

##### 示例
```SQL
create database if not exists Test;          -- 如果 test 数据库不存在，就创建它
```
```SQL
-- 创建 test 数据库，指定编码格式为 utf-8 ，校对规则为 utf8_chinese_ci
create database if not exists test default character set utf8 default collate utf8_chinese_ci;
```

##### 查看库数据库的定义声明
`show create database <数据库名>;`<br>
输出
```SQL
+--------------+-----------------------------------------------------+
| Database     | Create Database                                     |
+--------------+-----------------------------------------------------+
| 数据库名      | CREATE DATABASE `数据库名` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+--------------+-----------------------------------------------------+
1 row in set (0.05 sec)
```
> 为防止字符混乱的情况发生，MySQL 有时需要在创建数据库时明确指定字符集；在中国大陆地区，常用的字符集有 utf8 和 gbk。
>> - utf8 能够存储全球的所有字符，在任何国家都可以使用，默认的校对规则为 utf8_general_ci，对于中文可以使用 utf8_general_ci。
>>- gbk 只能存储汉语涉及到的字符，不具有全球通用性，默认的校对规则为 gbk_chinese_ci。

<br>
<br>

---

<br>

## 查看数据库

##### 语法
`show databases [like '数据库名']`
- `[like '数据库名']` 查看指定的数据库，可以加通配符，`%` 匹配多个字符，`_` 匹配一个字符

##### 显示所有数据库
```SQL
show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
6 row in set (0.22 sec)
```

##### 查看指定的数据库
```SQL
show databases like 'test';

+--------------------+
| Database           |
+--------------------+
| test               |
+--------------------+
6 row in set (0.22 sec)
```

##### 查看名字相似的数据库
```SQL
show databases like 'test%';

+--------------------+
| Database           |
+--------------------+
| test               |
| test01             |
| test02             |
+--------------------+
6 row in set (0.22 sec)
```

<br>
<br>

---

<br>

## 修改数据库

##### 语法
在 MySQL 中，可以使用 `alter database` 或 `alter schema` 语句来修改已经被创建或者存在的数据库的相关参数。
```SQL
alter database [数据库名] { [default] character set <字符集名> | [default] collate <校对规则名>};
```
- `alter database` 用于更改数据库的全局特性。这些特性存储在数据库目录的 db.opt 文件中。
- 使用 `alter database` 需要获得数据库 `alter` 权限。
- 数据库名称可以忽略，此时语句对应于默认数据库。
- `character set` 子句用于更改默认的数据库字符集。

<br>
<br>

---

<br>

## 删除数据库

##### 语法
```
drop database <数据库名>;
```
##### 示例
```SQL
drop database test;          -- 删除 test 数据库
```

<br>
<br>

---

<br>

## 选择使用数据库

##### 语法
```SQL
use <数据库名>;
```
##### 示例
```SQL
use test;          -- 使用 test 数据库
```

<br>
<br>

---

<br>

## 存储引擎
MySQL 支持多种类型的数据库引擎，可分别根据各个引擎的功能和特性为不同的数据库处理任务提供各自不同的适应性和灵活性。在 MySQL 中，可以利用 SHOW ENGINES 语句来显示可用的数据库引擎和默认引擎。

MySQL 提供了多个不同的存储引擎，包括处理事务安全表的引擎和处理非事务安全表的引擎。在 MySQL 中，不需要在整个服务器中使用同一种存储引擎，针对具体的要求，可以对每一个表使用不同的存储引擎。

MySQL 5.7 支持的存储引擎有 `InnoDB`、`MyISAM`、`Memory`、`Merge`、`Archive`、`Federated`、`CSV`、`BLACKHOLE` 等。可以使用`SHOW ENGINES`语句查看系统所支持的引擎类型，结果如图所示。

![image](http://c.biancheng.net/uploads/allimg/190222/4-1Z2221K006125.gif)

Support 列的值表示某种引擎是否能使用，`YES`表示可以使用，`NO`表示不能使用，`DEFAULT`表示该引擎为当前默认的存储引擎。

### 选择 MySQL 存储引擎
不同的存储引擎都有各自的特点，以适应不同的需求，如表所示。为了做出选择，首先要考虑每一个存储引擎提供了哪些不同的功能。


| 功能	       | MylSAM | MEMORY | InnoDB | Archive |
|--------------|--------|--------|--------|---------|
|存储限制	   | 256TB	|  RAM	 |  64TB  |  None   |
|支持事务      |  No    |  No    |	Yes   |	 No     |
|支持全文索引  |  Yes   |  No    |	No    |	 No     |
|支持树索引    |  Yes   |  Yes   |	Yes   |	 No     |
|支持哈希索引  |  No    |  Yes   |	No    |	 No     |
|支持数据缓存  |  No    |  N/A   |	Yes   |	 No     |
|支持外键      |  No    |  No    |	Yes   |	 No     |

可以根据以下的原则来选择 MySQL 存储引擎：
- 如果要提供提交、回滚和恢复的事务安全（ACID 兼容）能力，并要求实现并发控制，InnoDB 是一个很好的选择。
- 如果数据表主要用来插入和查询记录，则 MyISAM 引擎提供较高的处理效率。
- 如果只是临时存放数据，数据量不大，并且不需要较高的数据安全性，可以选择将数据保存在内存的 MEMORY - 引擎中，MySQL 中使用该引擎作为临时表，存放查询的中间结果。
- 如果只有 INSERT 和 SELECT 操作，可以选择Archive 引擎，Archive 存储引擎支持高并发的插入操作，但是本身并不是事务安全的。Archive 存储引擎非常适合存储归档数据，如记录日志信息可以使用 Archive 引擎

提示：使用哪一种引擎要根据需要灵活选择，一个数据库中多个表可以使用不同的引擎以满足各种性能和实际需求。使用合适的存储引擎将会提高整个数据库的性能。==

### 默认存储引擎
InnoDB 是系统的默认引擎，支持可靠的事务处理。

使用下面的语句可以修改数据库临时的默认存储引擎

```SQL
set default_storage_engine=<存储引擎名>
```
例如，将 MySQL 数据库的临时默认存储引擎修改为 MyISAM，输入的 SQL 语句和运行结果如图所示。

![image](http://c.biancheng.net/uploads/allimg/190222/4-1Z2221K501L1.gif)

此时，可以发现 MySQL 的默认存储引擎已经变成了 MyISAM。但是当再次重启客户端时，默认存储引擎仍然是 InnoDB。


<br><br><br><br>

<center> <h1>数据类型</h1> </center>
## 简介
在 MySQL 中常见的数据类型如下：
1) 整数类型
包括 TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT，浮点数类型 FLOAT 和 DOUBLE，定点数类型 DECIMAL。
2) 日期/时间类型
包括 YEAR、TIME、DATE、DATETIME 和 TIMESTAMP。
3) 字符串类型
包括 CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM 和 SET 等。
4) 二进制类型
包括 BIT、BINARY、VARBINARY、TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。


<br>
<br>

---

<br>

## 整数类型


| 类型名称 | 说明	|存储需求
|-|-|-|
| tinyint |	很小的整数 |	1个字节 |
| smallint |	小的整数 |	2个宇节 |
| mediumint |	中等大小的整数 |	3个字节 |
| int (integhr) |	普通大小的整数 |	4个字节 |
| bigint	| 大整数 |	8个字节 |

根据占用字节数可以求出每一种数据类型的取值范围。例如，tinyint 需要 1 个字节（8bit）来存储，那么 tinyint 无符号数的最大值为 128-1，即 255；tinyint 有符号数的最大值为 127-1，即 127。其他类型的整数的取值范围计算方法相同，如下表所示。


| 类型名称 |说明|	存储需求|
|-|-|-|
|tinyint|	-128〜127|	0 〜255|
|smallint|	-32768〜32767|	0〜65535|
|mediumint|	-8388608〜8388607	|0〜16777215|
| int (integhr)|	-2147483648〜2147483647|	0〜4294967295|
| bigint|	-9223372036854775808〜9223372036854775807|	0〜18446744073709551615|

>提示：显示宽度和数据类型的取值范围是无关的。显示宽度只是指明 MySQL 最大可能显示的数字个数，数值的位数小于指定的宽度时会由空格填充。如果插入了大于显示宽度的值，只要该值不超过该类型整数的取值范围，数值依然可以插入，而且能够显示出来。例如，year 字段插入 19999，当使用 SELECT 查询该列值的时候，MySQL 显示的将是完整的带有 5 位数字的 19999，而不是 4 位数字的值。
其他整型数据类型也可以在定义表结构时指定所需的显示宽度，如果不指定，则系统为每一种类型指定默认的宽度值。


<br>
<br>

---

<br>

## 浮点类型


| 类型名称 | 说明 | 存储需求 |
|-|-|-|
| float | 单精度浮点数 | 4 个字节 |
| double | 双精度浮点数 | 8 个字节 |
| decimal(M, D)，DEC | 压缩的“严格”定点数 (定点型) | M+2 个字节|

浮点类型和定点类型都可以用`(M, D)`来表示，其中`M`称为精度，表示总共的位数；`D`称为标度，表示小数的位数。

浮点数类型的取值范围为 M（1～255）和 D（1～30，且不能大于 M-2），分别表示显示宽度和小数位数。M 和 D 在 FLOAT 和double 中是可选的，float 和 double 类型将被保存为硬件所支持的最大精度。decimal 的默认 D 值为 0、M 值为 10。

decimal 类型不同于 float 和 double。double 实际上是以字符串的形式存放的，decimal 可能的最大取值范围与 double 相同，但是有效的取值范围由 M 和 D 决定。如果改变 M 而固定 D，则取值范围将随 M 的变大而变大。

从上表中可以看到，decimal 的存储空间并不是固定的，而由精度值 M 决定，占用 M+2 个字节。

FLOAT 类型的取值范围如下：
- 有符号的取值范围：-3.402823466E+38～-1.175494351E-38。
- 无符号的取值范围：0 和 -1.175494351E-38～-3.402823466E+38。

DOUBLE 类型的取值范围如下：
- 有符号的取值范围：-1.7976931348623157E+308～-2.2250738585072014E-308。
- 无符号的取值范围：0 和 -2.2250738585072014E-308～-1.7976931348623157E+308。

<br>
<br>

---

<br>

## 日期和时间

| 类型名称 | 日期格式 |	日期范围 | 存储需求 |
|-|-|-|-|
|year|	YYYY|	1901 ~ 2155|	1 个字节|
|time|	HH:MM:SS|	-838:59:59 ~ 838:59:59|	3 个字节|
|date|	YYYY-MM-DD|	1000-01-01 ~ 9999-12-3|	3 个字节|
|datetime|	YYYY-MM-DD HH:MM:SS|	1000-01-01 00:00:00 ~ 9999-12-31 23:59:59|	8 个字节|
|timestamp|	YYYY-MM-DD HH:MM:SS|	1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC|	4 个字节|

> 关于日期 可以使用 CURRENT_DATE 或者 NOW()，插入当前系统日期。

<br>
<br>

---

<br>

## 字符串类型

| 类型名称 |	说明 |	存储需求 |
|-|-|-|
|char(M)|	固定长度非二进制字符串 |	M 字节，1<=M<=255|
|varchar(M)|	变长非二进制字符串|	L+1字节，在此，L< = M和 1<=M<=65535|
|tinytext|	非常小的非二进制字符串|	L+1字节，在此，L<2^8|
|text|	小的非二进制字符串|	L+2字节，在此，L<2^16|
|mediumtext|	中等大小的非二进制字符串	|L+3字节，在此，L<2^24|
|longtext|	大的非二进制字符串|	L+4字节，在此，L<2^32|
|enum(值1,值2,值3,...)|	枚举类型，只能有一个枚举字符串值	|1或2个字节，取决于枚举值的数目 (最大值为65535)|
|set(值1,值2,值3,...)|	一个设置，字符串对象可以有零个或 多个SET成员|	1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员）|

> `M` 表示可以为其指定长度

### `char` 和 `varchar` 类型

- char(M) 为固定长度字符串，在定义时指定字符串列长。当保存时，在右侧填充空格以达到指定的长度。M 表示列的长度，范围是 0～255 个字符。
- varchar(M) 是长度可变的字符串，varchar的最大实际长度由最长的行的大小和使用的字符集确定，而实际占用的空间为字符串的实际长度加 1,范围是 0～65535 个字符。


|插入值|	CHAR(4)|	存储需求|	VARCHAR(4)|	存储需求|
|-|-|-|-|-|
|' '|	'&nbsp;&nbsp;&nbsp;&nbsp;'	|4字节|	' '	|1字节|
|'ab'|	'ab  '|	4字节|	'ab'|	3字节|
|'abc'|	'abc '|	4字节|	'abc'|	4字节|
|'abcd'|	'abcd'|	4字节|	'abcd'|	5字节|
|'abcdef'|	'abcd'|	4字节|	'abcd'|	5字节|

### `text` 类型
`text` 列保存非二进制字符串，如文章内容、评论等。当保存或查询 `text` 列的值时，不删除尾部空格。

`text` 类型分为 4 种：`tinytext`、`text`、`mediumtext` 和 `longtext`。不同的 `text` 类型的存储空间和数据长度不同。

### `enum` 类型
- `enum` 类型的字段在取值时，能在指定的枚举列表中获取，而且一次只能取一个。如果创建的成员中有空格，尾部的空格将自动被删除。
- `enum`值在内部用整数表示，每个枚举值均有一个索引值；列表值所允许的成员值从 1 开始编号，MySQL 存储的就是这个索引编号。

###  `set`类型
- 与 `enum` 类型相同，`set` 值在内部用整数表示，列表中每个值都有一个索引编号。当创建表时，`set` 成员值的尾部空格将自动删除。
- 但与 `enum` 类型不同的是，`enum` 类型的字段只能从定义的列值中选择一个值插入，而 SET 类型的列可从定义的列值中选择多个字符的联合。

<br>
<br>

---

<br>

## 二进制数据类型


| 类型名称 | 说明 | 存储需求 |
|-|-|-|
| bit(M)	 | 位字段类型 | 大约 (M+7)/8 字节|
| binary(M) | 固定长度二进制字符串 |	M 字节|
| varbinary (M) |	可变长度二进制字符串 |	M+1 字节|
| tinyblob(M)|	非常小的BLOB|	L+1 字节，在此，L<2^8|
| blob (M)|	小 BLOB |	L+2 字节，在此，    L<2^16|
| mediumblob (M) |	中等大小的BLOB |	L+3 字节，在此，L<2^24|
| longblob (M) |	非常大的BLOB |	L+4 字节，在此，L<2^32|

### `BLOB` 类型
`blob` 是一个二进制的对象，用来存储可变数量的数据。

| 数据类型 |	存储范围|
|-|-|
| tinyblob | 最大长度为255 (28-1)字节 |
| blob	| 最大长度为65535 (216-1)字节 |
| mediumblob |	最大长度为16777215 (224-1)字节 |
| longblob |	最大长度为4294967295或4GB (231-1)字节 |

<br>
<br>

---

<br>

## 创建数据表
```SQL
create table <表名字>(
    列名1 类型,
    列名n 类型
);
```
## 查看数据表
```SQL
show tables;
```
## 查看表结构(表信息)
```SQL
describe <表名>;

desc <表名>;    -- describe 简写
```

**示例输出 :**

```SQL
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |                     
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(255) | YES  |     | NULL    |                |
| age   | int(11)      | YES  |     | NULL    |                |                     
+-------+--------------+------+-----+---------+----------------+ 
```
- `Field` 字段名/列名
- `Type` 数据类型
- `Null` 是否允许为空值
- `Key` 键类型(主键`primary key` 外键`foregin key`)
- `Default` 是否有默认值
- `Extra` 附加信息 ( 主键自增`auto_increment`,非主键列不能重复`unique` )

## 查看创建表的`SQL`语句
```SQL
show create table <表名>\G;
```
- `\G` 格式化显示

**示例输出 :**

```SQL
*************************** 1. row ***************************
       Table: user
Create Table: CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=61 DEFAULT CHARSET=utf8
1 row in set (0.00 sec)

ERROR:
No query specified
```

<br>
<br>

## 修改数据表
**语法 :**
```SQL
alter table <表名> [修改选项];
```
**添加字段(列)**
```SQL
alter table <表名> add <新字段名> <数据类型> [约束条件] [ first | after 字段名 ];
```
- `first` 将 新添加的字段 设置为 表的 第一个 字段
- `after 字段名` 将新添加的 字段 放在 指定字段 的后面

**修改字段数据类型**
```SQL
alter table <表名> modify <字段名> <数据类型>;
```

**删除字段**
```SQL
alter table <表名> drop <字段名>;
```

**修改字段名称**
```SQL
alter table <表名> change <旧字段名> <新字段名> <新数据类型>;
```

**修改表名**
```SQL
alter table <表名> rename to <新表名>;
```

<br>
<br>

## 删除数据表
```
drop table [if exists] <表名> [,<表名N>]
```
- `if exists` 判断数据表是否存在,加上该参数,就算 表不存在 也不会报错

<br>
<br>

## 主键约束 `primary key`
> 用来 确保 每一行 数据都是 唯一的, 主键可以是表中的某一列,也可以是多列的组合, 多列组合 的主键称为 复合主键

**说明**
- 每个表只能有一个主键
- 主键唯一标识每一行,且不能为空(Null),即一个表中没有相同的两个主键,确保数据唯一性
- 一个列只能在复合主键中出现一次
- 复合主键不能包含不必要的多余列,这是最小化原则

**在创建时设置主键约束**
```SQL
create table <表名>(
    <字段名> <数据类型> primary key auto_increment
);
```
- `auto_increment` 让该列在自增,这样主键列的内容就不会相同

**在 定义完 所有列 后 创建 主键约束**
```SQL
create table <表名>(
    <字段名1> <数据类型>,
    <字段名n> <数据类型>,
    [constraint <约束名>] primary key (字段名1 [,字段名n])          -- 语句后面不能跟自增备注,`auto_increment`要添加在某个具体的列上
);
```
- `()`内可以放多个列名,即复合主键

**在修改表时添加主键约束**
```SQL
alter table <表名> add priary key(列名 [, 列名n]);
```

<br>
<br>

## 外键约束 `foreign key`
**说明**
- 父表必须存在, 父表可以是自己(自参照完整性)
- 必须为父表定义的主键
- 主键不包含空值，但允许在外键中出现空值
- 在父表的表名后面指定列名或列名的组合,这个列或列的组合必须是父表的主键或候选键
- 外键中列的数目必须和父表的主键中列的数目相同
- 外键中列的数据类型必须和父表主键中对应列的数据类型相同

**在创建表时设置外键约束**
```SQL
create table <表名>(
    <字段名> <数据类型>,
    <字段名n> <数据类型>,
    [constraint <约束名>] foreign key(字段1 [,字段n]) references <父表名>(主键列1 [,主键列n])
);
```
**在修改表时添加外键约束**
```SQL
alter table <表名> add constraint <约束名> foreign key(字段1 [,字段n]) references <父表名>(主键列1 [,主键列n])
```
**删除外键约束**
```SQL
alter table <表名> drop foreign key <约束名>
```

<br><br>
## 唯一约束 `unique key`
**说明**
- 使非主键列数据唯一,可以有多个
- 允许空值的存在

**创建表时设置唯一约束**
```SQL
create table <表名>(
    <字段名> <数据类型> unique
);
```
**在修改表时添加唯一约束**
```SQL
alter table <表名> add constraint <约束名> unique(<列名>);
```
**删除唯一约束**
```SQL
alter table <表名> drop index <约束名>;
```

<br><br>
## 检查约束 `check`
**说明**
- 检查插入的数据是否满足条件

**在创建表时设置检查约束**
```SQL
-- 基于 user 表的 检查约束
create table User(
    id int primary key,
    user char(6) not null,
    age tinyint not null,

    check(
        age > 0 and age < 120
    )
);
```
**在修改表时添加检查约束**
```SQL
alter table <表名> add constraint <约束名> check(表达式);
```
**删除检查约束**
```SQL
alter table <表名> drop constraint <约束名>;
```

<br><br>
## 默认值约束 `default`
**在创建表时设置默认值约束**
```SQL
create table <表名>(
    <字段名> <数据类型> default <默认值>
);
```
**在修改表时设置默认值约束**
```SQL
alter table <表名> change column <字段名> <数据类型> default <默认值>;
```
**删除默认值约束**
```SQL
alter table <表名> change column <字段名> <数据类型> default null;
```

<br><br>
## 非空约束 `not null`
**在创建表时设置非空约束**
```SQL
create table <表名>(
    <字段名> <数据类型> default <默认值>
);
```
**在修改表时设置非空约束**
```SQL
alter table <表名> change column <字段名> <数据类型> not null;
```
**删除非空约束**
```SQL
alter table <表名> change column <字段名> <数据类型> null;
```
---
---
---
```SQL
-- 在创建表时添加各种约束

create table <表名>(
    <字段名> <数据类型> primary key auto_increment,      -- 主键自增
    <字段名> <数据类型> unique,                          -- 唯一约束
    <字段名> <数据类型> default <默认值>,                -- 默认值约束
    <字段名> <数据类型> not null,                        -- 非空约束
    
    [constraint <约束名>] foreign key(字段1 [,字段n]) references <父表名>(主键列1 [,主键列n]),
    
    check( 表达式(条件) )
);
```


<br><br><br><br>

<center><h1>数据查询</h1></center>
## 查询`select`
### `select` 语法
```SQL
select  { * | [列1,列n] } form <表1>, <表n>     -- * 所有列
[
    where <表达式>          -- 数据筛选
]

[
    group by <列名>         -- 分组 
    [
        having <expression>
        [
            {<operator> <expression>}
        ]
    ]

[
    order by <列名> [ asc | desc ]      -- 排序    asc升序(默认)  desc降序
]

[  
    LIMIT[开始行,] <总数>     -- 设置显示数据数量, 从第几行开始,显示多少条
]

```

## 查询所有数据
**查询所有字段**
```SQL
select * from <表名>;
```
- `*` 通配符,表示显示所有字段;

**查询指定字段**
```SQL
select 列1,列n from <表名>;
```

**查询数据去重**
```SQL
select distinct * from <表名>
```

**设置别名**
```SQL
select <列> as <列别名> from <表名> as 表别名;

select <列> <列别名> from <表名> <表别名>;
```

**设置数据返回的数量**
```SQL
select * from <表名> limit n;        --显示 n 条数据

select * form <表名> limit n,s       --从 n 行开始,显示 s 条数据
```

**数据排序**
```SQL
select * form <表名> order by <列名> { asc | desc };    -- asc 升序(默认)  ,  desc 降序


-- 基于 User 表 的 数据排序

select * from `User` order by `Age`,`Name` desc; 

select * from `User` order by `Age` asc , `Name` desc;
```

## 条件查询`where`
```SQL
--基于user表的条件查询

select * from `User`
where
    `id` != 1                           -- id 不是 1
    and
    `Name` like '王%'                   -- 姓王的
    and
    `Age` between 18 and 30             -- 年龄在 18 到 30 之间 
    and
    `comment` is not null ;             -- 备注不为空
```

## 映射查询`case`
**将数据 变成自己需要的东西** 
```SQL
--基于user表的映射查询

select 
    `id`,
    `name`,
    `age`,
    case
        when( sex = 0 ) then '男'
        when( sex = 1 ) then '女'
        else '保密'
    end as sex
from `User`
```

**判断运算**
-  `=`,`<`,`<=`,`>=`,`!=` 
-  `[not] like` 像不像, `%` 匹配多个字符, `_` 匹配单个字符
-  `[not] [regexp|rlike]` 正则表达式
-  `[not] between  表达式1 and 表达式2 ` 是不是在两者之间
-  `表达式  is [not] null` 是不是空值

## 多表查询
### 内连接查询 `inner join`
>作用: 返回两个表中 指定列 相同的数据
```SQL
select * from <表名1> inner join <表名2>
on
    <表名1>.<列名> = <表名2>.<列名>


-- 因为 是 返回两个表的相同数据,所以可以直接使用 where 表达式之间 筛选 数据
select * from <表名1>,<表名2>
where
    <表名1>.<列名> = <表名2>.<列名>
```
### 左连接查询(外连接) `left join`
>作用: 返回 `left join` 左边表的 所有数据
```SQL
select * from <表名1> left join <表名2>     -- 表1 的所有数据都会显示
on
    <表名1>.<列名> = <表名2>.<列名>
```

### 右连接查询(外连接) `right join`
>作用: 返回 `right join` 右边表的 所有数据
```SQL
select * from <表名1> right join <表名2>     -- 表2 的所有数据都会显示
on
    <表名1>.<列名> = <表名2>.<列名>
```

## 子查询
> 在 select语句 中 嵌套 select语句

```SQL
-- 基于 User 表的 子查询
select * from `User`
where
    Level in (
        select VipLevel from `VipUser`         -- 查找 `User` 中 等级是 vip 的用户 
    )
```
- 在 `in` 中 查询出的结果只能返回一个字段的数据,因为只有一个字段和其比较
- 子查询可以作为 连接表使用,只需要起个别名

## 分组查询 `group by`
```SQL
group by {<列名> | <表达式> | <位置>} [ asc | desc ]
```
- **列名** 用于分组的列。可以指定多个列，彼此间用逗号分隔
- **表达式** 用于分组的表达式。通常与聚合函数一块使用
    ```SQL
    -- 基于 User 表的 分组表达式
    
    -- 根据 年龄 分组,把相同年龄的用户整合到一个单元格
    select
        age,
        gruop_concat(`name`) as names
    from
        User
    group by
        age;
    ```

- **位置** 一个数字,对应着,select查询的列

## 分组数据过滤 `having`
- 将分组之后的数据,再次过滤
- 该命令是 `group by` 的字句,所以必须紧跟在它后面

```SQL
-- 基于 User 表的分组过滤

-- 将分组之后的数据,筛选出 年龄 大于 18 岁的
select
    age,
    count(`name`) as sum
from
    User
group by age having age > 18 
```

## 正则表达式 `regexp`

选项 |	说明 |	例子 |	匹配值示例
|-|-|-|-|
^ |	匹配文本的开始字符 |	'^b' |匹配以字母 b 开头 的字符串	book、big、banana、 bike
$ |	匹配文本的结束字符|	'st$’| 匹配以 st 结尾的字 符串	test、resist、persist
. |	匹配任何单个字符|	'b.t’| 匹配任何 b 和 t 之间有一个字符	bit、bat、but、bite
* |	匹配零个或多个在它前面的字符 |	'f*n’ | 匹配字符 n 前面有 任意个字符 f	fn、fan、faan、abcn
+ |	匹配前面的字符 1 次或多次 |	'ba+’ | 匹配以 b 开头，后 面至少紧跟一个 a	ba、bay、bare、battle
<字符串> |	匹配包含指定字符的文本 |	'fa’ |	fan、afa、faad
[字符集合] |	匹配字符集合中的任何一个字符 |	'[xz]' | 匹配 x 或者 z	dizzy、zebra、x-ray、 extra
[^] |	匹配不在括号中的任何字符 |	'[^abc]’ | 匹配任何不包 含 a、b 或 c 的字符串	desk、fox、f8ke
字符串{n,} |	匹配前面的字符串至少 n 次 |	b{2} |匹配 2 个或更多 的 b	bbb、 bbbb、 bbbbbbb
字符串{n,m}|	匹配前面的字符串至少 n 次， 至多 m 次 |	b{2,4}| 匹配最少 2 个， 最多 4 个 b	bbb、 bbbb

```
-- 基于 User 表的正则表达式筛选

select * from User where `name` regexp '^王';   -- 查找姓王的用户
```

<br><br><br><br>
## 添加数据 `insert`
**使用 `value` 插入数据**
```SQL
insert into <表名>( [列1,列n] ) value(值1,值n);
```
- `value` 可以插入一条数据
- `values` 可以插入多条数据
```SQL
-- 基于 User 表 插入数据

insert into User() value( 1,"teacherWang",18 );     -- 一次插入一条数据

insert into User() values( 1,"teacherWang",18 ) ,
                         ( 2,"teacherZhao",18 );    -- 一次插入多条数据
                         
insert into User(id,`name`,age) value( 1,"teacherWang" 18 );    -- 在指定的列插入数据
```

**插入查询数据**
```SQL
-- 基于 User 表 插入查询数据
insert into VipUser() select * from User;    -- 将普通用户添加到vip用户
```

## 修改数据 `update`
```SQL
update <表名> set 字段1 = 值, 字段2 = 值 [where 条件];
```
- 不指定 条件 会 修改所有数据
```SQL
-- 基于 User 表的数据修改
update User set name = "teacherWang" , age = 18 where id = 1;       -- 将 id 为 1 的用户的信息进行修改
```

## 删除数据 `delete`
```SQL
delete from <表名> [where 条件] [order by 子句] [limit 子句];
```
- 不指定 条件 会 删除所有数据
```SQL
-- 基于 User 表 的 数据删除
delete from User where id = 1;          -- 将id 为 1 的 用户信息删除
```
- 清空表数据
```SQL
truncate table <表名>;
```
- 刷新自增
```SQL
alter table <表名> auto_increment = 1;
```


<br><br><br><br><br><br>

<center> <h1> 过程 </h1> </center>
> 待补充



















<br><br>
## 运算符详解
### 算数运算符
| 算术运算符 |	说明|
|-|-|
|+|	加法运算|
|-|	减法运算|
|*|	乘法运算|
|/|	除法运算，返回商|
|%|	求余运算，返回余数|

### 比较运算符
| 比较运算符|	说明|
|-|-|
|=	|等于|
<	|小于|
<=	|小于等于|
>	|大于|
>=	|大于等于|
<=>	|安全的等于，不会返回 UNKNOWN|
<> |或!=	不等于|
is null 或 isnull	|判断一个值是否为 null|
is not null	|判断一个值是否不为 null|
least( 值1,值n )	|当有两个或多个参数时，返回最小值|
getatest(值1,值n)	|当有两个或多个参数时，返回最大值|
betwenn and|判断一个值是否落在两个值之间|
in|判断一个值是IN列表中的任意一个值|
not in	|判断一个值不是IN列表中的任意一个值|
like|通配符匹配|
regexp|正则表达式匹配|

>满足条件返回1,否则返回0

### 逻辑运算符

逻辑运算符	 | 说明
|-|-|
not 或者 ! |	逻辑非
and 或者 &&	| 逻辑与
or 或者 `||` |	逻辑或
xor |	逻辑异或

### 位运算
位运算符 | 	说明
| -| -| 
`|`| 	按位或
&| 	按位与
^| 	按位异或
<<| 	按位左移
>>| 	按位右移
~| 	按位取反，反转所有比特


### 优先级
优先级由低到高排列 | 运算符
|-|-|
1|	=(赋值运算）、:=
2|	II、OR
3|	XOR
4|	&&、AND
5|	NOT
6|	BETWEEN、CASE、WHEN、THEN、ELSE
7|	=(比较运算）、<=>、>=、>、<=、<、<>、!=、 IS、LIKE、REGEXP、IN
8|	`|`
9|	&
10|	<<、>>
11|	-(减号）、+
12|	*、/、%
13|	^
14|	-(负号）、〜（位反转）
15|	!


# 记录
#### 日期差  和  时间差
```SQL
日期相减：
MySQL datediff(date1,date2)：两个日期相减 date1 - date2，返回天数。
select datediff('2008-08-08', '2008-08-01'); -- 7
select datediff('2008-08-01', '2008-08-08'); -- -7

MySQL timediff(time1,time2)：两个日期相减 time1 - time2，返回 time 差值。
select timediff('2008-08-08 08:08:08', '2008-08-08 00:00:00'); -- 08:08:08
select timediff('08:08:08', '00:00:00'); -- 08:08:08

注意：timediff(time1,time2) 函数的两个参数类型必须相同


TIMESTAMPDIFF函数，有参数设置，可以精确到天（DAY）、小时（HOUR），
分钟（MINUTE）和秒（SECOND），使用起来比datediff函数更加灵活。对于比较
的两个时间，时间小的放在前面，时间大的放在后面。


select TIMESTAMPDIFF(DAY, '2018-03-20 23:59:00', '2015-03-22 00:00:00');  --相差1天

select TIMESTAMPDIFF(HOUR, '2018-03-20 09:00:00', '2018-03-22 10:00:00');  --相差49小时

select TIMESTAMPDIFF(MINUTE, '2018-03-20 09:00:00', '2018-03-22 10:00:00');  --相差2940分钟

select TIMESTAMPDIFF(SECOND, '2018-03-20 09:00:00', '2018-03-22 10:00:00'  --相差176400秒
```
#### 日期格式化
```SQL
data_format( 日期 , 格式化字符串 )
```
##### 格式化字符参数
| 参数 | 描述 |
|-|-|
|%S,%s | 两位数字形式的秒（ 00,01, . . ., 59）|
|%i| 两位数字形式的分（ 00,01, . . ., 59）|
|%H |两位数字形式的小时，24 小时（00,01, . . ., 23）|
|%h, %I |两位数字形式的小时，12 小时（01,02, . . ., 12）|
|%k| 数字形式的小时，24 小时（0,1, . . ., 23）|
|%l| 数字形式的小时，12 小时（1, 2, . . ., 12）|
|%T| 24 小时的时间形式（hh : mm : s s）|
|%r |12 小时的时间形式（hh:mm:ss AM 或hh:mm:ss PM）|
|%p |AM 或P M|
|%W |一周中每一天的名称（ Sunday, Monday, . . ., Saturday）|
|%a |一周中每一天名称的缩写（ Sun, Mon, . . ., Sat）|
|%d | 两位数字表示月中的天数（ 00, 01, . . ., 31）|
|%e |数字形式表示月中的天数（ 1, 2， . . ., 31）|
|%D |英文后缀表示月中的天数（ 1st, 2nd, 3rd, . . .）|
|%w |以数字形式表示周中的天数（ 0 = Sunday, 1=Monday, . . ., 6=Saturday）|
|%j |以三位数字表示年中的天数（ 001, 002, . . ., 366）|
|% U |周（0, 1, 52），其中Sunday 为周中的第一天|
|%u |周（0, 1, 52），其中Monday 为周中的第一天|
|%M |月名（January, February, . . ., December）|
|%b |缩写的月名（ January, February, . . ., December）|
|%m |两位数字表示的月份（ 01, 02, . . ., 12）|
|%c| 数字表示的月份（ 1, 2, . . ., 12）|
|%Y| 四位数字表示的年份|
|%y |两位数字表示的年份
|%% |直接值“%”|
