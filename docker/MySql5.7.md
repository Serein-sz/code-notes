# 单体架构
## 拉取镜像

```
docker pull mysql:5.7
```
## 创建容器
```
docker run --env=MYSQL_ROOT_PASSWORD=Caonima3344 --volume=/opt/docker_config/mysql/log/:/var/log/mysql --volume=/opt/docker_config/mysql/data/:/var/lib/mysql --volume=/opt/docker_config/mysql/conf:/etc/mysql/conf.d --volume=/var/lib/mysql --privileged -p 3308:3306 --restart=no --runtime=runc -t -d mysql:latest
```
## 创建配置文件，解决中文编码问题
```
vim /home/mysql/conf/my.cnf

[client]
default_character_set=utf8
[mysqld]
collation_server=utf8_general_ci
character_set_server=utf8
```

# 主从复制
(master主节点由前文单体架构充当)
## master
### 修改master配置文件
vim /home/mysql/conf/my.cnf
```
[client]
## 设置编码
default_character_set=utf8
[mysqld]
## 设置编码
collation_server=utf8_general_ci
character_set_server=utf8
## 设置server_id，同一局域网中需要唯一
server_id=101
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql
## 开启二进制日志功能
log-bin=mall-mysql-bin
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed
## 二进制日志过期清理时间。默认值为0，表示不自动清理。
expire_logs_days=7
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062
```

### 重启容器
```
docker restart serein_mysql
docker ps
```
### 进入容器执行
```
docker exec -it serein_mysql /bin/bash
mysql -uroot -p
CREATE USER 'slave'@'%' IDENTIFIED BY '123456'
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';
```

## slave
### 复制挂载文件
```
cp -r mysql slave_mysql
rm -rf /home/slave_mysql/data/* //清空,否则error
rm -rf /home/slave_mysql/log/* //清空error
```
### 修改slave配置文件
```
vim /home/slave_mysql/conf/my.cnf

[client]
## 设置编码
default_character_set=utf8
[mysqld]
## 设置编码
collation_server=utf8_general_ci
character_set_server=utf8
## 设置server_id，同一局域网中需要唯一
server_id=102
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql
## 开启二进制日志功能
log-bin=mall-mysql-slave1-bin
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed
## 二进制日志过期清理时间。默认值为0，表示不自动清理。
expire_logs_days=7
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062
## relay_log配置中继日志
relay_log=mall-mysql-relay-bin
## log_slave_updates表示slave将复制事件写进自己的二进制日志
log_slave_updates=1
## slave设置为只读（具有super权限的用户除外）
read_only=1
```

### 新建容器实例
```
docker run -itd -p 3307:3306 --name serein_slave_mysql --privileged=true -v /home/slave_mysql/log/:/var/log/mysql -v /home/slave_mysql/data/:/var/lib/mysql -v /home/slave_mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=Caonima3344 mysql:5.7
```

### 进入mysql-slave容器
```
docker exec -it mysql-slave /bin/bash
mysql -uroot -proot
```
### 在从数据库中配置主从复制
```
change master to master_host='124.222.89.159', master_user='slave', master_password='123456', master_port=3306, master_log_file='mall-mysql-bin.000001', master_log_pos=617, master_connect_retry=30;
```

```
show slave status \G;
```