[![Logo Image](https://cdn.pterodactyl.io/logos/new/pterodactyl_logo.png)](https://pterodactyl.io)

![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/pterodactyl/panel/ci.yaml?label=Tests&style=for-the-badge&branch=1.0-develop)
![Discord](https://img.shields.io/discord/122900397965705216?label=Discord&logo=Discord&logoColor=white&style=for-the-badge)
![GitHub Releases](https://img.shields.io/github/downloads/pterodactyl/panel/latest/total?style=for-the-badge)
![GitHub contributors](https://img.shields.io/github/contributors/pterodactyl/panel?style=for-the-badge)

# Pterodactyl Panel

Pterodactyl® is a free, open-source game server management panel built with PHP, React, and Go. Designed with security
in mind, Pterodactyl runs all game servers in isolated Docker containers while exposing a beautiful and intuitive
UI to end users.

Stop settling for less. Make game servers a first class citizen on your platform.

![Image](https://cdn.pterodactyl.io/site-assets/pterodactyl_v1_demo.gif)

## Documentation

* [Panel Documentation](https://pterodactyl.io/panel/1.0/getting_started.html)
* [Wings Documentation](https://pterodactyl.io/wings/1.0/installing.html)
* [Community Guides](https://pterodactyl.io/community/about.html)
* Or, get additional help [via Discord](https://discord.gg/pterodactyl)

## Sponsors

I would like to extend my sincere thanks to the following sponsors for helping fund Pterodactyl's development.
[Interested in becoming a sponsor?](https://github.com/sponsors/matthewpi)

| Company                                                                           | About                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**Aussie Server Hosts**](https://aussieserverhosts.com/)                         | No frills Australian Owned and operated High Performance Server hosting for some of the most demanding games serving Australia and New Zealand.                                                                                                 |
| [**BisectHosting**](https://www.bisecthosting.com/)                               | BisectHosting provides Minecraft, Valheim and other server hosting services with the highest reliability and lightning fast support since 2012.                                                                                                 |
| [**MineStrator**](https://minestrator.com/)                                       | Looking for the most highend French hosting company for your minecraft server? More than 24,000 members on our discord trust us. Give us a try!                                                                                                 |
| [**HostEZ**](https://hostez.io)                                                   | US & EU Rust & Minecraft Hosting. DDoS Protected bare metal, VPS and colocation with low latency, high uptime and maximum availability. EZ!                                                                                                     |
| [**Blueprint**](https://blueprint.zip/?utm_source=pterodactyl&utm_medium=sponsor) | Create and install Pterodactyl addons and themes with the growing Blueprint framework - the package-manager for Pterodactyl. Use multiple modifications at once without worrying about conflicts and make use of the large extension ecosystem. |
| [**indifferent broccoli**](https://indifferentbroccoli.com/)                      | indifferent broccoli is a game server hosting and rental company. With us, you get top-notch computer power for your gaming sessions. We destroy lag, latency, and complexity--letting you focus on the fun stuff.                              |

### Supported Games

Pterodactyl supports a wide variety of games by utilizing Docker containers to isolate each instance. This gives
you the power to run game servers without bloating machines with a host of additional dependencies.

Some of our core supported games include:

* Minecraft — including Paper, Sponge, Bungeecord, Waterfall, and more
* Rust
* Terraria
* Teamspeak
* Mumble
* Team Fortress 2
* Counter Strike: Global Offensive
* Garry's Mod
* ARK: Survival Evolved

In addition to our standard nest of supported games, our community is constantly pushing the limits of this software
and there are plenty more games available provided by the community. Some of these games include:

* Factorio
* San Andreas: MP
* Pocketmine MP
* Squad
* Xonotic
* Starmade
* Discord ATLBot, and most other Node.js/Python discord bots
* [and many more...](https://github.com/parkervcp/eggs)

## License

Pterodactyl® Copyright © 2015 - 2022 Dane Everitt and contributors.

Code released under the [MIT License](./LICENSE.md).

# Руководство по установке панели Pterodactyl

## Шаг 1: Установка необходимых компонентов

```bash
# Добавление команды "add-apt-repository"
apt -y install software-properties-common curl apt-transport-https ca-certificates gnupg
```

## Шаг 2: Подключение дополнительных репозиториев

### Для PHP (Ubuntu 20.04 и Ubuntu 22.04)
```bash
LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php
```

### Для Redis
```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
```

### Для MariaDB (Ubuntu 20.04)
```bash
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

## Шаг 3: Обновление списка репозиториев
```bash
apt update
```

## Шаг 4: Установка зависимостей

```bash
apt -y install php8.3 php8.3-{common,cli,gd,mysql,mbstring,bcmath,xml,fpm,curl,zip} mariadb-server nginx tar unzip git redis-server

curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

## Шаг 5: Загрузка и настройка Pterodactyl

### Создание каталога для панели
```bash
mkdir -p /var/www/pterodactyl
cd /var/www/pterodactyl
```

### Загрузка и распаковка панели
```bash
curl -Lo panel.tar.gz https://github.com/pius-pp/pterodactyl-panel/releases/latest/download/panel.tar.gz
tar -xzvf panel.tar.gz
chmod -R 755 storage/* bootstrap/cache/
```

## Шаг 6: Настройка базы данных

### Подключение к MariaDB
```bash
mariadb -u root -p
```

### Подключение к MySQL (если используется)
```bash
mysql -u root -p
```

### Создание пользователя и базы данных
```sql
CREATE USER 'pterodactyl'@'127.0.0.1' IDENTIFIED BY 'yourPassword';
CREATE DATABASE panel;
GRANT ALL PRIVILEGES ON panel.* TO 'pterodactyl'@'127.0.0.1' WITH GRANT OPTION;
exit
```

## Шаг 7: Установка панели

### Установка зависимостей с помощью Composer
```bash
cp .env.example .env
COMPOSER_ALLOW_SUPERUSER=1 composer install --no-dev --optimize-autoloader
```

### Генерация ключа приложения (только при первой установке)
```bash
php artisan key:generate --force
```

### Настройка окружения
```bash
php artisan p:environment:setup
php artisan p:environment:database
```

### Настройка почтового сервера
```bash
php artisan p:environment:mail
```

### Выполнение миграций и заполнение базы данных
```bash
php artisan migrate --seed --force
```

### Создание пользователя
```bash
php artisan p:user:make
```

## Шаг 8: Настройка прав доступа

### Для NGINX, Apache или Caddy (не для RHEL / Rocky Linux / AlmaLinux)
```bash
chown -R www-data:www-data /var/www/pterodactyl/*
```

### Для NGINX на RHEL / Rocky Linux / AlmaLinux
```bash
chown -R nginx:nginx /var/www/pterodactyl/*
```

### Для Apache на RHEL / Rocky Linux / AlmaLinux
```bash
chown -R apache:apache /var/www/pterodactyl/*
```

## Шаг 9: Настройка cron-задачи

Добавьте следующую строку в файл crontab:
```bash
* * * * * php /var/www/pterodactyl/artisan schedule:run >> /dev/null 2>&1
```

## Шаг 10: Настройка службы Pterodactyl Queue Worker

Создайте файл службы:
```bash
sudo nano /etc/systemd/system/pteroq.service
```

Вставьте следующий текст:
```ini
[Unit]
Description=Pterodactyl Queue Worker
After=redis-server.service

[Service]
User=www-data
Group=www-data
Restart=always
ExecStart=/usr/bin/php /var/www/pterodactyl/artisan queue:work --queue=high,standard,low --sleep=3 --tries=3
StartLimitInterval=180
StartLimitBurst=30
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

### Включение службы
```bash
sudo systemctl enable --now redis-server
sudo systemctl enable --now pteroq.service
```
