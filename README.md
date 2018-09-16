## laravel-laradock

### version info
- Mac OS 10.11.6（El Capitan）
- Docker Toolbox
  - Docker version 18.03.0-ce
  - Oracle VM VirtualBox Manager 5.2.12
- Laradock
  - php-fpm 7.2.4
  - nginx version: nginx/1.15.0
  - mysql  Ver 8.0.11 for Linux on x86_64 (MySQL Community Server - GPL)
- [Laravel 5.6.26](https://laravel.com/)

#### other info
- Qiita:[LaradockでPHPフレームワークとCMSの開発環境を構築する \- Qiita](https://qiita.com/2no553/items/c1da7bb6dfed68c5af1b)
- blog:[DockerとLaradockでPHPフレームワークとCMSの開発環境を構築する – Ninolog](http://ninolog.com/docker-build-php-from-laradock/)

### setting laradock
```
$ git clone https://github.com/2no553/laravel_laradock.git
$ cd laravel_laradock

$ git clone https://github.com/Laradock/laradock.git
$ cd laradock
$ cp env-example .env
```

#### setting mysql latest
```
$ vi docker-compose.yml
  ports:
    - "${MYSQL_PORT}:3306"
  networks:
    - backend
+ user: "1000:50"

$ vi mysql/my.cnf
+ innodb_use_native_aio=0
+ default_authentication_plugin=mysql_native_password
```

### start container
```
$ docker-compose up -d nginx mysql
$ docker-compose ps
```

### install laravel
```
$ docker-compose exec workspace bash
# composer create-project laravel/laravel app-name --prefer-dist
$ exit

$ vi nginx/sites/default.conf
- root /var/www/public;
+ root /var/www/app-name/public;

$ docker-compose restart nginx
```

### view laravel start page
```
http://192.168.99.100/
```

### test migration
```
$ docker-compose exec workspace bash
# cd app-name
# vi .env
- DB_HOST=127.0.0.1
- DB_DATABASE=homestead
- DB_USERNAME=homestead
+ DB_HOST=mysql
+ DB_DATABASE=default
+ DB_USERNAME=default

# php artisan migrate:status
# php artisan migrate
```
