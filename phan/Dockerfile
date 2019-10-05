FROM php:7.3.10-cli-alpine3.10

WORKDIR /analyze

ENV PHPIZE_DEPS \
        autoconf \
        dpkg-dev dpkg \
        file \
        g++ \
        gcc \
        libc-dev \
        make \
        pkgconf \
        re2c

RUN apk update && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS; \
        pecl install ast-1.0.1; \
        echo 'extension=ast.so' > /usr/local/etc/php/php.ini; \
        apk del --no-network .build-deps

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"; \
        php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"; \
        php composer-setup.php; \
        php -r "unlink('composer-setup.php');"; \
        php composer.phar require "phan/phan:2.x"

CMD [ "./vendor/bin/phan"]