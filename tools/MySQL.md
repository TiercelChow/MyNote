MySQL

[toc]

## 1. MySQL 8的安装，升级和新特性（）

## 2. 数据库操作

### 2.1 操作数据库

```mysql
##显示当前已经存在的数据库
SHOW DATABASES;
##创建名为database_name的数据库
CREATE DATABASE database_name;
##选择数据库
USE database_name;
##删除数据库
DROP DATABASE database_name;
```

### 2.2 什么是存储引擎

```mysql
##首先确定数据库管理系统所支持的存储引擎
SHOW ENGINES;
##也可以通过下列语句进行查询
SHOW ENGINES \G;
##或者
SHOW VARIABLES LIKE 'have%';
##创建表时，若没有指定存储引擎，表的存储引擎为默认的存储引擎
##查看默认存储引擎
SHOW VARIABLES LIKE 'default_storage_engine';
##修改默认存储引擎（以修改为NyISAM为例，修改后通过查询语句验证）
SET DEFAULT_STORAGE_ENGINE=MyISAM;
```

> 查询数据库管理系统所支持的存储引擎得到各参数意义：
>
> - Engine参数表示存储引擎名称
> - Suppor参数表示MySQL数据库管理系统是否支持该存储引擎
> - DEFAULT表示系统默认支持的存储引擎
> - Comment参数表示对存储引擎的评论
> - Transactions参数表示存储引擎是否支持事务
> - XA参数表示存储引擎所支持的分布式是否符合XA规范
> - Savepoints参数表示存储引擎是否指出事务处理的保存点



> 查看默认存储引擎得到的各参数的意义
>
> - Variable_name参数表示存储引擎的名字
> - Value参数表示MySQL数据库管理系统是否支持存储引擎，YES:支持，NO:不支持，DISABLE:支持但还未开始

### 2.3 选择存储引擎

| 特性           | InnoDB | MyISAM | MENMORY |
| -------------- | ------ | ------ | ------- |
| 事务安全       | 支持   | 无     | 无      |
| 存储显示       | 64TB   | 有     | 有      |
| 空间使用       | 高     | 低     | 低      |
| 内存使用       | 高     | 低     | 高      |
| 插入数据的速度 | 低     | 高     | 高      |
| 锁机制         | 行锁   | 表锁   | 表锁    |
| 对外键的支持   | 支持   | 无     | 无      |
| 数据可压缩     | 无     | 支持   | 无      |
| 批量插入速度   | 低     | 高     | 高      |

在同一个数据库中，不同的表可以使用不同的存储引擎：如果一个表要求较高的事务处理，可以选择InnoDB；如果一个表会被频繁查询，可以选择MyISAM存储引擎；如果时一个用于查询的临时表，那么可以选择MEMORY存储引擎。

## 3. 数据表操作

### 3.1 数据表的设计理念

表中数据库对象：

>- 列（Column）：属性列，创建时必须指定列的名字和数据类型。
>- 索引（Index）：根据数据库表列建立起来的顺序，提供了快速访问数据的途径且可监督表的数据，使其索引指向的列中的数据不重复。
>- 触发器（Trigger）：用户定义的事务的集合，当对一个表中的数据进行插入，更新或者删除时，这组命令就会自动执行，累哦用来确保数据的完整性和安全性。

1. 标准化和规范化：

> - 第一范式（1NF），确保每列保持原子性。
> - 第二范式（2NF），确保每列都和主键相关。
> - 第三范式（3NF），确保每列都和主键直接相关，而不是间接相关。

2. 数据驱动

2. 考虑各种变化
3. 表和表的关系

> 数据库里表的关系有三种：一对一，一对多，多对多。

### 3.2 数据库中的数据类型（）

#### 3.2.1 整数类型

| 整数类型  | 字节数 | 无符号数的取值范围     | 有符号数的取值范围                       |
| --------- | ------ | ---------------------- | ---------------------------------------- |
| TINYINT   | 1      | 0~225                  | -128~127                                 |
| SMALLINT  | 2      | 0~65535                | -32768~32767                             |
| MEDIUMINT | 3      | 0~16777215             | -8388608~8388607                         |
| INT       | 4      | 0~4294967295           | -2147483648~2147483647                   |
| INTEGER   | 4      | 0~4294967295           | -2147483648~2147483647                   |
| BIGINT    | 8      | 0~18446744073709551615 | -9223372036854775808~9223372036854775807 |

```mysql
##使用命令HELP INT可以查看INT的数据范围
HELP INT;
```

如果数据插入超出了规定的范围，就会插入失败。

#### 3.2.2 浮点数类型和定点数类型

| 类型                   | 字节数 | 负数的取值范围                                    | 非负数的取值范围                                   |
| ---------------------- | ------ | ------------------------------------------------- | -------------------------------------------------- |
| FLOAT                  | 4      | -3.402823466E+38~-1.175494351E-38                 | 0和1.175494351E-38~3.402823466E+38                 |
| DOUBLE                 | 8      | -1.7976931348623157E+308~-2.2250738585072014E-308 | 0和2.2250738585072014E-308~1.7976931348623157E+308 |
| DECIMAL(M,D)或DEC(M,D) | M+2    | 同DOUBLE类型                                      | 同DOUBLE类型                                       |

#### 3.2.3 日期与时间

| 类型      | 字节数 | 取值范围                             | 零值                |
| --------- | ------ | ------------------------------------ | ------------------- |
| YEAR      | 1      | 1901~2155                            | 0000                |
| DATE      | 4      | 1000-01~9999-12-31                   | 0000:00:00          |
| TIME      | 3      | -838:59:59~838:59:59                 | 00-00-00            |
| DATETIME  | 8      | 1000-01 00:00:00~9999-12-31 23:59:59 | 0000-00-00 00:00:00 |
| TIMESTAMP | 4      | 197000101080001~2308011911407        | 00000000000000      |

通过函数NOW()可获取系统当前时间（DATATIME或TIMESTAMP类型）

#### 3.2.4 字符串类型

#### 3.2.5 二进制类型

#### 3.2.6 JSON类型以及MySQL 8 JSON增强

### 3.3 MySQL 8新特性：字符集与排序规则

### 3.4 创建表

**创建表的语法形式**

```mysql
CREATE TABLE tablename(
    属性名 数据类型 [完整性约束条件],
    属性名 数据类型 [完整性约束条件],
    ......
	属性名 数据类型 [完整性约束条件]);
```

注意对数据库操作前需要通过USE语句选择数据库。

**创建带JSON类型的表**

### 3.5 查看表结构

```mysql
##DESCRIBE语句查看表定义(table_name为所要查看对象定义信息的名字，DESCRIBE可缩写为DESC)
DESCRIBE table_name;
##SHOW CREATE TABLE语句查看表详细定义
SHOW CREATE TABLE table_name;
```

### 3.6 删除表

```mysql
##可使用DROP TABLE语句删除没有被其他关联的普通表
DROP TABLE table_name;
##删除后可通过DESCRIBE语句验证是否被删除
```

### 3.7 修改表

#### 3.7.1 修改表名

```mysql
##修改表名通过ALTER TABLE语句实现
ALTER TABLE oldTablename [TO] newTablename;
##修改完成后同样可以通过DESCRIBE语句进行验证
```

#### 3.7.2 增加字段

```mysql
##MySQL数据库管理系统中通过下列SQL语句来实现新增字段
##tablename为所要修改表的名称，propName表示所要增加字段的名称，propType表示索要增加字段存储数据的数据类##型，pNameNew表示新增的字段名，pNameOld表示已经存在的字段名
##添加在所有字段最后一个位置
ALTER TABLE tablename ADD propName propType;
##添加在表中第一个位置
ALTER TABLE tablename ADD propName propType FIRST;
##添加在pNameOld之后
ALTER TABLE tablename ADD pNameNew propType AFTER pNameOld;
##增加后可通过DESCRIBE语句验证是否被删除
```

#### 3.7.3 删除字段

```mysql
##删除字段通过SQL语句ALTER TABLE来实现
ALTER TABLE tablename DROP propName;
##删除后可通过DESCRIBE语句验证是否被删除
```

#### 3.7.4 修改字段

```mysql
##在MySQL中，ALTER TABLE语句可以实现修改字段的操作
##修改字段类型
ALTER TABLE tablename MODIFY porpName propType;
##修改字段的名称
ALTER TABLE tablename CHANGE pNameOld pNameNew pTypeOld;
##同时修改字段名称和类型
ALTER TABLE tablename CHANGE pNameOld pNameNew pTypeNew;
##修改字段的顺序（FIRST表示讲字段调整至表的的第一个位置，AFTER pName2表示将字段调整至pName2字段位置之后）
ALTER TABLE tablename MODIFY pName propName1 propType FIRST|AFTER pName2;
##修改后可以通过DESCRIBE语句进行验证
```

### 3.8 操作表的约束

完整性约束是对字段进行限制，要求用户对该属性进行的操作符合特定的要求，否则讲不执行用户的操作。

**完整性约束条件**

| 约束条件       | 说明                                           |
| -------------- | ---------------------------------------------- |
| PRIMARY KEY    | 标识该属性为该表的主键，可以唯一标识对应的元组 |
| FOREIGN KEY    | 标识该属性为该表的外键，是与之联系的某表的主键 |
| NOT NULL       | 标识该属性不能为空                             |
| UNIQUE         | 标识该属性的值是唯一的                         |
| AUTO_INCREMENT | 标识该属性的值自动增加，这是MySQL语句的特色    |
| DEFAULT        | 为该属性设置默认值                             |

#### 3.8.1设置表字段的非空约束（NOT NULL,NK）

```mysql
CREATE TABLE tablename(
	propName propType NOT NULL,
	...);
```

#### 3.8.2设置表字段的默认值（DAFAULT）

```mysql
##defaultValue为默认值，如果用户插入的新记录中该字段为空值，那么数据库管理系统将会自动插入该默认值
CREATE TABLE tablename(
	propName propType DEFAULT defaultValue,
	...);
```

#### 3.8.3 设置表字段唯一约束（UNIQUE,UK）

```mysql
##设置UK约束后，若用户插入的记录在该字段上的值与其他记录在该字段上的值重复，那么数据库管理系统将报错
CREATE TABLE tablename(
	propName propType UNIQUE,
	...);
```

#### 3.8.4 设置表字段的主键约束（PRIMARY,PK）

主键是表的以一个特殊字段，能唯一标识该表中的每条信息。主键用来标识每个记录，每个记录的主键值都不同。

主键的主要目的是帮助数据库管理系统以最快的速度查找到表的某一条信息。主键必须满足的条件就是主键必须是唯一的，表中任意两条记录的主键字段的值笨呢个相同，并且是非空值。

主键可以是单一的字段，也可以是多个字段的组合。

1. 单字段主键

```mysql
CREATE TABLE tablename(
	propName propType PRIMARY KEY,
	...);
```

```mysql
##若想给某字段的PK约束设置一个名字，可执行SQL语句CONSTRAINT（pk_name为设置的名字）
CREATE TABLE tablename(
	propName propType,
	...
	CONSTRAINT pk_name PRIMARY KEY(propName));
```

2. 多字段主键

```mysql
##主键由多个属性组合而成,在属性定义完之后统一设置主键
CREATE TABLE tablename(
	propName1 propType1,
	propName2 propType2,
	...
	[CONSTARINT pk_name] PRIMARY KEY(propName1,propName2));
```

#### 3.8.5 设置表字段值自动增加（AUTO_INCREMENT）

AUTO_INCREMENT是MySQL唯一扩展的完整性约束，当向数据库表中插入新记录时，字段上的值会自动生成唯一的ID，在设置该约束时，一个数据库表中只能有一个字段使用该约束且其数据类型必须是整数类型，由于生成的ID唯一，因此该字段也经常会同时设置成PK主键。

```mysql
CREATE TABLE tablename(
	propName propType AUTO_INCREMENT,
	...);
```

#### 3.8.6 设置表字段的外键约束（FOREIGN KEY,FK）

外键是表的一个特殊字段，外键约束是为了保证多个表（通常为两个表）之间的参照完整性，即构建两个表的字段之间的参照关系。

设置外键的两个表之间具有父子关系，即子表中某个字段的值的取值范围由父表决定，在具体设置FK约束时，设置FK约束的字段必须依赖于数据库中已经存在的父表的主键，同时外键可以为空（NULL）。

```mysql
##tablename_1是要设置外键的表名，propName1_1是要设置外键的字段
##tablename_2是父表的名称，propName2_1是父表中设置主键约束的字段名
##FK_prop为设置的FK约束的名称
CREATE TABLE tablename(
	propName1_1 propType1_1,
	propName1_2 propType1_2,
	...
	[CONSTRAINT FK_prop] FOREIGN KEY(propName1_1)
	REFERENCES tablename_2(propName2_1));
```

## 4. 数据操作

### 4.1 插入数据记录

```mysql
##插入完整数据记录
INSERT INTO tablename(field1,field2,field3,...,fieldn)
	VALUES(value1,value2,value3,...,valuen);
##插入部分数据记录，语句与上述相同，只需要将需要插入的fieldn列出并且valuen与之一一对应即可
##插入多条完整数据记录
INSERT INTO tablename(field1,field2,field3,...,fieldn)
	VALUES(value11,value12,value13,...,value1n),
	VALUES(value21,value22,value23,...,value2n),
	...
	VALUES(valuem1,valuem2,valuem3,...,valuemn);
##另一种语法
INSERT INTO tablename
VALUES(value11,value12,value13,...,value1n),
	  (value21,value22,value23,...,value2n),
	  ...
	  (valuem1,valuem2,valuem3,...,valuemn);
##插入多条部分数据记录，结合插入多条完整记录和插入部分数据记录语法即可
##插入JSON结构的数据记录
INSERT INTO tablename(jsonfield)
	VALUES(jsonObjectValue);
##插入完成后可通过SELECT语句查看是否插入
SELECT * FROM tablename;
```

### 4.2 更新数据记录

```mysql
##更新特定数据记录（参数CONDITION指定更新满足条件的特定数据记录）
UPDATE tablename
SET field1=value1,field2=value2,field3=value3
	WHERE CONDITION;
##更新所有数据记录（参数CONDITION表示满足表tablename中的所有数据记录，或不使用关键字WHERE语句）	
UPDATE tablename
SET field1=value1,field2=value2,field3=value3
	WHERE CONDITION;
##更新JSON结构的数据记录（colname为JSON类型的字段，path为路径，通常为美元符号加key的形式“$.key”，val为要添加的##新值，WHERE为条件语句）
UPDATE tablename
SET colname = JSON_REPLACE(colname,path,val)
	WHERE CONDITION;
##更新完成后可通过SELECT语句查看是否插入
SELECT * FROM tablename;
```

### 4.3 删除数据记录

```mysql
##删除特定数据记录
DELETE FROM tablename
	WHERE CONDITION;
##删除所有数据记录（参数CONDITION表示满足表tablename中的所有数据记录，或不使用关键字WHERE语句）
DELETE FROM tablename WHERE CONDITION;

##更新完成后可通过SELECT语句查看是否插入
SELECT * FROM tablename;
```

## 5. 数据查询

### 5.1 简单查询

SELECT语句的基本语法

```mysql
##field1~fieldn参数表示需要查询的字段名；tablename参数表示表的名称；CONDITION1表示查询条件；
##fieldm参数表示按该字段中的数据进行分组；CONDITION2参数表示满足该表达式的数据才能输出；
##fieldn参数指按该字段中数据进行排序，排序方式由ASC（升序，默认参数）或DESC（降序）两个参数指出。
	SELECT field1 field2 ... fieldn
FROM tablename
[WHERE CONDITION1]
[GROUP BY fieldm [HAVING CONDITION2]]
[ORDER BY fieldn [ASC|DESC]]
```

#### 5.1.1 查询所有字段数据

```mysql
##列出表的所有列
SELECT field1,field2,...,fieldn FROM tablename;
##使用“*”，该方法同样可以查询查询所有字段数据
##“*”可以替代表中所有符号，但不够灵活，只能按照表中字段的给固定顺序显示，不能随便改变字段的顺序
SELECT * FROM tablename;
```

#### 5.1.2 查询指定字段数据

```mysql
##语法与查询所有字段数据类似，但是只需列出需要查询的字段名即可
SELECT field1,field2,...,fieldn FROM tablename;
```

#### 5.1.3 DISTINCT查询

```mysql
##DISTINCT可以去掉查询结果中重复的数据，实现查询不重复数据
SELECT DISTINCT field1 field2 field3 ... fieldn
	FROM tablename;
```

#### 5.1.4 IN查询

```mysql
##IN用来实现判断字段的数值是否在指集合的条件查询，若fieldm的值存在集合中，则该记录会被查询出来
##若需要查询fieldm的值不在集合中的记录，则在IN前面加上NOT即可
SELECT field1,field2,...,fieldn
FROM tablename WHERE fieldm IN(value1,value2,value3,...,valuen);
```

> - 使用关键字IN时，若集合中存在NULL，则不会影响查询结果，使用关键字NOT IN，若集合中存在NULL，则不会由任何结果。

#### 5.1.5 BETWEEN AND查询

```mysql
##BETWEEN AND用于实现判断字段的数值是否在指定范围内的条件查询
##若需要查询不符合范围的数据记录，在BETWEEN前加上NOT即可
SELECT field1,field2,...,fieldn
FROM tablename WHERE fildm BETWEEN minvalue AND maxvalue;
```

#### 5.1.6 LIKE模糊查询

```mysql
##fieldn表示表中的字段名字，通过关键字LIKE来判断字段field的值是否与value字符串匹配，字符串必须加上''或""
##若需要查询不匹配的数据记录，在LIKE前加上NOT即可
SELECT field1,field2,...,fieldn
FROM tablename WHERE fieldm LIKE value;
```

由于关键字LIKE可以实现模糊查询，因此该关键字后面的字符串参数除了可以使用完整的字符串外，还可以包含通配符

| 符号 | 功能描述                                                     |
| ---- | ------------------------------------------------------------ |
| _    | 该通配符能匹配单个字符                                       |
| %    | 该通配符可以匹配任意长度的字符串，既可以是0个字符，1个字符，也可以是很多字符 |

进行模糊查询时将需要模糊查询的部分放入字符串中即可，如"..._..."，"...%..."。

```mysql
##使用LIKE关键字查询其他类型数据，如：查询带有数字9的所有记录
SELECT name,English FROM score WHERE English LIKE '%9%';
##若匹配"%%"，就表示查询所有数据记录
```

#### 5.1.7 对查询结果排序

```mysql
##参数fieldm表示按照该字段进行排序，ASC表示按升序进行排序（默认），DESC表示按照降序进行排序
SELECT field1,field2,field3,...,fieldn
FROM tablename ORDER BY fieldm [ASC|DESC];
##也可指定多个字段进行排序
SELECT field1,field2,field3,...,fieldn
FROM tablename ORDER BY fieldm1 [ASC|DESC],fieldm2 [ASC|DESC];
```

#### 5.1.8 简单分组查询

```mysql
##MySQL提供了5个统计函数帮助用户统计数据，具体使用时，先根据指定特定条件分组或针对表中所有记录数，后统计计算。
##GROUP BY单独使用时，默认查询出每个分组中随机的一条记录
SELECT function()
FROM tablename WHERE CONDITION GROUP BY field;
##只进行简单分组查询
SELECT * FROM tablename WHERE CONDITION GROUP BY field;
```

#### 5.1.9 统计分组查询

```mysql
##GROUP_CONCAT()可以实现显示每个分组中的指定字段，COUNT()可以显示每个分组中信息的个数
SELECT GROUP_CONCAT(field)
FROM tablename
WHERE CONDITION GROUP BY field;
```

### 5.2 联合查询

#### 5.2.1 内连接查询



#### 5.2.2 外连接查询

#### 5.2.3 合并查询数据记录

#### 5.2.4 子查询

## 6.索引







 





