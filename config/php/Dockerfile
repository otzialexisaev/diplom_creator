FROM php:7.4-fpm

RUN ln -snf /usr/share/zoneinfo/Europe/Moscow /etc/localtime && echo Europe/Moscow > /etc/timezone

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    unzip \
    mc \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libxrender1 \
    libfontconfig \
    libzip-dev \
    libicu-dev \
    libonig-dev \
    libwebp-dev \
    libpq-dev \
    sendmail \
    libxml2-dev \
    && docker-php-ext-install -j$(nproc) opcache iconv intl mbstring pdo_mysql zip soap pcntl exif \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) gd

RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

RUN pecl install xdebug && docker-php-ext-enable xdebug \
    && echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.discover_client_host=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN mkdir -p /var/www

RUN adduser wwwdata
RUN usermod -d /var/www wwwdata

USER wwwdata

# Рабочая директория
WORKDIR /var/www

ENTRYPOINT ["php-fpm"]
