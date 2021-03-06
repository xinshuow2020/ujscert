# 江苏大学网络安全漏洞响应平台

## 运行

### 环境准备

数据库使用安装了 zhparser 插件的 postgresql，可以通过 dockerhub 中的 codecolorist/zhparser-docker 直接获取，也可从 [zhparser-docker 源码](https://github.com/chichou/zhparser-docker/) 构建。前端库需要使用 npm 进行安装，但 node 不是生产环境所必须的。可先在具有 node 环境的计算机上执行 `npm install`，随后复制 `web/node_modules` 目录即可。

1. 安装 docker 和 [docker-compose](https://docs.docker.com/compose/)
2. 安装 git, 克隆仓库
3. 源码构建或 `docker pull zhparser-docker`，使用 docker tag 重命名 image 为 zhparser-docker
4. `docker-compose build && docker-compose up -d` 运行

第一次启动需要初始化数据库和安装前端依赖：

```
docker-compose run --rm web python manage.py migrate
cd web && npm install
```

添加超级管理员（可选）

`docker-compose run --rm web python manage.py createsuperuser`，随后按照向导完成添加。

## 开发环境

开发配置文件为 `docker-compose.yml`。部署前复制 `production-example.yml` 为 `docker-compose-production.yml`, 按需求修改配置。

使用 `docker-compose up` 可直接运行测试环境。
使用 `docker-compose --file=docker-compose-production.yml` 可指定配置文件, 实现生产环境变量的切换。

## 目录结构

* `docker/data` docker 的数据持久化
* `docker/nginx` `docker/postgres` `docker/web` 分别对应 nginx, 数据库, Python web 的 Dockerfile
* `docker/web/ca` 存储 CA 私钥和证书, 服务器端私钥和证书
* `web` Django 项目源码
* `web/assets` 运行 Django collectstatic 之后的目的目录
* `node_modules` 使用 npm 安装的前端依赖项
* `templates` Django 模板
* `ujscert` 业务逻辑实现

## 生产环境

可参考 `production-example.yml`, 根据需要自行修改 docker-compose-production.yml 的配置。
docker-compose-production.yml 中实例名和对应的服务如下

* `web_prod` 生产环境的 Python web
* `nginx` nginx, 做静态文件服务和应用反向代理
* `db_prod` 生产环境数据库

## TODO

* 礼品兑换
* 漏洞响应时间线
* 用户手册
* 社区和站内信