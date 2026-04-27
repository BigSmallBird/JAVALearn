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
