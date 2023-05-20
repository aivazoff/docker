FROM php:5.6-fpm

LABEL org.opencontainers.image.authors="Arthur Aivazov <artur.aivazoff@gmail.com>"

RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y \
    apt-utils \
    libjpeg62-turbo-dev libfreetype6-dev libpng-dev \
    libzip-dev

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-configure zip --with-libzip
RUN docker-php-ext-install -j$(nproc) gd zip pcntl mysql mysqli pdo_mysql

RUN pecl install xdebug-2.5.5
RUN docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer \
    | php -- --install-dir=/usr/local/bin --filename=composer

# memcached
RUN apt-get update && \
    apt-get install -y libz-dev libmemcached-dev && \
    apt-get install -y memcached libmemcached-tools && \
    pecl install memcached-2.2.0 && \
    docker-php-ext-enable memcached

# memcache
RUN apt-get update && \
    apt-get install -y --no-install-recommends zlib1g-dev && \
    rm -rf /var/lib/apt/lists/* && \
    pecl install memcache-3.0.8 && \
    docker-php-ext-enable memcache

RUN docker-php-ext-install calendar
RUN docker-php-ext-install exif

RUN mkdir -p /var/lib/php/sessions
RUN chmod 733 /var/lib/php/sessions

RUN usermod -u 1000 www-data
RUN chown -R www-data:www-data /var/www

WORKDIR /var/www
CMD ["php-fpm"]
