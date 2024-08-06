# 拉取镜像
```
docker pull nacos/nacos-server:v2.1.1
```
 
# 数据库建立nacos用户及数据库
```
CREATE USER 'nacos'@'%' IDENTIFIED BY 'nacos';
CREATE DATABASE nacos CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
GRANT ALL PRIVILEGES ON nacos.* TO 'nacos'@'%';
FLUSH PRIVILEGES;
```

 
# 插入nacos-sql 
https://github.com/alibaba/nacos/blob/2.1.1/distribution/conf/nacos-mysql.sql
 
```
docker run -d --name nacos -p 8848:8848 -p 9848:9848 -p 9849:9849 -e MODE=standalone -e SPRING_DATASOURCE_PLATFORM=mysql  -e MYSQL_SERVICE_HOST=124.222.89.159 -e MYSQL_SERVICE_PORT=3308 -e MYSQL_SERVICE_USER=nacos -e MYSQL_SERVICE_PASSWORD=nacos -e MYSQL_SERVICE_DB_NAME=nacos -e MYSQL_SERVICE_DB_PARAM="characterEncoding=utf8&connectTimeout=10000&socketTimeout=30000&autoReconnect=true&serverTimezone=UTC" nacos/nacos-server:v2.1.1
```

# 修改配置文件