## 拉取镜像
```
docker pull redis
```



## 创建容器
```
docker run -p 6379:6379 --name redis --privileged=true --sysctl net.core.somaxconn=1024  -v /opt/docker_config/redis/redis.conf:/etc/redis/redis.conf -v /opt/docker_config/redis/data:/data -d redis redis-server /etc/redis/redis.conf --requirepass caonima3344 --bind 0.0.0.0
```

