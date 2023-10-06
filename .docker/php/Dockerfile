FROM php:8.2-fpm-alpine

ENV TZ=Europe/Prague

WORKDIR /srv

# Install dependencies
RUN set -eux && \
    apk update && \
    apk add --no-cache \
        unzip && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# https://github.com/mlocati/docker-php-extension-installer
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions \
    intl \
    zip \
    pdo \
    pdo_mysql \
    xdebug

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

COPY .docker/php/conf.d/* /usr/local/etc/php/conf.d/

EXPOSE 9000

CMD ["php-fpm"]
