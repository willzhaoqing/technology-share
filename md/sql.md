# 数据库入门
市面上很多数据库，目前，主流的关系数据库主要分为以下几类：

1. 商用数据库，例如：Oracle，SQL Server，DB2等；
2. 开源数据库，例如：MySQL，PostgreSQL等；
3. 桌面数据库，以微软Access为代表，适合桌面应用程序使用；
4. 嵌入式数据库，以Sqlite为代表，适合手机应用和桌面程序。

之前有过一篇简单的归纳比对[mongodb-memcache-redis](./mongodb-memcache-redis.md) ，大家有兴趣可以去看看。

这篇主要说的是关系型数据库，我自己的理解：计算机所有的数据都是存储在文件中，库是采用特殊存储和读取方式的一组数据文件集合。本质上还是对文件的操作！

如果要让非计算机人士来听懂，可以解释为药抽屉或图书馆，对不同类型的数据进行分门别类；某个抽屉里只有甘草，另一个就放黄芪……

## 范式
这个概念，在计算机中就叫：数据库（的）设计范式 或 范式。是符合某一种级别的关系模式的集合。构造数据库必须遵循一定的规则。在关系数据库中，这种规则就是范式。关系数据库中的关系必须满足一定的要求，即满足不同的范式。

满足最低要求的范式是第一范式（1NF）。在第一范式的基础上进一步满足更多要求的称为第二范式（2NF），其余范式以次类推。一般说来，数据库只需满足第三范式（3NF）就行了。

### 第一范式

第一范式（1NF）：如果一个关系模式R的所有属性都是不可分的基本数据项，则R∈1NF（即R符合第一范式）。简单的说，就是每一个列（属性）只有一个，没有重复。

*要求*

1. 必须有主键来加以识别。
2. 每个字段只能存放单一的值并确保有数据没有重复的组。

例如：

姓名 | 班级	| 课程
-|-|-
小明 |	1班 |	数学、语文
小红 |	2班 |	英语
小明 |	2班 |	数学

里面还有重复组并且没有存放单一的值，并不符合第一范式，我们给其增加主键学号加以区别：

学号	| 姓名 | 班级 |	课程 |
-|-|-|-
101	| 小明 | 1班 |	数学 |
101	| 小明 | 1班 |	语文 |
201	| 小红 | 2班 |	英语 |
202	| 小明 | 2班 |	数学 |

### 第二范式

首先要满足第一范式。它的规则是要求数据表里的所有数据都要和该数据表的主键有完全依赖关系。例如：

货物 | 供应商ID	| 供应商 | 价格 | 供应商地址
-|-|-|-|-
毛巾 | 01	| 世纪联华 | 10.0 |	星光大道|
牙刷 | 01	| 世纪联华 | 5.0 |	星光大道|
毛巾 | 02	| 十足 | 12.0|	月光大道|

这里的主键有货物和供应商ID，价格和两个主键都有关，可是供应商地址只和供应商ID有依赖关系。那么不符合第二范式，将其修改为两张表：

供应商ID | 供应商 | 供应商地址
-|-|-
01 | 世纪联华 | 星光大道
02 | 十足 | 月光大道

货物 | 供应商ID | 价格
-|-|-
毛巾 | 01 | 10.0
牙刷 | 01 | 5.0
毛巾 | 02 | 12.0

这样就符合了第二范式要求的表内数据和表内主键完全依赖的关系。

### 第三范式

在第二范式的基础上，要求所有非键属性都只和候选键有相关性，也就是说非键属性之间应该是独立无关的。
从上述表来说，供应商和供应商地址是相关的，知道了供应商也就知道了供应商地址（不考虑一厂多址的情况）。可以分为：

供应商ID | 供应商
-|-
01 | 世纪联华
02 | 十足

供应商ID | 供应商地址
-|-
01 | 星光大道
02 | 月光大道

## 关系模型
关系数据库是建立在关系模型上的。而关系模型本质上就是若干个存储数据的二维表，可以把它们看作很多Excel表。

表的每一行称为记录（Record），记录是一个逻辑意义上的数据。

表的每一列称为字段（Column），同一个表的每一行记录都拥有相同的若干字段。

字段定义了数据类型（整型、浮点型、字符串、日期等），以及是否允许为NULL。注意NULL表示字段数据不存在。一个整型字段如果为NULL不表示它的值为0，同样的，一个字符串型字段为NULL也不表示它的值为空串''。
> 通常情况下，字段应该避免允许为NULL。不允许为NULL可以简化查询条件，加快查询速度，也利于应用程序读取数据后无需判断是否为NULL。

和Excel表有所不同的是，关系数据库的表和表之间需要建立“一对多”，“多对一”和“一对一”的关系，这样才能够按照应用程序的逻辑来组织和存储数据。

例如，一个班级表：

ID | 名称 | 班主任
-|-|-
201	| 二年级一班 | 王老师
202	| 二年级二班 | 李老师

每一行对应着一个班级，而一个班级对应着多个学生，所以班级表和学生表的关系就是“一对多”：

ID | 姓名 | 班级ID | 性别 | 年龄
-|-|-|-|-
1	|小明	|201	|M	|9
2	|小红	|202	|F	|8
3	|小军	|202	|M	|8
4	|小白	|201	|F	|9

反过来，如果我们先在学生表中定位了一行记录，例如ID=1的小明，要确定他的班级，只需要根据他的“班级ID”对应的值201找到班级表中ID=201的记录，即二年级一班。所以，学生表和班级表是“多对一”的关系。

如果我们把班级表分拆得细一点，例如，单独创建一个教师表：

ID | 名称 | 年龄
-|-|-
A1 | 王老师 | 26
A2 | 张老师	| 39
A3 | 李老师	| 32
A4 | 赵老师	| 27

班级表只存储教师ID：

ID | 名称 | 班主任ID | 科任老师ID
-|-|-|-
201	| 二年级一班 | A1 | A2,A3
202	| 二年级二班 | A3 | A1,A2

这样，一个班级总是对应一个教师，班级表和教师表就是“一对一”关系。

## CRUD 操作
新建库：  
```
CREATE DATABASE `DB_NAME`
```

新建表：  
```
CREATE TABLE 表名称
(
  ID int PRIMARY KEY
  Name varchar(255) not null default NULL
  列名称1 数据类型 是否为空 默认值 是否主键,
  列名称2 数据类型,
  .......
)ENGINE=INNODB AUTO_INCREMENT=1000 
```

新建索引：  
```
# UNIQUE 唯一，基于散列，内容差异越大越好
CREATE UNIQUE INDEX 索引名称
ON 表名称 (列名称) 
```

查询：
```
select * from table where age>10 or sex='M'
select s.* , a.address from table_student s, table_addr a where s.id=a.sid and s.age>10 and a.address!=null

left join, right join

```