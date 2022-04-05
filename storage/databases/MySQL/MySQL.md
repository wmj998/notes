# MySQL



## 数据库的基本操作

> + 查看数据库
>
> ```
> SHOW DATABASES [LIKE '数据库名'];
> ```
>
> > - information_schema：主要存储了系统中的一些数据库对象信息，比如用户表信息、列信息、权限信息、字符集信息和分区信息等。
> > - mysql：MySQL 的核心数据库，类似于 SQL Server 中的 master 表，主要负责存储数据库用户、用户访问权限等 MySQL 自己需要使用的控制和管理信息。常用的比如在 mysql 数据库的 user 表中修改 root 用户密码。
> > - performance_schema：主要用于收集数据库服务器性能参数。
> > - sakila：MySQL 提供的样例数据库，该数据库共有 16 张表，这些数据表都是比较常见的，在设计数据库时，可以参照这些样例数据表来快速完成所需的数据表。
> > - sys：sys 数据库主要提供了一些视图，数据都来自于 performation_schema，主要是让开发者和使用者更方便地查看性能问题。
> > - world：world 数据库是 MySQL 自动创建的数据库，该数据库中只包括 3 张数据表，分别保存城市，国家和国家使用的语言等内容。
>
> + 创建数据库
>
> ```
> CREATE DATABASE [IF NOT EXISTS] <数据库名>;
> ```
>
> + 删除数据库
>
> ```
> DROP DATABASE [ IF EXISTS ] <数据库名>;
> ```
>
> + 选择数据库
>
> ```
> USE <数据库名>
> ```



## 存储引擎

| 存储引擎  | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| ARCHIVE   | 用于数据存档的引擎，数据被插入后就不能在修改了，且不支持索引。 |
| CSV       | 在存储数据时，会以逗号作为数据项之间的分隔符。               |
| BLACKHOLE | 会丢弃写操作，该操作会返回空内容。                           |
| FEDERATED | 将数据存储在远程数据库中，用来访问远程表的存储引擎。         |
| InnoDB    | 具备外键支持功能的事务处理引擎                               |
| MEMORY    | 置于内存的表                                                 |
| MERGE     | 用来管理由多个 MyISAM 表构成的表集合                         |
| MyISAM    | 主要的非事务处理存储引擎                                     |
| NDB       | MySQL 集群专用存储引擎                                       |



## 数据表的基本操作

> + 创建数据表
>
> ```
> CREATE TABLE <表名> ([表定义选项])[表选项][分区选项])
> 
> PRIMARY KEY (<列名>) #主键约束
> FOREIGN KEY (<列名>) REFERENCES <表名> (<列名>) #外键约束
> <表定义选项>：表创建定义，由列名（col_name）、列的定义（column_definition）以及可能的空值说明、完整性约束或表索引组成。
> ```
>
> + 修改数据表
>
> ```
> ALTER TABLE <表名> [修改选项]
> 
> { ADD COLUMN <列名> <类型>
> | CHANGE COLUMN <旧列名> <新列名> <新列类型>
> | ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
> | MODIFY COLUMN <列名> <类型>
> | DROP COLUMN <列名>
> | RENAME TO <新表名>
> | CHARACTER SET <字符集名>
> | COLLATE <校对规则名> }
> ```
>
> + 删除数据表
>
> ```
> DROP TABLE [IF EXISTS] <表名1> [, 表名2, 表名3 ...]
> ```
>
> + 查看数据表结构
>
> ```
> DESCRIBE/DESC <表名>;
> 
> SHOW CREATE TABLE <表名>;
> ```



## 表中数据操作

> + 查询数据表
>
> ```
> SELECT
> {* | <字段列名>}
> [
> FROM <表 1>, <表 2>…
> [WHERE <表达式>
> [GROUP BY <group by definition>
> [HAVING <expression> [{<operator> <expression>}…]]
> [ORDER BY <order by definition>]
> [LIMIT[<offset>,] <row count>]
> ]
> ```
>
> + 字符匹配
>
> ```
> [NOT] LIKE <匹配字符串>
> 
> % 表示任意长度的字符串，长度可以为0
> _ 表示任意一个字符，长度不能为0
> ```
>
> + 排序
>
> ```
> ORDER BY <列名1> [, 列名2, 列名3...] [ASC|DESC] 升序|降序 
> ```
>
> + 分组
>
> ```
> GROUP BY <列名>
> HAVING <表达式> 对分组的结果进行选择，输出满足条件的组
> ```
>
> + 去重
>
> ```
> SELECT DISTINCT <字段名> FROM <表名>;
> ```
>
> + 设置别名
>
> ```
> <表名> [AS] <别名>
> <字段名> [AS] <别名>
> ```
>
> + 限制查询结果的条数
>
> ```
> LIMIT [初始位置,]<记录数>
> ```
>
> + 范围查询
>
> ```
> [NOT] BETWEEN 取值1 AND 取值2
> ```
>
> + 空值查询
>
> ```
> IS [NOT] NULL
> ```
>
> + 交叉连接
>
> ```
> SELECT <字段名> FROM <表1> CROSS JOIN <表2> [WHERE子句]
> 
> SELECT <字段名> FROM <表1>, <表2> [WHERE子句] 
> ```
>
> + 内连接
>
> ```
> SELECT <字段名> FROM <表1> INNER JOIN <表2> [ON子句](连接条件)
> ```
>
> + 外连接
>
> ```
> 左外连接
> SELECT <字段名> FROM <表1> LEFT OUTER JOIN <表2> <ON子句>
> 左连接以“表1”为基表，“表2”为参考表。左连接查询时，可以查询出“表1”中的所有记录和“表2”中匹配连接条件的记录。如果“表1”的某行在“表2”中没有匹配行，那么在返回结果中，“表2”的字段值均为空值（NULL）。
> 
> 右外连接
> SELECT <字段名> FROM <表1> RIGHT OUTER JOIN <表2> <ON子句>
> 右连接以“表2”为基表，“表1”为参考表。右连接查询时，可以查询出“表2”中的所有记录和“表1”中匹配连接条件的记录。如果“表2”的某行在“表1”中没有匹配行，那么在返回结果中，“表1”的字段值均为空值（NULL）。
> ```
>
> + 子查询
>
> ```
> WHERE <表达式> <操作符> (子查询)
> 操作符可以是比较运算符和 IN、NOT IN、EXISTS、NOT EXISTS 等关键字
> ```
>
> + 正则表达式
>
> ```
> 属性名 REGEXP '匹配方式'
> ```
>
> | 选项         | 说明                                  | 例子                                       | 匹配值示例                 |
> | ------------ | ------------------------------------- | ------------------------------------------ | -------------------------- |
> | ^            | 匹配文本的开始字符                    | '^b' 匹配以字母 b 开头的字符串             | book、big、banana、bike    |
> | $            | 匹配文本的结束字符                    | 'st$' 匹配以 st 结尾的字符串               | test、resist、persist      |
> | .            | 匹配任何单个字符                      | 'b.t' 匹配任何 b 和 t 之间有一个字符       | bit、bat、but、bite        |
> | *            | 匹配零个或多个在它前面的字符          | 'f*n' 匹配字符 n 前面有任意个字符 f        | fn、fan、faan、abcn        |
> | +            | 匹配前面的字符 1 次或多次             | 'ba+' 匹配以 b 开头，后面至少紧跟一个 a    | ba、bay、bare、battle      |
> | <字符串>     | 匹配包含指定字符的文本                | 'fa' 匹配包含‘fa’的文本                    | fan、afa、faad             |
> | [字符集合]   | 匹配字符集合中的任何一个字符          | '[xz]' 匹配 x 或者 z                       | dizzy、zebra、x-ray、extra |
> | [^]          | 匹配不在括号中的任何字符              | '[^abc]' 匹配任何不包含 a、b 或 c 的字符串 | desk、fox、f8ke            |
> | 字符串{n,}   | 匹配前面的字符串至少 n 次             | 'b{2}' 匹配 2 个或更多的 b                 | bbb、bbbb、bbbbbbb         |
> | 字符串 {n,m} | 匹配前面的字符串至少 n 次， 至多 m 次 | 'b{2,4}' 匹配最少 2 个，最多 4 个 b        | bbb、bbbb                  |
>
> + 插入数据
>
> ```
> INSERT INTO <表名> [ (<列名1>) [ , … (<列名n>)] ] VALUES (值1) [… , (值n) ];
> 
> INSERT INTO <表名>
> SET <列名1> = <值1>,
>     <列名2> = <值2>,
>     …
> ```
>
> + 更新数据
>
> ```
> UPDATE <表名> SET <修改后的值> [WHERE <需要被修改的值>] 
> 
> UPDATE <表名> SET 字段1 = 值1 [,字段2 = 值2… ] [WHERE 子句 ]
> [ORDER BY 子句] [LIMIT 子句]
> WHERE 子句：可选项。用于限定表中要修改的行。若不指定，则修改表中所有的行。
> ORDER BY 子句：可选项。用于限定表中的行被修改的次序。
> LIMIT 子句：可选项。用于限定被修改的行数。
> ```
>
> + 删除数据
>
> ```
> DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句]
> ORDER BY 子句：可选项。表示删除时，表中各行将按照子句中指定的顺序进行删除。
> WHERE 子句：可选项。表示为删除操作限定删除条件，若省略该子句，则代表删除该表中的所有行。
> LIMIT 子句：可选项。用于告知服务器在控制命令被返回到客户端前被删除行的最大值。
> ```
>
> + 清空数据表
>
> ```
> TRUNCATE [TABLE] 表名
> ```



## 约束

> + 主键约束
>
> ```
> <字段名> <数据类型> PRIMARY KEY [默认值]
> 
> [CONSTRAINT <约束名>] PRIMARY KEY <字段名>
> ```
>
> + 外键约束
>
> ```
> [CONSTRAINT <约束名>] FOREIGN KEY <字段名1> [，字段名2...] REFERENCES <主表名> <主键列1 > [，主键列2...]
> ```
>
> + 唯一约束
>
> ```
> <字段名> <数据类型> UNIQUE
> 
> ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名(unique_列名)> UNIQUE(<列名>);
> 
> ALTER TABLE <表名> DROP INDEX <唯一约束名>;
> ```
>
> + 检查约束
>
> ```
> CHECK(<检查约束，表达式>)
> 
> ALTER TABLE <数据表名> ADD CONSTRAINT <检查约束名(check_列名)> CHECK(<检查约束>)
> 
> ALTER TABLE <数据表名> DROP CONSTRAINT <检查约束名>;
> ```
>
> + 非空约束
>
> ```
> <字段名> <数据类型> NOT NULL;
> 
> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> NOT NULL;
> 
> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;
> ```
>
> + 默认值约束
>
> ```
> <字段名> <数据类型> DEFAULT <默认值>;
> 
> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT <默认值>;
> 
> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;
> ```
>
> + 查看数据表约束
>
> ```
> SHOW CREATE TABLE <数据表名>;
> ```



## 运算符优先级

| 优先级由低到高排列 | 运算符                                                       |
| ------------------ | ------------------------------------------------------------ |
| 1                  | =(赋值运算）、:=                                             |
| 2                  | II、OR                                                       |
| 3                  | XOR                                                          |
| 4                  | &&、AND                                                      |
| 5                  | NOT                                                          |
| 6                  | BETWEEN、CASE、WHEN、THEN、ELSE                              |
| 7                  | =(比较运算）、<=>、>=、>、<=、<、<>、!=、 IS、LIKE、REGEXP、IN |
| 8                  | \|                                                           |
| 9                  | &                                                            |
| 10                 | <<、>>                                                       |
| 11                 | -(减号）、+                                                  |
| 12                 | *、/、%                                                      |
| 13                 | ^                                                            |
| 14                 | -(负号）、〜（位反转）                                       |
| 15                 | !                                                            |



## 视图

> 创建视图
>
> ```
> CREATE VIEW <视图名> AS <SELECT语句>
> ```
>
> 查看视图
>
> ```
> DESCRIBE 视图名；
> DESC 视图名;
> SHOW CREATE VIEW 视图名;
> ```
>
> 修改视图
>
> ```
> ALTER VIEW <视图名> AS <SELECT语句>
> ```
>
> 删除视图
>
> ```
> DROP VIEW <视图名1> [ , <视图名2> …]
> ```



## 索引

> 创建索引
>
> ```
> CREATE <索引名> ON <表名> (<列名> [<长度>] [ ASC | DESC])
> 
> CONSTRAINT PRIMARY KEY [索引类型] (<列名>,…)
> KEY | INDEX [<索引名>] [<索引类型>] (<列名>,…)
> UNIQUE [ INDEX | KEY] [<索引名>] [<索引类型>] (<列名>,…)
> FOREIGN KEY <索引名> <列名>
> 
> ADD INDEX [<索引名>] [<索引类型>] (<列名>,…)
> ADD PRIMARY KEY [<索引类型>] (<列名>,…)
> ADD UNIQUE [ INDEX | KEY] [<索引名>] [<索引类型>] (<列名>,…)
> ADD FOREIGN KEY [<索引名>] (<列名>,…)
> ```
>
> 查看索引
>
> ```
> SHOW INDEX FROM <表名> [ FROM <数据库名>]
> ```
>
> 删除索引
>
> ```
> DROP INDEX <索引名> ON <表名>
> ```



## 存储过程

> 创建存储过程
>
> ```
> CREATE PROCEDURE <过程名> ( [过程参数[,…] ] ) <过程体>
> [过程参数[,…] ] 格式
> [ IN | OUT | INOUT ] <参数名> <类型>
> ```
>
> 查看存储过程
>
> ```
> SHOW PROCEDURE STATUS LIKE 存储过程名;
> ```
>
> 修改储存过程
>
> ```
> ALTER PROCEDURE 存储过程名 [ 特征 ... ]
> 
> 特征指定了存储过程的特性，可能的取值有：
> CONTAINS SQL 表示子程序包含 SQL 语句，但不包含读或写数据的语句。
> NO SQL 表示子程序中不包含 SQL 语句。
> READS SQL DATA 表示子程序中包含读数据的语句。
> MODIFIES SQL DATA 表示子程序中包含写数据的语句。
> SQL SECURITY { DEFINER |INVOKER } 指明谁有权限来执行。
> DEFINER 表示只有定义者自己才能够执行。
> INVOKER 表示调用者可以执行。
> COMMENT 'string' 表示注释信息。
> ```
>
> 删除存储过程
>
> ```
> DROP PROCEDURE [ IF EXISTS ] <过程名>
> ```
>
> 调用存储过程
>
> ```
> CALL sp_name([parameter[...]]);
> sp_name 表示存储过程的名称，parameter 表示存储过程的参数
> ```



## 流程控制语句

> IF 语句，条件判断
>
> ```
> IF search_condition THEN statement_list
>     [ELSEIF search_condition THEN statement_list]...
>     [ELSE statement_list]
> END IF
> ```
>
> CASE 语句，条件判断
>
> ```
> CASE case_value
>     WHEN when_value THEN statement_list
>     [WHEN when_value THEN statement_list]...
>     [ELSE statement_list]
> END CASE
> ```
>
> LOOP 语句，循环操作
>
> ```
> [begin_label:]LOOP
>     statement_list
> END LOOP [end_label]
> ```
>
> LEAVE 语句，跳出循环
>
> ```
> LEAVE label
> ```
>
> ITERATE 语句，跳出本次循环，直接进入下一次循环
>
> ```
> ITERATE label
> ```
>
> REPEAT 语句，条件控制循环
>
> ```
> [begin_label:] REPEAT
>     statement_list
>     UNTIL search_condition
> END REPEAT [end_label]
> ```
>
> WHILE 语句，条件竞争循环
>
> ```
> [begin_label:] WHILE search_condition DO
>     statement list
> END WHILE [end label]
> ```



## 触发器

> 创建触发器
>
> ```
> CREATE <触发器名> < BEFORE | AFTER >
> <INSERT | UPDATE | DELETE >
> ON <表名> FOR EACH Row<触发器主体>
> ```
>
> 查看触发器
>
> ```
> SHOW TRIGGERS;
> ```
>
> 删除触发器
>
> ```
> DROP TRIGGER [ IF EXISTS ] [数据库名] <触发器名>
> ```



## 事务

> 开始事务
>
> ```
> BEGIN;
> 
> START TRANSACTION;
> ```
>
> 提交事务
>
> ```
> COMMIT;
> ```
>
> 回滚（撤销）事务
>
> ```
> ROLLBACK;
> ```



## 查看字符集

```
SHOW VARIABLES LIKE 'character%';
```

| 名称                     | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| character_set_client     | MySQL 客户端使用的字符集                                     |
| character_set_connection | 连接数据库时使用的字符集                                     |
| character_set_database   | 创建数据库使用的字符集                                       |
| character_set_filesystem | MySQL 服务器文件系统使用的字符集，默认值为 binary，不做任何转换 |
| character_set_results    | 数据库给客户端返回数据时使用的字符集                         |
| character_set_server     | MySQL 服务器使用的字符集，建议由系统自己管理，不要人为定义   |
| character_set_system     | 数据库系统使用的字符集，默认值为 utf8，不需要设置            |
| character_sets_dir       | 字符集的安装目录                                             |



查看可用字符集

```
SHOW CHARACTER set;

第一列（Charset）为字符集名称；
第二列（Description）为字符集描述；
第三列（Default collation）为字符集的默认校对规则；
第四列（Maxlen）表示字符集中一个字符占用的最大字节数。
```



## 查看校对规则

```
SHOW VARIABLES LIKE 'collation\_%';

SHOW COLLATION LIKE '***';
```



## 用户

> 创建用户
>
> ```
> CREATE USER <用户(user_name'@'host_name)> [ IDENTIFIED BY [ PASSWORD ] 'password' ] [ ,用户 [ IDENTIFIED BY [ PASSWORD ] 'password' ]]
> ```
>
> 修改用户
>
> ```
> RENAME USER <旧用户(user_name'@'host_name)> TO <新用户(user_name'@'host_name)>
> ```
>
> 删除用户
>
> ```
> DROP USER <用户1(user_name'@'host_name)> [ , <用户2> ]…
> 
> DELETE FROM <表名> WHERE Host='hostname' AND User='username';
> ```
>
> 查看用户权限
>
> ```
> SHOW GRANTS FOR 'username'@'hostname';
> ```
>
> 用户授权
>
> ```
> GRANT priv_type [(column_list)] ON database.table
> TO user [IDENTIFIED BY [PASSWORD] 'password']
> [, user[IDENTIFIED BY [PASSWORD] 'password']] ...
> [WITH with_option [with_option]...]
> ```
>
> 其中：
>
> - priv_type 参数表示权限类型；
> - columns_list 参数表示权限作用于哪些列上，省略该参数时，表示作用于整个表；
> - database.table 用于指定权限的级别；
> - user 参数表示用户账户，由用户名和主机名构成，格式是“'username'@'hostname'”；
> - IDENTIFIED BY 参数用来为用户设置密码；
> - password 参数是用户的新密码。
>
>
> WITH 关键字后面带有一个或多个 with_option 参数。这个参数有 5 个选项，详细介绍如下：
>
> - GRANT OPTION：被授权的用户可以将这些权限赋予给别的用户；
> - MAX_QUERIES_PER_HOUR count：设置每个小时可以允许执行 count 次查询；
> - MAX_UPDATES_PER_HOUR count：设置每个小时可以允许执行 count 次更新；
> - MAX_CONNECTIONS_PER_HOUR count：设置每小时可以建立 count 个连接;
> - MAX_USER_CONNECTIONS count：设置单个用户可以同时具有的 count 个连接。
>
>
> MySQL 中可以授予的权限有如下几组：
>
> - 列权限，和表中的一个具体列相关。例如，可以使用 UPDATE 语句更新表 students 中 name 列的值的权限。
> - 表权限，和一个具体表中的所有数据相关。例如，可以使用 SELECT 语句查询表 students 的所有数据的权限。
> - 数据库权限，和一个具体的数据库中的所有表相关。例如，可以在已有的数据库 mytest 中创建新表的权限。
> - 用户权限，和 MySQL 中所有的数据库相关。例如，可以删除已有的数据库或者创建一个新的数据库的权限。
>
>
> 对应地，在 GRANT 语句中可用于指定权限级别的值有以下几类格式：
>
> - *：表示当前数据库中的所有表。
> - *.*：表示所有数据库中的所有表。
> - db_name.*：表示某个数据库中的所有表，db_name 指定数据库名。
> - db_name.tbl_name：表示某个数据库中的某个表或视图，db_name 指定数据库名，tbl_name 指定表名或视图名。
> - db_name.routine_name：表示某个数据库中的某个存储过程或函数，routine_name 指定存储过程名或函数名。
> - TO 子句：如果权限被授予给一个不存在的用户，MySQL 会自动执行一条 CREATE USER 语句来创建这个用户，但同时必须为该用户设置密码。
>
> | 权限名称                       | 对应user表中的字段    | 说明                                                         |
> | ------------------------------ | --------------------- | ------------------------------------------------------------ |
> | SELECT                         | Select_priv           | 表示授予用户可以使用 SELECT 语句访问特定数据库中所有表和视图的权限。 |
> | INSERT                         | Insert_priv           | 表示授予用户可以使用 INSERT 语句向特定数据库中所有表添加数据行的权限。 |
> | DELETE                         | Delete_priv           | 表示授予用户可以使用 DELETE 语句删除特定数据库中所有表的数据行的权限。 |
> | UPDATE                         | Update_priv           | 表示授予用户可以使用 UPDATE 语句更新特定数据库中所有数据表的值的权限。 |
> | REFERENCES                     | References_priv       | 表示授予用户可以创建指向特定的数据库中的表外键的权限。       |
> | CREATE                         | Create_priv           | 表示授权用户可以使用 CREATE TABLE 语句在特定数据库中创建新表的权限。 |
> | ALTER                          | Alter_priv            | 表示授予用户可以使用 ALTER TABLE 语句修改特定数据库中所有数据表的权限。 |
> | SHOW VIEW                      | Show_view_priv        | 表示授予用户可以查看特定数据库中已有视图的视图定义的权限。   |
> | CREATE ROUTINE                 | Create_routine_priv   | 表示授予用户可以为特定的数据库创建存储过程和存储函数的权限。 |
> | ALTER ROUTINE                  | Alter_routine_priv    | 表示授予用户可以更新和删除数据库中已有的存储过程和存储函数的权限。 |
> | INDEX                          | Index_priv            | 表示授予用户可以在特定数据库中的所有数据表上定义和删除索引的权限。 |
> | DROP                           | Drop_priv             | 表示授予用户可以删除特定数据库中所有表和视图的权限。         |
> | CREATE TEMPORARY TABLES        | Create_tmp_table_priv | 表示授予用户可以在特定数据库中创建临时表的权限。             |
> | CREATE VIEW                    | Create_view_priv      | 表示授予用户可以在特定数据库中创建新的视图的权限。           |
> | EXECUTE ROUTINE                | Execute_priv          | 表示授予用户可以调用特定数据库的存储过程和存储函数的权限。   |
> | LOCK TABLES                    | Lock_tables_priv      | 表示授予用户可以锁定特定数据库的已有数据表的权限。           |
> | ALL 或 ALL PRIVILEGES 或 SUPER | Super_priv            | 表示以上所有权限/超级权限                                    |
>
> 删除用户权限
>
> ```
> REVOKE priv_type [(column_list)]...
> ON database.table
> FROM user [, user]...
> ```
>
> 登录和退出服务器
>
> ```
> mysql -h hostname|hostlP -p port -u username -p DatabaseName -e "SQL语句"
> 
> QUIT;
> ```
>
> 修改密码
>
> ```
> root 修改普通用户密码
> SET PASSWORD FOR 'username'@'hostname' = PASSWORD ('newpwd');
> username 参数是普通用户的用户名，hostname 参数是普通用户的主机名，newpwd 是要更改的新密码。
> 新密码必须使用 PASSWORD() 函数来加密，如果不使用 PASSWORD() 加密，也会执行成功，但是用户会无法登录。
> 
> GRANT USAGE ON *.* TO 'user'@’hostname’ IDENTIFIED BY 'newpwd';
> 
> 普通用户修改密码
> SET PASSWORD = PASSWORD('newpwd');
> ```
>
> 修改root密码
>
> ```
> mysqladmin -u username -h hostname -p password "newpwd"
> 
> SET PASSWORD = PASSWORD ("rootpwd");
> ```



## 备份与恢复

> 备份数据库
>
> ```
> mysqldump -u username -p dbname [tbname ...]> filename.sql
> 
> mysqldump -u username -P --all-databases>filename.sql
> ```
>
> 恢复数据库
>
> ```
> mysql -u username -P [dbname] < filename.sql
> ```
>
> 导出数据表
>
> ```
> SELECT 列名 FROM table [WHERE 语句] INTO OUTFILE '目标文件'[OPTIONS]
> ```
>
> [OPTIONS] 为可选参数选项，OPTIONS 部分的语法包括 FIELDS 和 LINES 子句，其常用的取值有：
>
> - FIELDS TERMINATED BY '字符串'：设置字符串为字段之间的分隔符，可以为单个或多个字符，默认情况下为制表符‘\t’。
> - FIELDS [OPTIONALLY] ENCLOSED BY '字符'：设置字符来括上 CHAR、VARCHAR 和 TEXT 等字符型字段。如果使用了 OPTIONALLY 则只能用来括上 CHAR 和 VARCHAR 等字符型字段。
> - FIELDS ESCAPED BY '字符'：设置如何写入或读取特殊字符，只能为单个字符，即设置转义字符，默认值为‘\’。
> - LINES STARTING BY '字符串'：设置每行开头的字符，可以为单个或多个字符，默认情况下不使用任何字符。
> - LINES TERMINATED BY '字符串'：设置每行结尾的字符，可以为单个或多个字符，默认值为‘\n’ 。
>
> 导入数据
>
> ```
> LOAD DATA INFILE '目标文件'
> ```



## 日志

日志是数据库的重要组成部分，主要用来记录数据库的运行情况、日常操作和错误信息。

- 二进制日志：该日志文件会以二进制的形式记录数据库的各种操作，但不记录查询语句。
- 错误日志：该日志文件会记录 MySQL 服务器的启动、关闭和运行错误等信息。
- 通用查询日志：该日志记录 MySQL 服务器的启动和关闭信息、客户端的连接信息、更新、查询数据记录的 SQL 语句等。
- 慢查询日志：记录执行事件超过指定时间的操作，通过工具分析慢查询日志可以定位 MySQL 服务器性能瓶颈所在。





## MySQL教程

http://c.biancheng.net/mysql/
