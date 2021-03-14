---
title: 部署 vue 项目
date: 2021-02-28 00:00
category: Vue
---

# 通用指南

如果你用 Vue CLI 处理静态资源并和后端框架一起作为部署的一部分，那么你需要的仅仅是确保 Vue CLI 生成的构建文件在正确的位置，并遵循后端框架的发布方式即可。

如果你独立于后端部署前端应用——也就是说后端暴露一个前端可访问的 API，然后前端实际上是纯静态应用。那么你可以将 dist 目录里构建的内容部署到任何静态文件服务器中，但要确保正确的 `publicPath`。

# Docker (Nginx)

在 Docker 容器中使用 Nginx 部署你的应用。

1. 安装 Docker

   使用官方安装脚本自动安装

   安装命令如下：

   ```bash
   curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
   ```

   也可以使用国内 daocloud 一键安装命令：

   ```bash
   curl -sSL https://get.daocloud.io/docker | sh
   ```

2. 在项目根目录创建 Dockerfile 文件

   ```yaml
   FROM node:10
   COPY ./ /app
   WORKDIR /app
   RUN npm install && npm run build

   FROM nginx
   RUN mkdir /app
   COPY --from=0 /app/dist /app
   COPY nginx.conf /etc/nginx/nginx.conf
   ```

3. 在项目根目录创建 `.dockerignore` 文件

   设置 `.dockerignore` 文件能防止 `node_modules` 和其他中间构建产物被复制到镜像中导致构建问题。

   ```
   **/node_modules
   **/dist
   ```

4. 在项目根目录创建 `nginx.conf` 文件

   `Nginx` 是一个能在 Docker 容器中运行的 HTTP(s) 服务器。它使用配置文件决定如何提供内容、要监听的端口等。参阅 Nginx 设置文档

   以了解所有可能的设置选项。

   下面是一个简单的 Nginx 设置文件，它会在 `80` 端口上提供你的 Vue 项目。`页面未找到` / `404` 错误使用的是 `index.html`，这让我们可以使用基于 `pushState()` 的路由。

   ```conf
   user  nginx;
   worker_processes  1;
   error_log  /var/log/nginx/error.log warn;
   pid        /var/run/nginx.pid;
   events {
     worker_connections  1024;
   }
   http {
     include       /etc/nginx/mime.types;
     default_type  application/octet-stream;
     log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                       '$status $body_bytes_sent "$http_referer" '
                       '"$http_user_agent" "$http_x_forwarded_for"';
     access_log  /var/log/nginx/access.log  main;
     sendfile        on;
     keepalive_timeout  65;
     server {
       listen       80;
       server_name  localhost;
       location / {
         root   /app;
         index  index.html;
         try_files $uri $uri/ /index.html;
       }
       error_page   500 502 503 504  /50x.html;
       location = /50x.html {
         root   /usr/share/nginx/html;
       }
     }
   }
   ```

5. 构建你的 Docker 镜像

   ```shell
   docker build . -t my-app
   # Sending build context to Docker daemon  884.7kB
   # ...
   # Successfully built 4b00e5ee82ae
   # Successfully tagged my-app:latest
   ```

6. 运行你的 Docker 镜像

   这个例子基于官方 Nginx 镜像，因此已经设置了日志重定向并关闭了自我守护进程。它也提供了其他有利于 Nginx 在 Docker 容器中运行的默认设置。更多信息参阅 Nginx Docker 仓库。

   ```bash
   docker run -d -p 8080:80 my-app
   curl localhost:8080
   ```

---

参考 （[部署｜ CLI](https://cli.vuejs.org/zh/guide/deployment.html#docker-nginx)）
