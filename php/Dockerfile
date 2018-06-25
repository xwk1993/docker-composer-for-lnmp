FROM php:7.1.18-fpm-alpine

ENV BUILD_DEPS='git bash curl g++ make autoconf re2c libtool pcre-dev libmcrypt-dev libxml2-dev freetype-dev libpng-dev libjpeg-turbo-dev' \
    PHP_EXTENSION_DEPS='libmcrypt freetype libpng libjpeg-turbo'

COPY ./composer.phar /usr/local/bin/composer

RUN apk update \
    && apk add --no-cache $BUILD_DEPS $PHP_EXTENSION_DEPS \
    && docker-php-source extract \
    && pecl -vvv install apcu-5.1.11 && docker-php-ext-enable apcu \
    && pecl -vvv install swoole-2.2.0 && docker-php-ext-enable swoole \
    && pecl -vvv install redis-4.0.2 && docker-php-ext-enable redis \
    && docker-php-source delete \
    && docker-php-ext-configure gd --with-gd --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-freetype-dir=/usr/include/ \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" gd \
    && docker-php-ext-configure mcrypt --with-mcrypt \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" mcrypt \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" pdo_mysql mysqli bcmath \
    && cd /tmp \
    && git clone --depth=1 "git://github.com/phalcon/cphalcon.git" \
    && cd cphalcon/build \
    && ./install \
    && docker-php-ext-enable phalcon \
    && chmod +x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com \
    && apk del --no-cache $BUILD_DEPS \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/*