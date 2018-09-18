FROM php:7.1.18-fpm-alpine

ENV BUILD_DEPS='bash curl g++ make autoconf re2c libtool pcre-dev libmcrypt-dev libxml2-dev freetype-dev libpng-dev libjpeg-turbo-dev libmemcached-dev zlib-dev' \
    PHP_EXTENSION_DEPS='git libmcrypt freetype libpng libjpeg-turbo libmemcached zlib'

COPY ./composer.phar /usr/local/bin/composer
COPY ./ext/* /tmp/

RUN echo -e "https://mirrors.aliyun.com/alpine/v3.7/main\nhttps://mirrors.aliyun.com/alpine/v3.7/community" > /etc/apk/repositories \
    && apk update \
    && apk add --no-cache $BUILD_DEPS $PHP_EXTENSION_DEPS \
    && chmod +x /usr/local/bin/composer && composer config -g repo.packagist composer https://packagist.phpcomposer.com && composer config -g secure-http false \
    && docker-php-ext-configure gd --with-gd --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-freetype-dir=/usr/include/ \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" gd \
    && docker-php-ext-configure mcrypt --with-mcrypt \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" mcrypt \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" pdo_mysql mysqli bcmath \
    # && docker-php-source extract \
    # && pecl -vvv install apcu-5.1.11 && docker-php-ext-enable apcu \
    # && pecl -vvv install swoole-4.0.1 && docker-php-ext-enable swoole \
    # && pecl -vvv install redis-4.0.2 && docker-php-ext-enable redis \
    # && pecl -vvv install memcached-3.0.4 && docker-php-ext-enable memcached \
    # && docker-php-source delete \
    && cd /tmp && tar -xvf apcu-5.1.11.tgz && cd apcu-5.1.11 && phpize && ./configure && make -j"$(getconf _NPROCESSORS_ONLN)" && make install && docker-php-ext-enable apcu\
    && cd /tmp && tar -xvf swoole-4.0.1.tgz && cd swoole-4.0.1 && phpize && ./configure && make -j"$(getconf _NPROCESSORS_ONLN)" && make install && docker-php-ext-enable swoole\
    && cd /tmp && tar -xvf redis-4.0.2.tgz && cd redis-4.0.2 && phpize && ./configure && make -j"$(getconf _NPROCESSORS_ONLN)" && make install && docker-php-ext-enable redis\
    && cd /tmp && tar -xvf memcached-3.0.4.tgz && cd memcached-3.0.4 && phpize && ./configure && make -j"$(getconf _NPROCESSORS_ONLN)" && make install && docker-php-ext-enable memcached\
    && cd /tmp && git clone --depth=1 "git://github.com/phalcon/cphalcon.git" && cd cphalcon/build && ./install && docker-php-ext-enable phalcon \
    && cd /tmp && git clone https://github.com/allegro/php-protobuf.git && cd php-protobuf && phpize && ./configure && make -j"$(getconf _NPROCESSORS_ONLN)" && make install && docker-php-ext-enable protobuf \
    && cd /tmp && git clone https://github.com/lionsoul2014/ip2region.git && cp /tmp/ip2region/binding/c/* /tmp/ip2region/binding/php_extension/php7/ip2region/lib/ && cd ip2region/binding/php_extension/php7/ip2region && phpize && ./configure && make -j"$(getconf _NPROCESSORS_ONLN)" && make install && docker-php-ext-enable ip2region \
    && echo "ip2region.db_file=/hosts/adn-api.pingcoo.com/vendor/pingcoovendor/ip2Region/src/data/ip2region.db" >> /usr/local/etc/php/conf.d/docker-php-ext-ip2region.ini \
    && apk del --no-cache $BUILD_DEPS \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/*
