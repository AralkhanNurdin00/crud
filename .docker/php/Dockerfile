FROM php:7.4.32-fpm-alpine3.16

WORKDIR /var/www/crud

ENV TZ=Asia/Almaty

RUN set -xe; apk add --no-cache --virtual .temp $PHPIZE_DEPS git zip tzdata shadow; \
    apk add --no-cache icu-dev icu-data-full curl-dev libpq-dev freetype-dev libjpeg-turbo-dev libxml2-dev \
    libzip-dev libpng-dev libmcrypt-dev gettext-dev imap-dev libxslt-dev libffi-dev; \
    docker-php-ext-configure gd --with-freetype --with-jpeg; docker-php-ext-configure zip; \
    docker-php-ext-install -j$(nproc) bcmath bz2 calendar dba exif intl ffi fileinfo gd gettext imap \
    opcache pcntl pdo_pgsql pgsql shmop soap sockets sysvmsg sysvsem sysvshm xmlrpc xsl zip; \
    pecl install -o -f mcrypt redis xdebug-3.1.6; docker-php-ext-enable mcrypt redis xdebug; \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer; \
    groupmod -o -g 1000 www-data; usermod -o -u 1000 -g www-data www-data; \
    chown -R www-data:www-data /var/www/crud; docker-php-source delete; apk del .temp; \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /usr/share/doc/* /usr/share/man/* /var/www/html;

COPY php.ini /usr/local/etc/php/conf.d/custom.ini

USER www-data
