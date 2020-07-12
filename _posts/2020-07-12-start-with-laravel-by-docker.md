---
layout: post
title: ติดตั้ง Laravel ด้วย Docker สำหรับ Dev
categories: laravel
---
วิธีที่ผมใช้นี้มีแค่ Docker ก็เพียงพอแล้ว สำหรับการเริ่มต้นโปรเจค

1. ติดตั้ง Laravel ด้วย `$ docker run --rm --interactive --tty \\
--volume $PWD:/app \\
composer create-project --prefer-dist laravel/laravel laravel-blog`
2. เข้า https://phpdocker.io/generator เพื่อสร้างไฟล์ `docker-compose.yml` ตั้งชื่อโปรเจค `laravel-blog`
    
    ```
    Global configuration
        — Project name : laravel-blog
        — Base port: 8080
    PHP configuration
        — PHP Version: 7.2.x
        — Extensions (PHP 7.2.x): MySQL, Bcmath, GD
    MySQL
        — Enable MySQL
        — Version: 5.7
            — Root Password: toor
            — Database Name: admin_laravel_blog
            — Database Username: admin_laravel_blog
            — Database Password: 123456
    ```

    แล้วกด “Generate project archive” จะได้รับ “laravel-blog.zip”
3. แตกไฟล์ laravel-blog.zip `unzip laravel-blog.zip && mv laravel-blog/* /<project_path>/aravel-blog`
4. แก้ไข้ .env
    ```env
    DB_CONNECTION=mysql
    DB_HOST=msql
    DB_PORT=3306
    DB_DATABASE=admin_laravel_blog
    DB_USERNAME=admin_laravel_blog
    DB_PASSWORD=1234
    ```
5. เปิด server project ด้วยคำสั่ง `$ docker-compose up`
6. รันคำสั่ง `$ docker-compose exec php-fpm php artisan migate`
7. เปิด Browser “[http:/127.0.0.1:8080](http:/127.0.0.1:8080)”