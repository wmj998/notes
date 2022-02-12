## MySQL

```shell
wget https://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm
rpm -ivh mysql80-community-release-el7-1.noarch.rpm
yum install mysql-server

rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022

systemctl list-unit-files|grep mysqld  # 查看开机启动
systemctl enable mysqld.service
ps -ef|grep mysql
systemctl start mysqld.service

mysqld --initialize  # 初始化
grep 'temporary password' /var/log/mysqld.log  # 密码
mysql -uroot -p
alter user 'root'@'localhost' identified by '12345678';
SHOW VARIABLES LIKE 'validate_password%';
set global validate_password.policy=0;
set global validate_password.length=1;

use mysql;
update user set host = '%' where user = 'root';
flush privileges;
```

https://www.jianshu.com/p/224a891932d8



## RabbitMQ

```shell
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
yum install erlang
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
yum install rabbitmq-server

service rabbitmq-server start
ps -ef|grep rabbitmq-server
systemctl enable rabbitmq-server
systemctl list-unit-files|grep rabbitmq-server

rabbitmqctl add_user rabbitmq 123456
rabbitmqctl set_user_tags rabbitmq administrator
rabbitmqctl set_permissions -p "/" rabbitmq ".*" ".*" ".*"
rabbitmqctl  list_users

rabbitmq-plugins enable rabbitmq_management  # 浏览器插件
```

https://cloud.tencent.com/developer/article/1447179



## Python

```shell
yum -y install gcc
yum -y install zlib*
yum -y install libffi-devel
yum -y install openssl-devel
# yum -y install readline-devel

wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
tar -zxvf Python-3.8.10.tgz
cd Python-3.8.10

------------------------------------------------------------------------------------
# mkdir -p /usr/local/python3.8
# ./configure --prefix=/usr/local/python3.8
./configure
make && make install

ln -s /usr/local/python3.8/bin/python3 /usr/bin/python3
ln -s /usr/local/python3.8/bin/pip3 /usr/bin/pip3

------------------------------------------------------------------------------------
mkdir -p /opt/python3.8
./configure --prefix=/opt/python3.8  # 指定安装目录为/opt/python3.8
make && make install

vim /etc/profile   # 系统环境变量配置文件
PATH=/opt/python3.8/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
source /etc/profile
```

https://zhuanlan.zhihu.com/p/137904053

