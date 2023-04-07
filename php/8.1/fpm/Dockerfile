FROM php:8.1-fpm

COPY --from=composer/composer /usr/bin/composer /usr/bin/composer

MAINTAINER Arthur Aivazov <artur.aivazoff@gmail.com>

RUN apt-get update
RUN apt-get install -y \
    apt-utils \
    libjpeg62-turbo-dev libfreetype6-dev libpng-dev \
    libzip-dev \
    libicu-dev \
	libc-client-dev \
	libkrb5-dev \
	libtidy-dev \
	libcurl3-dev

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-configure zip
RUN docker-php-ext-configure intl
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl 

RUN docker-php-ext-install -j$(nproc) gd zip pcntl mysqli pdo_mysql intl imap tidy curl

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug tidy curl

RUN usermod -u 1000 www-data
RUN chown -R www-data:www-data /var/www

WORKDIR /var/www
CMD ["php-fpm"]
