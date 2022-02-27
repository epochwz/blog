# Introduction

数据库是按照数据结构来组织、存储、管理数据的仓库

**相关术语**

```text
数据库系统 (Database System,DBS)
    数据库 (Database,DB)
    数据库管理系统 (Database Management System,DBMS)
    数据库应用开发工具
    数据库管理员及用户

结构化查询语言 (Structured Query Language,SQL)
    数据定义语言    DDL     定义数据库、数据表、索引、触发器等
    数据操作语言    DML     插入、更新、删除数据
    数据查询语言    DQL     查询数据
    数据控制语言    DCL     权限控制
```

**常见数据库**

- Oracle
- SQL Server
- MySQL
- Access
- DB2
- Postgre SQL
- Redis

## 配置文件

数据库的配置文件是 `my.cnf`

## 数据类型

```text
数值类型
    整数型
        TINYINT         1 byte
        SMALLINT        2 byte
        MEDIUMINT       3 byte
        INT             4 byte
        BIGINT          8 byte
        BOOL/BOOLEAN    1 byte, 相当于 TINYINT(1)：0-false,else-true
    浮点型
    定点型
字符串类型
    CHAR
    VARCHAR
    TEXT
时间日期类型
    TIME
    DATE
    DATETIME
    TIMESTAMP
    YEAR
```

- INTEGER(Display Width): 数值类型后面的括号中的数字是指检索数据时显示的数据长度，与数据取值范围无关，需要与 `ZEROFILL` 配合使用
- CHAR & VARCHAR
  - CHAR 是定长，拿空间换时间；VARCHAR 是变长，拿时间换空间
  - CHAR 在存储数据时会在末尾使用空格补齐长度，而在检索时会去除末尾空格
  - VARCHAR 在存储数据时不会进行空格填充，而在检索时也不会去除末尾空格
- ENUM & SET
  - ENUM 可以指定字段的枚举值，赋值时只能选择其中一个值
  - SET 可以指定字段的枚举值，赋值时可以选择其中的多个值
  - ENUM 和 SET 使用枚举值赋值时，会自动过滤值的末尾空格
  - ENUM 和 SET 都可以使用编号进行赋值，编号从 1 开始，超过枚举值长度则报错
- BLOB/TEXT 列不能有默认值，检索时不存在大小写转换

## 约束 (Constraint)

**约束**：在数据表的字段上设置条件，从而保证数据的正确性、完整性和一致性

**约束的分类**

```bash
# 按约束字段的数目分类
表级约束：对多个数据列建立的约束
列级约束：对单个数据列建立的约束

# 按约束功能分类
非空约束    NOT NULL        指定字段的值不能为空（插入记录时必须给出非空值）
默认约束    DEFAULT         指定字段的默认值，不指定时所有数据的默认值都是 NULL
唯一约束    UNIQUE KEY      指定字段的值具有唯一性，自动创建索引，一个表可以有多个唯一键，NULL 值可以重复
主键约束    PRIMARY KEY     指定字段是记录的唯一标识，自动创建索引，自带非空约束，一个表只能有一个主键
外键约束    FOREIGN KEY     将当前表的某个字段与其他表的某个字段进行关联

无符号      UNSIGNED        只能用于数值类型的字段：指定字段的值只能是非负数
零填充      ZEROFILL        只能用于数值类型的字段，自带无符号约束：当字段值的长度不够时在前面补零
自增长      AUTO_INCREMENT  只能用于数值类型的字段，且只能用于主键列、唯一列、外键列：插入记录时没有指定值的时候，自动增加 1
```

### 外键约束

**定义**：如果一个表的某个列关联了另一个表的某个列，则称该表的某个列添加了外键约束

**作用**：保证数据的完整性和一致性，实现一对一或一对多的关系

**要求**

- 外键列和参照列的数据类型必须相同（数值的长度、是否有符号位必须相同，而字符的长度可以不同）
  - 外键列：添加 `FOREIGN KEY` 约束的列
  - 参照列：外键列所参照的列
- 父表和子表必须使用相同的存储引擎（只能是 `InnoDB`），且禁止使用临时表
  - 子表（外键表）：具有外键列的表
  - 父表（参照表）：子表所参照的表
- 外键列和参照列必须创建索引
  - MySQL 会自动为主键列、唯一列、外键列创建索引
  - MySQL 不会自动为参照列创建索引

**语法**

```sql
[CONSTRAINT fk_name] FOREIGN KEY (col_name) REFERENCES 父表(col_name)
```

## 存储引擎

存储引擎也称表类型，指的是数据表的存储机制、索引方案等一整套相关技术

- MySQL 提供了多种存储引擎，它们分别使用不同的技术（存储机制、索引技巧、锁定水平）将数据存储在文件（内存) 中，从而提供不同的功能和性能；通过选择恰当的存储引擎，可以获得额外的速度或功能，从而改善应用的整体性能
- MySQL 的 `InnoDB` 和 `BDB` 存储引擎提供了事务安全功能，其他存储引擎都是非事务安全的
- MySQL5.5 之后默认使用 `InnoDB` 存储引擎

### InnoDB

- 遵循 ACID 模型
  - 原子性 (Atomiocity)
  - 一致性 (Consistency)
  - 隔离性 (Isolation)
  - 持久性 (Durability)
- 支持事务，具有从服务崩溃中恢复的能力，能够最大限度的保护用户数据
- 支持行级锁，提升多用户并发时的读写性能
- 支持外键，保证数据的完整性和一致性
- 拥有独立的缓冲池，常用的数据和索引都存在缓存中
- 对于 insert/update/deltete 操作，使用 change_buffering 机制自动优化，提供一致性的读，缓存变更数据，减少磁盘 IO，提高性能
- 生成表文件
  - `table.frm` 表结构文件
  - `table.ibd` 数据和索引存储在表空间中

### MyISAM

- 建表时可以指定保存索引文件的路径 `INDEX DIRCTORY [=] absolute_path`
- 建表时可以指定保存数据文件的路径 `DATA DIRECTORY [=] absolute_path`
- 建表时可以指定数据表的存储格式 `ROW FORMAT=format_name`
  - FIXED: 静态 / 定长，数据表中的字段类型不包含 VARCHAR/TEXT/BLOB
  - DYNAMIC: 动态，数据表中的字段类型包含了 VARCHAR/TEXT/BLOB
  - COMPRESSED: 压缩
- 单表最大记录数量：2^64
- 单表最多索引数量：64
- 复合索引最多包含 16 列，索引值最大长度是 1000B

### 常用存储引擎的对比

| 功能特性         | InnoDB                                                                                       | MyISAM                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| 是否支持外键     | 支持                                                                                         | 不支持                                                                                  |
| 是否支持事务     | 支持                                                                                         | 不支持                                                                                  |
| 支持的锁类型     | 支持行级锁，优势在于更新和删除                                                               | 支持表级锁，优势在于插入和检索                                                          |
| 生成的数据表文件 | 每个表只生成一个 `table.frm` 文件，所有使用 `InnoDB` 的数据表的数据都存储在 `ibdata*` 文件中 | 每个表生成 3 个文件：索引文件 (table.MYI)、数据文件 (table.MYD)、表结构文件 (table.frm) |

## SQL

### 语法规范

- 库名、表名、列名尽量不使用 SQL 关键字，如果必须使用时需要添加 ``
- SQL 关键字使用大写字母，库名、表名、列名使用小写字母
- SQL 语句支持折行书写，但不能拆开完整单词

### 数据库

```sql
#/--        SQL 注释
help/?/\h   查看帮助文档
;/\g        SQL 语句的默认结束符
\c          取消当前语句的执行
\G          格式化检索结果
```

```sql
# 查看版本信息
mysql -V
mysql --version
# 登录
mysql -u<user> -p
mysql -u<user> -p<password>
mysql -u<user> -p<password> -D <db_name>
# 退出
exit
quit
\q

# 查看当前版本
select version();
# 查看当前数据库
select database();
# 查看当前用户
select user();
# 查看当前时间
select now();

# 查看上一步操作的警告
SHOW WARNINGS;

# 查看数据库
SHOW {DATABASES|SCHEMAS}
# 创建数据库
CREATE {database|schema} db_name
CREATE {database|schema} {IF NOT EXISTS} db_name
CREATE {database|schema} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] 'charset_name'
# 查看数据库详细信息
SHOW  CREATE DATABASE db_name
# 删除数据库
DROP DATABASE [IF EXISTS] db_name
# 修改数据库
ALTER DATABASE db_name [DEFAULT] CHARACTER SET [=] 'charset_name'
# 打开数据库
use <db_name>;

```

### 数据表

数据表表名不能重复；数据表由行列组成，每一行至少有一列

```sql
# 创建数据表
CREATE TABLE [IF NOT EXISTS] tb_name(fieldName fieldType [Constraint])ENGINE=<StorageEngine> CHARSET=<charset>;

-- 修改表结构
# 添加字段
ALTER TABLE tb_name ADD fieldName fieldType [Constraint] {After fieldName|First}
# 删除字段
ALTER TABLE tb_name DROP [COLUMN] fieldName
# 同时添加 / 删除字段
ALTER TABLE tb_name ADD fieldName fieldType [Constraint] {After fieldName|First},ADD fieldName fieldType [Constraint] {After fieldName|First},DROP fieldName
ALTER TABLE tb_name ADD [COLUMN] (field1,field2)
# 修改表名称
ALTER TABLE tb_name RENAME {AS|TO} new_tb_name
RENAME old_tb_name TO new_tb_name
# 修改自增长的起始值
ALTER TABLE tb_name AUTO_INCREMENT=1;

-- 修改列结构
# 添加默认值
ALTER TABLE tb_name ALTER fieldName SET DEFAULT defaultValue
# 删除默认值
ALTER TABLE tb_name ALTER fieldName DROP DEFAULT
# 修改字段类型、字段属性
ALTER TABLE tb_name MODIFY fieldName filedType [Constraint] [After field|First]
# 修改字段名称、字段类型、字段属性
ALTER TABLE tb_name CHANGE oldFieldName newFieldName filedType [Constraint] [After field|First]
# 添加主键
ALTER TABLE tb_name ADD RPIMARY KEY(fieldName)
# 删除主键：删除自增长的主键时，需要先修改字段属性，去除自增长约束
ALTER TABLE tb_name DROP PRIMARY KEY;
# 添加索引
ALTER TABLE tb_name ADD (UNIQUE KEY|INDEX) index_name(fieldName)
# 删除索引
ALTER TABLE tb_name DROP [UNIQUE] INDEX index_name
```

### 索引

```sql
# 查看数据表的索引
SHOW INDEXES FROM tb_name;
# 创建索引
CREATE INDEX index_name ON tb_name(col_name);
# 创建表的同时创建索引
CREATE TABLE demo(
    id INT,
    INDEX(id)
);
```

### 数据操作

#### 添加记录

```sql
INSERT [INTO] tb_name[(col_name,...)] {VALUE|VALUES}(col_value,....);
# 不指定字段列表: 需要按照建表时的字段顺序依次赋值
INSERT INTO tb_name VALUE(col_value,...);
# 指定字段列表
INSERT tb_name(col_name,...) VALUES(col_value,...);
# 同时添加多条记录
INSERT INTO tb_name VALUES(...),(...);
#
INSERT INTO tb_name SET col_name=col_value,...;
INSERT INTO tb_name(col_name,...) SELECT col_name,... FROM tb_name [WHERE CONDITION];
```

#### 更新记录

```sql
UPDATE tb_name SET col_name=col_value,... [WHERE CONDITION];
```

#### 删除记录

```sql
DELETE FROM tb_name [WHERE CONDITION];

# 清空数据表：清空记录，重置 AUTO_INCREMENT
TRUNCATE [TABLE] tb_name;
```

#### 查询记录

```sql
SELECT expression,... FROM [db_name.]tb_name [WHERE CONDITION] [GROUP BY {col_name|position} [WITH ROLLUP] HAVING condition] [ORDER BY {col_name|position|expression} {desc|asc}] [LIMIT num]

# 查询所有记录的所有字段
SELECT * FROM tb_name;
# 查询所有记录的指定字段
SELECT col_name,... FROM tb_name;
# 给字段/数据表起别名
SELECT col_name [AS] new_col_name,... FROM tb_name [AS] new_tb_name;
SELECT tb_name.col_name,... FROM tb_name;


WHERE CONDITION         筛选符合条件的记录
    比较运算符
        WHERE col_name >|<|>=|<=|!=|<>|=|<=> value
    范围限定
        WHERE col_name [NOT] BETWEEN min_value AND max_value
    集合限定
        WHERE col_name IN (value,...)
    逻辑运算符
        逻辑与  WHERE expr1 AND expr2 AND expr3...
        逻辑或  WHERE expr1 OR expr2 OR expr3...
    模糊匹配
        WHERE col_name [NOT] LIKE 'value'
        %   任意长度的任意字符
        _   单个任意字符
    检测 NULL 值
        WHERE col_name IS [NOT] NULL
        WHERE col_name<=>NULL
        WHERE col_name<>NULL

GROUP BY col_name       分组：把查询结果按照指定的列进行分组（值相同的记录属于同一个分组），最终的查询结果中一个分组只显示其中一条记录
    GROUP_CONCAT(col_name)  查看分组中某个字段的所有值
    聚合函数
        COUNT(*)            统计记录的总数
        COUNT(col_name)     统计记录的总数，col_name=NULL 的记录不计入
        SUM(col_name)       求和
        MAX(col_name)       最大值
        MIN(col_name)       最小值
        AVG(col_name)       平均值
    WITH ROLLUP             在查询结果中追加一条记录，内容是原始查询结果的总和

HAVING CONDITION        对分组结果进行二次筛选

ORDER BY col_name [DESC|ASC],[col_name [DESC|ASC]]    对查询结果进行排序
ORDER BY RAND()     对查询结果进行随机排序


LIMIT offset,n         显示结果集的从 offset 开始的前 n 条记录，offset 从0开始
UPDATE/DELETE 时 LIMIT 只支持一个参数
```

#### 多表查询

```sql
# 笛卡尔积
# 内连接：查询两个表中符合连接条件的记录
SELECT col_name,... FROM tb_name1 INNER JOIN tb_name2 ON CONDITION
# 外连接
    # 左外连接：先显示左表中的全部记录，再去右表中查询符合条件的记录，不符合的以 NULL 替代
    SELECT col_name,... FROM tb_name1 LEFT [OUTER] JOIN tb_name2 ON CONDITION
    # 右外连接：先显示右表中的全部记录，再去左表中查询符合条件的记录，不符合的以 NULL 替代
    SELECT col_name,... FROM tb_name1 RIGHT [OUTER] JOIN tb_name2 ON CONDITION
```