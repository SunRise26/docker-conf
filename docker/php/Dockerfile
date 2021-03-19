FROM php:8.0-fpm-alpine

RUN apk update && apk add -u \
    composer \
    icu-dev \
    zlib-dev \
    libzip-dev \
    zip \
    freetype-dev \
    libpng-dev \
    jpeg-dev \
    libjpeg-turbo-dev

RUN docker-php-ext-configure gd \
    --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/

# install php extentions
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-3.0.3 \
    && docker-php-ext-install \
        bcmath \
        pdo_mysql \
        intl \
        opcache \
        gd \
        zip \
        exif \
    && docker-php-ext-enable xdebug

RUN apk add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data

RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

ENV COMPOSER_MEMORY_LIMIT=-1

WORKDIR /var/www/amigo

CMD ["php-fpm"]

RUN echo "xdebug.client_port = 9000" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.client_host = \"172.17.0.1\""  >> /usr/local/etc/php/php.ini \
    && echo "xdebug.mode = debug"  >> /usr/local/etc/php/php.ini \
    && echo "xdebug.start_with_request = yes"  >> /usr/local/etc/php/php.ini \
    && echo "xdebug.idekey = VSCODE"  >> /usr/local/etc/php/php.ini \
    && echo "post_max_size = 100M"  >> /usr/local/etc/php/php.ini \
    && echo "upload_max_filesize = 100M"  >> /usr/local/etc/php/php.ini
