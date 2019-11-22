FROM php:7.3.6-fpm-alpine3.9

MAINTAINER Marko Vušak <marko.vusak2@gmail.com>

RUN apk update && apk upgrade && \
    apk add --no-cache bash git zlib-dev && \
    apk add --no-cache --virtual .build-deps  libzip-dev && \
    apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS

RUN pecl channel-update pecl.php.net && \
    pecl install xdebug && \
    docker-php-ext-install mbstring mysqli pdo_mysql sysvsem zip calendar && \
    docker-php-ext-enable xdebug

RUN echo "xdebug.remote_enable=1"                                               >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
        echo "xdebug.remote_autostart=0"                                            >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
        echo "xdebug.remote_port=9000"                                              >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
        echo "xdebug.remote_connect_back=0"                                         >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
        echo "xdebug.idekey=PHPSTORM"                                               >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
        echo "xdebug.remote_host=$(/sbin/ip route|awk '/default/ { print $3 }')"    >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

COPY php-fpm.conf /etc/php/7.3/fpm
COPY xdebug.ini /etc/php/7.3/mods-available

RUN addgroup -g 1000 app && \
    adduser -s /bin/sh -DS -u 1000 -G app app

# Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    chmod +x /usr/local/bin/composer

RUN mkdir -m777 -p ~/app/var && \
    mkdir -m777 -p ~/app/vendor

USER app
WORKDIR /app

CMD  ["php-fpm", "-F"]
