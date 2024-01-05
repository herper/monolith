**English** | [简体中文](README.zh-CN.md)

## 🚀 Quick start

### Step 1: Clone the repository

Make sure Git is installed on your server. If not, you need to install the `git` software first:

```bash
git clone https://github.com/seatonjiang/monolith.git
```

### Step 2: Edit the config

Navigate to the project folder:

```bash
cd monolith/
```

Rename the environment config file (if you don't execute this command, Docker Compose will throw an error due to missing environment variables):

```bash
cp env.example .env
vi .env
```

Check the environment config:

```ini
# PHP pre-installed extensions
PHP_EXTENSIONS=redis,memcached,opcache,pdo_mysql,mysqli,zip,gd,imagick,bz2,exif,bcmath,intl,mcrypt,ioncube_loader

# PHP environment
PHP_ENVIRONMENT=production

# MariaDB database name
DB_MARIADB_DATABASE_NAME=monolith

# phpMyAdmin WEB Port
PHPMYADMIN_WEB_PORT=28080
```

### Step 3: Change password

Modify the files in the `secrets` directory. The `mariadb-root-pwd` file contains the MariaDB root password, the `mariadb-user-name` file contains the MariaDB user name, and the `mariadb-user-pwd` file contains the MariaDB user password.

### Step 4: Build the container

Build and run all containers in the background:

```bash
docker compose up -d
```

### Step 5: Browse the website

For local development environment, access the website using `localhost`. For production environment, use the server IP to access the website.

For example, the default page is `http://localhost`, and the phpMyAdmin page is `http://localhost:28080`.

## 📂 Directory Structure

The folder structure of the entire project, with the files in the folder omitted from the display:

```bash
monolith
├── data
│   ├── composer               Composer data directory
│   ├── mariadb                MariaDB data directory
│   └── redis                  Redis data directory
├── logs
│   ├── openresty              OpenResty log directory
│   └── php                    PHP log directory
├── secrets
│   ├── mariadb-root-pwd       MariaDB administrator password
│   ├── mariadb-user-name      MariaDB username
│   └── mariadb-user-pwd       MariaDB user password
├── services
│   ├── mariadb                MariaDB config directory
│   ├── openresty              OpenResty config directory
│   ├── php                    PHP config directory
│   └── redis                  Redis config directory
├── wwwroot                    Web service root directory
├── compose.yaml               Docker Compose config file
└── env.smaple                 Example environment config file
```

## 💻 Management Commands

### Manage Containers

```bash
# Build and run all containers in the background
docker compose up -d

# Build and run specified containers in the background
docker compose up -d openresty php mariadb

# Stop all containers and remove the network
docker compose down

# Manage a specific service (using PHP as an example here)
docker compose start php            # Start the service
docker compose stop php             # Stop the service
docker compose restart php          # Restart the service
docker compose build php            # Rebuild the service
```

### Accessing Containers

During the maintenance process, it is often necessary to enter containers using `docker exec -it`. Here are some commonly used commands:

```bash
# Execute a command in a running PHP container
docker exec -it php /bin/sh

# Execute a command in a running Nginx container
docker exec -it openresty /bin/sh

# Execute a command in a running Redis container
docker exec -it redis /bin/sh

# Execute a command in a running Memcached container
docker exec -it memcached /bin/sh

# Execute a command in a running phpMyAdmin container
docker exec -it phpmyadmin /bin/bash

# Execute a command in a running Mariadb container
docker exec -it mariadb /bin/bash
```

## 📚 FAQs

### Nginx Restart Services

After change the Nginx config file, you need to execute this command to make it take effect:

```bash
docker exec -it openresty nginx -s reload
```

### PHP install extensions

Installing PHP extensions using `install-php-extensions` requires modifying the `PHP_EXTENSIONS` variable in the `.env` config file and then rebuilding the PHP container.

```bash
docker compose build php
```

> Supported PHP Extensions：https://github.com/mlocati/docker-php-extension-installer#supported-php-extensions

### Composer Mirrors

Before executing the command for the first time, it is recommended to change the mirrors:

```bash
docker exec -it php /bin/sh
composer config -g repos.packagist composer https://mirrors.tencent.com/composer/
```

### PHP Slow Script Log

Modify the `services/php/www.conf` file with two lines and remove the comments (recommended to be turned off for production environments):

```ini
slowlog = /var/log/php/slowlog.log
request_slowlog_timeout = 3
```

### MariaDB Slow Query Log

Modify the `services/mariadb/mariadb.cnf` file by changing two lines to 1 (0 is required for production environments):

```ini
slow_query_log = 1
log_queries_not_using_indexes = 1
```

### Redis Set Password

Modify the `services/redis/redis.conf` file to find the `requirepass` parameter and change the password:

```ini
requirepass foobared
```

## 🤝 Contributing

We welcome all contributions. You can submit any ideas as Pull requests or as Issues, have a good time!

## 📃 License

The project is released under the MIT License, see the [LICENCE](https://github.com/seatonjiang/monolith/blob/main/LICENSE) file for details.
