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





```
# 启动，停止，重启服务
systemctl start|stop|restart postgresql-9.4.service

# 查看一个服务的状态
systemctl status postgresql-9.4.service

# 开机是，否启动服务
systemctl enable|disable postgresql-9.4.service

# 查看服务是否开机启动
systemctl is-enabled postgresql-9.4.service
```

