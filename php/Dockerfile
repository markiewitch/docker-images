ARG PHP_BASE_IMAGE_TAG=8.0-fpm

FROM php:${PHP_BASE_IMAGE_TAG} AS sylius_php
RUN apt update
RUN apt install -y zlib1g-dev
RUN apt install -y libpng-dev
RUN apt install -y libjpeg-dev
RUN apt install -y libfreetype6-dev
RUN apt install -y libicu-dev
RUN apt install -y zip
RUN apt install -y libmemcached-dev
RUN docker-php-ext-install gd
RUN docker-php-ext-install exif
RUN docker-php-ext-install fileinfo
RUN docker-php-ext-install intl
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install opcache
RUN pecl install apcu
RUN docker-php-ext-enable apcu --ini-name 10-docker-php-ext-apcu.ini
RUN pecl install memcached
RUN docker-php-ext-enable memcached
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN apt install -y libmagickwand-dev --no-install-recommends
RUN pecl install imagick
RUN docker-php-ext-enable imagick
ENV PHP_DATE_TIMEZONE="Europe/Warsaw"
COPY --from=composer /usr/bin/composer /usr/bin/composer
WORKDIR /symfony
RUN apt install -y wget
RUN wget https://get.symfony.com/cli/installer
RUN chmod +x installer
RUN bash installer
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony
WORKDIR /
