FROM php:7.0-fpm

COPY --from=composer/composer:2.2 /usr/bin/composer /usr/bin/composer

LABEL org.opencontainers.image.authors="Arthur Aivazov <artur.aivazoff@gmail.com>"

RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list \
    && apt-get update \ 
    && apt-get install -y \
        apt-utils \
        libjpeg62-turbo-dev libfreetype6-dev libpng-dev \
        libzip-dev \
        libicu-dev \
        zlib1g-dev g++

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-configure zip --with-libzip
RUN docker-php-ext-configure intl

RUN docker-php-ext-install -j$(nproc) gd zip pcntl mysqli pdo_mysql intl

RUN pecl install xdebug-2.9.0
RUN docker-php-ext-enable xdebug

RUN usermod -u 1000 www-data
RUN chown -R www-data:www-data /var/www

WORKDIR /var/www
CMD ["php-fpm"]
