FROM php:8.1-cli

COPY --from=composer/composer /usr/bin/composer /usr/bin/composer

MAINTAINER Arthur Aivazov <artur.aivazoff@gmail.com>

RUN apt-get update
RUN apt-get install -y \
    apt-utils \
    libjpeg62-turbo-dev libfreetype6-dev libpng-dev \
    libzip-dev

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-configure zip
RUN docker-php-ext-install -j$(nproc) gd zip pcntl mysqli pdo_mysql

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

WORKDIR /var/www

