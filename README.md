# docker-lnmp

使用 Docker-Compose部署lnmp开发环境

### 目录结构
```
docker-lnmp
|----log                   日志目录
|--------nginx                nginx日志目录
|--------php                  php日志目录
|----nginx                 nginx配置文件目录
|--------base                 基础配置文件目录（毋需修改）
|--------conf.d               自动加载配置文件目录
|--------vhost                用户自定义配置文件目录
|--------nginx.conf           nginx核心配置文件
|--------nginx.conf.bak       nginx配置文件模版
|----php                   php配置文件目录
|--------74                   php7.4配置文件目录
|----www                   用户项目目录
|--------default              默认项目目录
|----README.md             说明文件
|----docker-compose.yml    docker compose 配置文件
```

### 准备
开始之前需在本地安装docker及docker-compose

### 安装

```shell
# 克隆项目
git clone git@github.com:firekylin1993/docker-lnmp.git
# 进入目录
cd docker-lnmp
# 容器编排
docker-compose -f lnmp-compose.yml up -d --force-recreate
```

### 效果
本地访问 http://127.0.0.1:8080/ 出现字样
```text
Welcome lnmp
```
表示部署成功

### 自定义项目流程
```
1、在www目录下放入自己的项目目录
|----www
|--------your app          

2、在vhost目录下拷贝一份conf配置，并将文件尾缀改为conf
|--------vhost           
|--------your-app.conf

3、修改lnmp-compose.yml文件nginx.port暴露端口映射，并确保 端口 与 your-app.conf  listen 端口一致
services:
  nginx:
    ports:
      - "8082:82" //此处以82端口为例

4、重启nginx，浏览器访问http://127.0.0.1:8082/ 即可
docker restart c_nginx

5、若需要域名访问
修改your-app.conf文件 server_name 改为 your-app.com

6、本地hosts文件新增
127.0.0.1 lumen-app.com

7、重启nginx，浏览器访问http://your-app.com:8082/ 即可
docker restart c_nginx

8、附加，若修改了php.ini文件，若要生效需重启php
docker restart php74
```