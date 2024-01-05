[English](README.md) | **简体中文**

## 🚀 快速入门

### 第一步：克隆仓库

确保服务器安装了 Git，否则需要先安装 `git` 软件：

```bash
git clone https://github.com/seatonjiang/monolith.git
```

### 第二步：编辑配置

进入项目文件夹：

```bash
cd monolith/
```

重命名环境配置文件（如果不执行此命令，Docker Compose 会因为没有环境变量报错）：

```bash
cp env.example .env
vi .env
```

检查以下环境配置：

```ini
# PHP 预装扩展库
PHP_EXTENSIONS=redis,memcached,opcache,pdo_mysql,mysqli,zip,gd,imagick,bz2,exif,bcmath,intl,mcrypt,ioncube_loader

# PHP 环境类型
PHP_ENVIRONMENT=production

# MariaDB 数据库名称
DB_MARIADB_DATABASE_NAME=monolith

# phpMyAdmin 访问端口
PHPMYADMIN_WEB_PORT=28080
```

### 第三步：修改密码

修改 `secrets` 目录的文件，`mariadb-root-pwd` 文件是 MariaDB 管理员密码，`mariadb-user-name` 文件是 MariaDB 用户名称，`mariadb-user-pwd` 文件是 MariaDB 用户密码。

### 第四步：构建容器

构建并后台运行所有容器：

```bash
docker compose up -d
```

### 第五步：网站浏览

本地开发环境，使用 localhost 访问网站；线上生产环境，使用服务器 IP 访问网站。

例如默认页面是 `http://localhost`，phpMyAdmin 页面是 `http://localhost:28080`

## 📂 目录结构

下面是整个项目的文件夹结构，文件夹中的文件省略显示：

```bash
monolith
├── data
│   ├── composer               Composer 数据目录
│   ├── mariadb                MariaDB 数据目录
│   └── redis                  Redis 数据目录
├── logs
│   ├── openresty              OpenResty 日志目录
│   └── php                    PHP 日志目录
├── secrets
│   ├── mariadb-root-pwd       MariaDB 管理员密码
│   ├── mariadb-user-name      MariaDB 用户名称
│   └── mariadb-user-pwd       MariaDB 用户密码
├── services
│   ├── mariadb                MariaDB 配置目录
│   ├── openresty              OpenResty 配置目录
│   ├── php                    PHP 配置目录
│   └── redis                  Redis 配置目录
├── wwwroot                    Web 服务根目录
├── compose.yaml               Docker Compose 配置文件
└── env.smaple                 环境配置示例文件
```

## 💻 管理命令

### 管理容器

```bash
# 构建并后台运行所有容器
docker compose up -d

# 构建并后台运行指定容器
docker compose up -d openresty php mariadb

# 停止所有容器并移除网络
docker compose down

# 管理指定服务（此处以 PHP 为例）
docker compose start php            # 启动服务
docker compose stop php             # 停止服务
docker compose restart php          # 重启服务
docker compose build php            # 重新构建服务
```

### 进入容器

运维过程中经常会使用 `docker exec -it` 进入容器，下面是常用的命令：

```bash
# 进入运行中的 PHP 容器
docker exec -it php /bin/sh

# 进入运行中的 Nginx 容器
docker exec -it openresty /bin/sh

# 进入运行中的 Redis 容器
docker exec -it redis /bin/sh

# 进入运行中的 Memcached 容器
docker exec -it memcached /bin/sh

# 进入运行中的 phpMyAdmin 容器
docker exec -it phpmyadmin /bin/bash

# 进入运行中的 Mariadb 容器
docker exec -it mariadb /bin/bash
```

## 📚 常见问题

### Nginx 重启服务

修改 Nginx 配置文件之后，需要执行此命令使之生效：

```bash
docker exec -it openresty nginx -s reload
```

### PHP 安装扩展

使用 `install-php-extensions` 安装 PHP 扩展，需要在 `.env` 配置文件中修改 `PHP_EXTENSIONS` 变量，然后重新构建 PHP 容器。

```bash
docker compose build php
```

> 支持的扩展列表：https://github.com/mlocati/docker-php-extension-installer#supported-php-extensions

### Composer 更换镜像源

第一次执行命令前，建议先更换镜像源（此处以腾讯云镜像为例，内网机器可使用 `mirrors.tencentyun.com`）：

```bash
docker exec -it php /bin/sh
composer config -g repos.packagist composer https://mirrors.tencent.com/composer/
```

### PHP 开启慢脚本日志

修改 `services/php/www.conf` 文件，找到下面两行内容并将注释删除（生产环境建议关闭）：

```ini
slowlog = /var/log/php/slowlog.log
request_slowlog_timeout = 3
```

### MariaDB 开启慢查询日志

修改 `services/mariadb/mariadb.cnf` 文件，将以下两行参数改为 1（生产环境需要修改为 0）：

```ini
slow_query_log = 1
log_queries_not_using_indexes = 1
```

### Redis 设置密码

修改 `services/redis/redis.conf` 文件，找到 `requirepass` 参数并修改密码：

```ini
requirepass foobared
```

## 🤝 参与共建

我们欢迎所有的贡献，你可以将任何想法作为 Pull requests 或 Issues 提交，顺颂商祺！

## 📃 开源许可

项目基于 MIT 许可证发布，详细说明请参阅 [LICENCE](https://github.com/seatonjiang/monolith/blob/main/LICENSE) 文件。
