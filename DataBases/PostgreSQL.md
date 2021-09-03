# PostgreSQL



## 登录

```
su - postgres
psql
```



## 查看数据库

```
\l
```



## 选择数据库

```
\c db_name
```



## 查看数据表

```
select * from pg_tables;

select * from pg_tables where schemaname = 'public';
```



## 查看数据表结构

```
\d table_name
```



## 退出

```
\q
```