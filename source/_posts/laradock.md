---
title: install laradock on ubuntu by snap
date: 2020-12-17 16:03
tags: -docker laravel
---

## how to install laradock

(Laradock Offical website)[https://laradock.io/] 

1. snap install docker
2. git clone https://github.com/laradock/laradock.git
3. cd laradock && cp env-example .env
4. vi .env 
 Find "APP_CODE_PATH_HOST" Set to laravel project location.
5. docker-compose up -d mysql redis phpmyadmin redis workspace
6. docker-compose exec mysql bash
7. mysql -uroot -proot 
8. CREATE 'user'@'localhost' IDENTIFIED BY 'PASSWORD'
9. CREATE DATABASE laravel;
10. Start Developing~~  e.g. php artisan key:generate && php artisan migrate && php artisan db:seed

Now you can see laravel offical website pages by visiting http://localhost:80/

