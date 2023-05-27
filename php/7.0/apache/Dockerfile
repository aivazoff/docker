FROM php:7.0-apache

COPY --from=composer:2.1 /usr/bin/composer /usr/bin/composer

LABEL org.opencontainers.image.authors="Arthur Aivazov <artur.aivazoff@gmail.com>"

RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list \
    && apt-get update \ 
    && apt-get install -y \
        apt-utils \
        libjpeg62-turbo-dev libfreetype6-dev libpng-dev \
        libzip-dev unzip \
        libicu-dev \
        zlib1g-dev g++

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-configure zip --with-libzip
RUN docker-php-ext-configure intl

RUN docker-php-ext-install -j$(nproc) gd zip pcntl mysqli pdo_mysql intl

RUN pecl install xdebug-2.9.0
RUN docker-php-ext-enable xdebug

# memcached
RUN apt update && \
    apt install -y libz-dev libmemcached-dev && \
    apt install -y memcached libmemcached-tools && \
    pecl install memcached && \
    docker-php-ext-enable memcached

# memcache
RUN apt-get update && \
    apt-get install -y --no-install-recommends zlib1g-dev && \
    rm -rf /var/lib/apt/lists/* && \
    pecl install memcache-4.0.5.2 && \
    docker-php-ext-enable memcache

RUN docker-php-ext-install calendar exif

RUN usermod -u 1000 www-data
RUN chown -R www-data:www-data /var/www

RUN mkdir -p /var/lib/php/sessions
RUN chmod 733 /var/lib/php/sessions


RUN a2enmod rewrite

WORKDIR /var/www
