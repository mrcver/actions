ARG PHP_VERSION
# base image
FROM php:${PHP_VERSION}-fpm-alpine

ENV COMPOSER_HOME=/tmp/composer

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

#install exts
RUN chmod +x /usr/local/bin/install-php-extensions && \
    # php extensions install
    install-php-extensions pdo_mysql mysqli mbstring gd curl opcache && \
    # composer
    install-php-extensions @composer

# set timezone
ARG TZ
RUN apk --no-cache add tzdata \
    && cp "/usr/share/zoneinfo/$TZ" /etc/localtime \
    && echo "$TZ" > /etc/timezone

# php image's www-data user uid & gid are 82, change them to 1000 (primary user)
RUN apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data


WORKDIR /var/html/www