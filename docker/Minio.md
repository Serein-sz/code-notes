# Minio

## 拉取镜像

docker pull minio/minio:latest

## 创建容器

```shell
sudo  docker run -p 9000:9000 -p 9001:9001  \
  -d --restart=always  --user=root --privileged=true \
  -e "MINIO_ROOT_USER=serein" \
  -e "MINIO_ROOT_PASSWORD=caonima3344" \
  -v/opt/docker_config/minio/data:/data \
  -v/opt/docker_config/minio/config:/root/.minio \
  --name=minio  minio/minio server  /data --console-address ":9001" --address ":9000"
```



