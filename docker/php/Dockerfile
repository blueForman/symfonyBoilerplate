FROM php:8.1.3-fpm

MAINTAINER Marko Vušak <marko.vusak2@gmail.com>

RUN addgroup --group app && \
    adduser --shell /bin/bash app -u 1000 --ingroup app

RUN apt-get update && \
    apt-get install -y bash git zip

# Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    chmod +x /usr/local/bin/composer

RUN mkdir -m777 -p ~/app/var && \
    mkdir -m777 -p ~/app/vendor

USER app
WORKDIR /app

CMD  ["php-fpm", "-F"]
