---
title: PHP xdebug in docker container
tags: [docker, xdebug]
categories: [development]

---

When you are using Docker as a development environment, you face the situation to debug your php code with xdebug soon
or later.
All yo need to do, is to change your `Dockerfile` of your php container like this:
~~~dockerfile
FROM php:7.0-fpm

RUN docker-php-ext-install pdo_mysql \
    && docker-php-ext-install json

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN sed -i '1 a xdebug.remote_autostart=true' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_mode=req' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_handler=dbgp' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_connect_back=1 ' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_port=9000' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_host=192.168.0.196' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_enable=1' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
~~~

If your container was already running, you have to stop it and start it again
~~~bash
$ for id in $(sudo docker ps | grep -v CONTAINER | awk '{print $1}') ; do sudo docker stop $id ; done
$ sudo docker-compose up -d
~~~

And that's it.
