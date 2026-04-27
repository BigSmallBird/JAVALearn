# MYSQL:

## 什么是数据库：

* 将大量数据保存起来，通过计算机架构而成的可以进行高效访问的数据集合称为数据库 (DataBase) 简称 DB
* 用来管理数据库的计算机系统称为数据库管理系统（DataBaseManagenmentSystem）DBMS

对于个人来说 Excel， word， 记事本 等功能，基本足够支持问你对数据的记录，但是对于公司，政府，Excel或 记事本有以下不足：

* 无法多人共享
* 无法操作大量数据
* 无法对应多次修改数据

## 数据库分类：

按早期的数据库理论，有以下三种模型;

* 层次式数据库
* 网络式数据库
* 关系型数据库

但是技术的发展，前两种基本上就拜拜了，在当下的互联网，最常用的主要为 **关系型数据库** **非关系数据库**

### 关系型数据库：

关系型数据库模型是把复杂的数据结构归结为简单的二元关系（二维列表）。在关系数据库里，对数据的操作几乎全部建立在一个或多个关系表格上，通过对这些关系表格分类，合并，连接或者取等来实现数据的管理。

![img](https://style.youkeda.com/newcoursep4/d1/d1-1/%E7%94%BB%E6%9D%BF.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### 表结构：

我们来看：

![img](https://style.youkeda.com/newcoursep4/d1/d1-1/%E5%AD%A6%E7%94%9F%E8%A1%A8123.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

* 表由名，行，列，列名构成
* 列是Excel 中的一列，表示纵周
* 行是Excel 中的以行，表示横轴
* 表示列的名字，不可以重复
* 第一行对应的下表是0

我们可以看到，上述的几张表格间，都存在关系数据。关系就是数据能够对应匹配，关系就是数据能够对应匹配，在关系型数据库就是联结

## SQL(Structured Query Language)：

在数据库的学习中，我们不可能避免会接触 SQL， SQL （结构化查询语言）， 我们使用 SQL 语言对数据库进行操作，数据库就像是一个碗，里面的数据就是土豆泥，而 SQL 就是哪个勺子

## NoSQL(非关系型数据库)：

web2.0时代的到来，关系型数据库处理起来有有点力不从心，而NoSQL 本身特点，可以很好的处理这些东西

## 对于MySQL的使用：

### 创建数据库：

MySQL是一个软件，在具体使用之前，还要创建一个库，库中存放具体的数据表，（一个软件中可以创建多个“库”，用来来隔离和区分不同范围，不同作用域的数据。）

**创建代码** 

```sql
$ mysql -e 'CREATE DATABASE XXXX(小写，仓库名称);'
```

**验证代码**

```sql
mysql -e 'show DATABASES;'
```

**查看表格**

```sql
mysql -Dxxx(仓库名) -e 'desc `xxxx(表名)`;'
```

## 表格的结构：

* 表由表明， 行， 列， 列名构成
* 表名是表的名称
* 列名表示列的名字，列名不可以重复
* 表格实质上是一个二维数组，行和列都是从零开始的

### 数据库和表：

我们在云服务器上创建了一个数据库，在数据库上我们可以创建很多DB，它们供用一个与服务器，我们配置超链接的是其中一个数据库，之后我们涉及的所有表格都在我们配置的数据库下

![img](https://style.youkeda.com/newcoursep4/d1/d1-3/db1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### 表明：

表明就是表格的名称，在MySQL中我们使用英文小写来约定表示，比如用户名称，我们名为名user

如果遇到多个单词，则用分隔符_分隔，比如 pay_record 这种命名方式，一般在数据库里，我们将表名，字段名都小写，将数据库操作语句都大写。

### 字段：

在数据库表中，每一列都是一个字段，第一行是字段名，下面都是字段值，从第二行开始读取，数组下标为 0

**注意**： 字段必须是唯一的，即无法重名，字段用来约定行的值，或者与其他表格产生联系，其值可以为 NULL

### 主键：

* 主键是一个特殊字段。
* 表格可以没有主键，但是最多只能拥有一个主键
* 主键值可以为 NULL ，必须由值对应。
* 主键的值必须是绝对唯一的
* 一般用主键和其他表进行关联

## SQL数据类型：

| 类型       | 含义                                                                |
|:--------:|:-----------------------------------------------------------------:|
| VARCHAR  | 可变的长字符串，可以类比于java中的String类型                                       |
| INT      | 整形，和java中的int类型一致                                                 |
| DOUBLE   | 浮点型，和double类型一致，没有长度限制                                            |
| DATETIME | 时间类型，长度为0，格式为  **YYYY-MM-DD** **HH:MM:SS** ，例如2025-12-31 23：59：59 |
| BIGINT   | 长整型，和java的long类型一致                                                |

## 数据：

我们把表格中的一行称为一条数据，比如  HashMap  ，就会想到  **key-value** ，而主键就像是它们的**key** ，我们通常用主键快速查找。

## CRUD：

在计算机里： **创建（Create）**，**读取（Read）**，**更新（Update）**。**删除（Delete）**，也就是CEUD；这一连的动作行为，通常是为了针对某个特定的资源所做出来的举动。这四个动作最常见的用途是在使用SQL数据库于网站的API端口时被发现。

| 英文     | 中文  | SQL        | HTTP   |
|:------:|:---:|:----------:|:------:|
| CREATE | 创建  | INSERT（插入） | POST   |
| READ   | 读取  | SELECT（查询） | GET    |
| UPDATE | 更新  | UPDATE     | POST   |
| DELETE | 删除  | DELETE     | DELETE |

## 创建表格：

在创建时我们需要提供以下属性：

- 表名
- 字段名
- 字段的数据类型

例如：对应用户信息：我们要创建一张用户表，用户表包含用户id，昵称，手机号。同时我们需要创建时间和修改时间。我们来创建表格代码：

```sql
CREATE TABLE `user`(
    `id` INT(10) NOT NULL,
    `mobile` VARCHAR(11) NOT NULL,
    `nickname` VARCHAR(40) NOT NULL,
    `gmt_created` datetime,
    `gmt_modified` datetime NOT NULL,
    PRIMARY KEY (`id`)
)ENGINE= InnoDB DEFAULT CHARSET=utf-8;
```

### 创建表格：

首先是

```sql
CREATE TABLE  `user`()
```

这一行意思是：创建一张叫 user 的表格

### 创建字段：

```SQL
`id` INT(10)NOT NULL,
`mobile` VARCHAR(11) NOT NULL,
`nickname` VARCHAR(40) NOT NULL,
`gmt_created` datetime ,
`gmt_modified` datetime NOT NULL,
```

他们的语法结构都是一样的：

**字段名+数据类型+长度+是否为 NULL** 

- id 是字段名，我们用  ``  这个符号包裹他
- INT 代表数据类型，表示 id 的字段是  INT  值
- （10） 表示其最长长度
- datetime 类型无长度
- NOT NULL 表示字段不能为空，所以必须输入值

### 约定主键：

```sql
PRIMARY KEY (`id`)
```

表示主键必须是 id 这个字段  主键由以下特点

- 主键必须是已经约定的字段
- 主键不难为空
- 主键的值无法重复
- 主键的作用是表示，所以最好是计算机生成，生成后人工不干预

### 对于 UNSIGNED:

```sql
`id` INT UNSIGNED AUTO_INCREMENT
```

这句话意思是id 会从 1 开始自增，第二个为 2 ，以此类推

UNSIGNED 是指无符号的，也就是说除了负数。但是数据库默认是从1开始加

### 设置储存引擎于编码方式：

```sql
ENGING=InnoDB DEFAULT CHARSET=utf-8
```

设置引擎 InnoDB （为sql的默认引擎）默认编码为 utf-8

### 符号：

- **``** 这个符号是反引号，是用来过滤数据库关键字的，我们会把它加在字段名和表名外面，避免字段名和表名跟MySQL 系统的关键字冲突。

- 定义字段的语，语句间由  **，** 最后一句没有

- SQL 语句以  **;**  结束

### 关键字于保留字：

遇见关键字与保留字的时候，用 **``**  这个符号进行过滤

### 删除表格：

```sql
drop table table_name;
//或者这样//
DROP TABLE IF EXISTS table_name;
```

**IF EXISTS** 判断存在

**删除操作不可逆**

## 插入语句(INSERT)：

 MySQL中经常用 INSERT  INTO SQL 语句进行插入数据

### 语法：

```sql
INSERT INTO table_name(field1,field2,...fieldN)
VALUES
(value1, value2,...valueN);
```

这句话的意思是，我们向指定表插入若干个字段，和它们对应的值，例如：

```sql
INSERT INTO
  `user` (`id`, `mobile`, `nickname`, `gmt_created`)
  VALUES
    (1, '13231312313', '撒比', now());
```

- user是表名
- id，mobile 是字段名
- id 值是数字，可以直接写
- mobile 的值是VARCHAR 类型，要用  ' '  包含
- gmt_created 是 datetime 类型，我们一般使用 now() 这个函数来获取服务器时间

### 插入语句的简化：

- 如果 PRIMARY KEY 是 AUTO_INCREMENT   则可以不插入主键和对应的数据
- 如果插入的是所有的字段，则可以省略字段名，直接插入值，但是类型必须一致，比如：

```sql
INSERT INTO table_name
VALUES
(value1, value2...);
```

### 批量插入数据：

如果要一次性对一张表格插入大量数据，我们可以使用以下语法：

```sql
INSERT INTO table_name
VALUES
(value1, value2 ...)
(value1, value2 ...)
```

插入的一条语句对应表格一行，规定 NOT NULL 字段没有给到值时，插入语句就会报错。

### 查看数据库内容：

```sql
mysql -Dxxx -e 'select * from table_name;'
```

### 数据备份：

对于某些服务器关闭以后其数据也会一同关闭，为了避免这种情况，可以在终端执行

```sql
mysqldump XXXX(数据库名称) > (数据库名称).sql
```

就可以把整个数据库内的数据导出，执行 ls 就可以在终端看见除了 index.sql 文件外，多了一个 XXXX.sql 文件。

### 恢复数据：

在服务器重启后，可以拉动命令进行数据库的创建，然后终端进入备份文件夹，执行：

```sql
mysql -Dxxx<xxx.sql
```

## 查询：

### 语法;

我们指定表中的查询指定列的信息。

```sql
SELECT filder1, filder2,.... FROM table_name;
```

比如：

```sql
SELECT
  id,
  hero_name
FROM
  timi_adc;
```

如果我们查询所有的字段，那么 SQL 语句可以写：

```sql
SELECT * FROM timi_adc;
```

## WHERE 子句：

在实际工作的查询中，我们很少直接限定字段查找数据，更多的时候，我们希望查询符合某以条件的数据，这时候需要限定语句。

在 MYSQL 中，我们使用 WHERE 语句来限定条件，它的作用类似程序语句的if 

### 语法：

```sql
SELECT * FROM table_name WHERE condition;
```

### 应用：

比如上次作业我们有 timi_adc 表，想要查询胜率 > 0.5 的英雄，则：

```sql
SELECT 
  * 
FROM 
  timi_adc
WHERE
  win_rate > 0.5;
```

> WHERE 判别相等用 **=**

## Limit 子句：

在查询工作中，我们有时候需要返回指定行，比如我想查询符合某个条件的前是个数据，这时候我们需要使用Limit子句

### 语法：

```sql
SELECT * FROM table_name Limit parameter;
```

parameter 是 LIMIT 语句的参数，例如：

### 查询第 x-y 行：

```sql
SELECT 
  *
FROM
  timi_adc
LIMIT
  5，6；
```

> 这句话的意思是 查询 timi_adc 表的第 6 ~ 11 行数据，第一个参数 5 表示从第 6 行开始查，第二个参数 6， 表示一共查询 6 条记录。
> 
> 数据库的表格类似数组，是从第 0 行开始计数的，所以 5 表示第 6 行。
> 
> LIMIT 语句一般是配合 **分页** 使用的，比如我们在购物软件上，每一页都是由固定的商品数的

### 查询 0 ~ x 行：

```sql
SELECT 
  * 
FROM 
  timi_adc
LIMIT
  5;
```

> 这句话的意思是查询 0 ~ 5 行数据

**等价于：**

```sql
SELECT * FROM timi_adc LIMIT 0, 5;
```

### 查询第x行：

```sql
SELECT 
  * 
FROM 
  timi_adc
LIMIT
  4, 1;
```

> 这句话的意思是查询第 5 行数据

## 和 WHERE 子句联合使用：

LIMIT 子句通常与其他子句一同使用， LIMIT 语句会放在 WHERE  语句后面，比如说我要查 timi_adc 表中登场率大于 10% 的前五条数据：

```sql
SELECT 
  *
FROM
  timi_adc
WHERE 
  appearance_rate > 0.1
LIMIT
5;
```

# 排序（ORDER BY子句）

在实际的应用场景中，我们会需要对查询结果进行排序，这时候我们就需要使用 ORDER BY 子句。

### 语法

我们来看一下语法：

```SQL
SELECT * FROM table_name ORDER BY field_name;
```

终于到了拿胜率说话的时候了，老王想知道所有射手的胜率排序，那么我们可以这么写查询语句：

```SQL
SELECT
  *
FROM
  timi_adc
ORDER BY
  win_rate;
```

> 排序默认按照升序排序，对于int,double而言，是从小到大，对于varchar而言，是从字母A到Z，对于datetime而言，是从过去到现在

**DESC关键词**

ORDER BY 语句的排序默认是正序排序，关键词为ASC（一般不写），我们可以通过加上关键词DESC，使得排序变为逆序，比如上面的演示中，我们的想查看胜率从高到低排序：

```SQL
SELECT
  *
FROM
  timi_adc
ORDER BY
  win_rate DESC;
```

**和其他子句连用**

和LIMIT子句一样，ORDER BY子句一般和其他语句联合使用，比如我想查询胜率最高的三个射手，那么我们的查询语句可以这么写：

```SQL
SELECT
  *
FROM
  timi_adc
ORDER BY
  win_rate DESC
LIMIT
  3;
```

# 更新/删除

我们在登录形形色色的网站时，往往需要我们的密码，而忘记密码是一个基本操作，大家基本都有找回过密码的行为，找回密码时往往会要求我们创建一个新的密码，这个创建新密码并且生效的过程，在SQL语句中，就是UPDATE语句。

### 语法

我们来看一下UPDATE语句的语法：

```SQL
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

> UPDATE语句我们必须加入WHERE限定条件，否则的话UPDATE语句就会对整列起作用。

比如我们要将timi_adc表格中的艾琳这个英雄的ban率改为1%，那么我们应该这么写SQL语句：

```SQL
UPDATE
  timi_adc
SET
  ban_rate = 0.01
WHERE
  hero_name = '艾琳';
```

> 一定要注意加上WHERE限定语句，否则所有英雄的ban率都会被改成0.01 UPDATE语句不关心原有的值

### 删除语句（DELETE）

在数据库中的使用中，有时候我们需要删除一些数据，这时候就需要使用DELETE语句。 我们来看一下DELETE语句的语法：

```SQL
DELETE FROM table_name [WHERE Clause]
```

> 由于删除语句是不可恢复的，所以我们务必要增加WHERE语句，否则将会删除整张表格的数据

我们来看一下DELETE语句的几种应用场景：

删除user表中id为4的行：

```SQL
delete from user where id=4;
```

删除user表中所有id小于20的数据：

```SQL
delete from `user` where id<20;
```

删除user表中的所有数据：

```SQL
delete from user;
```

> DELETE 语句只删除表中的数据，如果要删除表格，我们用之前学过的 DROP TABLE + 表名的语句

# MySQL 基础语法学习笔记

## 1. 创建表 (CREATE TABLE)

使用 `CREATE TABLE` 语句创建一张新表。通过 `字段名称 + 数据类型 + 长度 + 是否为 NULL` 来定义字段，并使用 `PRIMARY KEY` 约定主键。

```sql
CREATE TABLE `user`(
    `id` INT(10) NOT NULL,
    `mobile` VARCHAR(11) NOT NULL,
    `gmt_created` datetime,
    `gmt_modified` datetime NOT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 主键自增
在企业级开发中，通常使用 `BIGINT` 作为主键，并设置自增：
- `AUTO_INCREMENT`: 初始值为 1，自动增加。
- `UNSIGNED`: 无符号，即非负数。

```sql
`id` INT UNSIGNED AUTO_INCREMENT
```

---

## 2. 插入数据 (INSERT INTO)

向指定表插入若干字段及其对应的值。

**语法：**
```sql
INSERT INTO table_name (field1, field2, ....fieldN)
VALUES (value1, value2, ....valueN);
```

**示例：**
```sql
INSERT INTO `user` (`id`, `mobile`, `nickname`, `gmt_created`)
VALUES (1, '15256644644', 'xx', NOW());
```

**简化语法（插入多行）：**
```sql
INSERT INTO table_name
VALUES 
    (value1, value2, .....valueN),
    (value1, value2, .....valueN);
```

---

## 3. 查询数据 (SELECT)

使用 `SELECT` 语句从数据库中提取数据。

**语法：**
```sql
SELECT field1, field2 .... fieldN FROM table_name;
```

**示例：**
```sql
SELECT id, hero_name FROM timi_adc;
```

**查询所有字段：**
```sql
SELECT * FROM timi_adc;
```

---

## 4. 条件查询 (WHERE)

使用 `WHERE` 子句来过滤结果，只返回符合特定条件的记录。其作用类似于编程语言中的 `if`。

**语法：**
```sql
SELECT * FROM table_name WHERE condition;
```

**示例：**
```sql
SELECT * FROM timi_adc WHERE win_rate > 0.5;
```

---

## 5. 结果限制 (LIMIT)

使用 `LIMIT` 子句来限制返回的记录数量，常用于分页。

**语法：**
```sql
SELECT * FROM table_name LIMIT parameter;
```

**示例（查询第 6 到 11 行的数据）：**
```sql
SELECT * FROM table_name LIMIT 5, 6;
```

---

## 6. 排序 (ORDER BY)

使用 `ORDER BY` 子句对结果集进行排序。

- `ASC`: 升序（默认）。
- `DESC`: 降序。

**示例（按胜率降序排序）：**
```sql
SELECT * FROM timi_adc ORDER BY win_rate DESC;
```

---

## 7. 更新数据 (UPDATE)

使用 `UPDATE` 语句修改表中的现有记录。

**注意：** 务必配合 `WHERE` 子句使用，否则会更新整张表的数据。

```sql
UPDATE timi_adc SET ban_rate = 0.1 WHERE hero_name = '艾琳';
```

---

## 8. 删除数据 (DELETE)

使用 `DELETE` 语句删除表中的记录。

**注意：** 删除操作不可恢复，务必加上 `WHERE` 子句。

**示例：**
```sql
-- 删除指定 ID 的数据
DELETE FROM user WHERE id = 4;

-- 删除所有 ID 小于 20 的数据
DELETE FROM `user` WHERE id < 20;

-- 删除表中所有数据（慎用）
DELETE FROM user;
```

---

## 9. 模糊查询 (LIKE)

使用 `LIKE` 子句进行搜索。通常配合通配符 `%` 使用（`%` 代表任意字符）。

**示例（查找名字中带“孙”的英雄）：**
```sql
SELECT * FROM timi_adc WHERE hero_name LIKE '%孙%';
```

**多条件模糊查询：**
```sql
SELECT * FROM timi_adc 
WHERE hero_name LIKE '%孙%' AND hero_name NOT LIKE '%悟空%';
```

### % 的位置会决定搜索结果的不同：
比如 ```'%孙'``` 这个字符串含  “孙” ```'孙%'``` 表示 这个字符串以 “孙” 这个字开头  

**比如:**
```sql
SELECT 
    *
FROM
    timi_adc
WHERE
    hero_name LIKE '孙%';


SELECT
    *
FROM
    timi_adc
WHERE
    hero_name LIKE '%孙%';
```
