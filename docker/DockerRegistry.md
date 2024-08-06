# 私有Docker 镜像仓库

## 在服务器上启动docker registry

**1.拉取镜像：** 

```shell
docker pull registry:latest
```

**2.安装htpasswd工具：**

```shell
sudo yum install httpd-tools
```

**3.生成htpasswd文件：**

```shell
sudo htpasswd -Bc /opt/docker_config/docker-registry/htpasswd username
```

**4.将htpasswd文件挂载到Registry容器**：

```shell
docker run -d -p 5000:5000 --restart=always --name registry \
  -v /opt/docker_config/docker-registry:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
  registry:latest
```

**5.验证Docker Registry：**

```shell
curl http://your-registry-domain.com:5000/v2/
```

如果一切正常，你应该会收到一个类似于`{}`的响应。



## 客户端配置

**1.配置Docker客户端以允许不安全的注册表：**

```json
//通常是/etc/docker/daemon.json
{
  "insecure-registries": ["112.125.89.224:5000"]
}
```

**2.登录docker registry**

```
docker login your-registry-url
```

**3.为镜像添加标签**：

在推送之前，需要为本地的镜像添加一个标签，将其命名为Registry的地址。示例：

```shell
docker tag local-image your-registry-url/remote-image:tag
```

这里 `local-image` 是本地镜像的名称，`your-registry-url` 是私有Registry的地址，`remote-image` 是将要在Registry中创建的镜像的名称，`tag` 是镜像的标签.

**3.推送镜像**：

使用 `docker push` 命令将标记过的镜像推送到Registry。示例：

```shell
docker push your-registry-url/remote-image:tag
```

这将推送标记为 `your-registry-url/remote-image:tag` 的镜像到Registry。