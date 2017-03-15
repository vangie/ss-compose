FROM php:7.1.0-fpm-alpine
MAINTAINER Vangie Du <duwan@live.com>

RUN set -ex \
    && if [ $(wget -qO- ipinfo.io/country) == CN ]; then echo "http://mirrors.aliyun.com/alpine/latest-stable/main/" > /etc/apk/repositories ;fi \
    && apk update && apk upgrade \
    && apk add --no-cache freetype-dev libpng-dev libjpeg-turbo-dev git openssh \
    \
    && apk add --update libintl \
    && apk add --virtual build_deps gettext \
    && apk add --no-cache --virtual .gettext gettext \
    && cp /usr/bin/envsubst /usr/local/bin/envsubst \
    && apk del build_deps \
    \
    && docker-php-ext-configure gd \
          --with-gd \
          --with-freetype-dir=/usr/include/ \
          --with-png-dir=/usr/include/ \
          --with-jpeg-dir=/usr/include/ \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} gd pdo_mysql \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/bin --filename=composer

RUN mkdir -p /var/www; cd /var/www  \
    && curl -sS https://codeload.github.com/orvice/ss-panel/tar.gz/master | tar zxvf - \
    && mv ss-panel-master/* html; rm -rf ss-panel-master \
    && cd /var/www/html/; composer install \
    && chmod -R 777 /var/www/html/storage

VOLUME /var/www/html
