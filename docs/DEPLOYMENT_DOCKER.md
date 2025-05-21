# Docker 部署指南

本指南介绍如何使用 `docker-compose.yml` 一键启动整套服务，包括 Python 设备服务、Java 后端、Vue 前端、MySQL 与 Redis。

## 前置条件
- 安装 [Docker](https://docs.docker.com/get-docker/) 与 [Docker Compose](https://docs.docker.com/compose/install/)。
- 获取本仓库源码：

```bash
git clone <repo-url>
cd xiaozhi-esp32-server
```

## 配置环境变量
在项目根目录下创建 `.env` 文件，可直接复制 `.env.example`：

```bash
cp .env.example .env
```

`.env.example` 中包含了默认的数据库账号密码等参数，可根据实际需求修改。

示例内容如下：

```env
MYSQL_ROOT_PASSWORD=strongpassword
MYSQL_DATABASE=xiaozhi
MYSQL_USER=xiaozhi_user
MYSQL_PASSWORD=userpass
```

可根据实际需求调整数据库密码等参数。

## 自定义配置
复制 `main/xiaozhi-server/config.yaml` 到 `main/xiaozhi-server/data/.config.yaml` 后，可根据需要修改其中内容。该 `data` 目录会被挂载到容器的 `/opt/xiaozhi-esp32-server/data`，升级镜像时依旧保留你的个性化配置。

## 构建并启动服务

```bash
docker compose build
docker compose up -d
```

首次运行会构建各镜像并启动下列服务：
- `mysql_db`：数据库，端口 `3306`。
- `redis_cache`：缓存服务，端口 `6379`。
- `xiaozhi-device-service`：Python 服务，端口 `8000` 与 `8002`。
- `xiaozhi-manager-api`：Java 后端，端口 `8080`。
- `xiaozhi-manager-web`：前端服务，端口 `80`。

浏览器访问 `http://localhost` 即可看到管理界面。

## 常用命令
- 查看运行状态：`docker compose ps`
- 查看日志：`docker compose logs -f xiaozhi-manager-api`
- 停止服务：`docker compose down`

MySQL 与 Redis 数据分别持久化在 `mysql_data` 与 `redis_data` 卷中，可在 `docker-compose.yml` 中修改挂载路径。

