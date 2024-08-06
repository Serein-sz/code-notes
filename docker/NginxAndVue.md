## 拉取镜像
docker pull nginx

## 修改挂载配置文件
```
server {
  listen 80;
  server_name 112.124.89.224;

  root /usr/share/nginx/html;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
  }
}
```

## 创建Dockerfile

```dockerfile
FROM nginx:latest
COPY dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

