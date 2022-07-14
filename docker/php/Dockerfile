FROM php:8.1.8-apache

WORKDIR /var/www/

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Setup context
ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS="0"

RUN apt-get update \
    &&  apt-get install -y --no-install-recommends \
    locales apt-utils git libicu-dev g++ libpq-dev libpng-dev libxml2-dev libzip-dev libonig-dev libxslt-dev unzip

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen  \
    &&  echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen \
    &&  locale-gen

RUN  curl -sS https://getcomposer.org/installer | php -- \
    &&  mv composer.phar /usr/local/bin/composer

RUN curl -sS https://get.symfony.com/cli/installer | bash \
    &&  mv /root/.symfony/bin/symfony /usr/local/bin \
    \
    &&  docker-php-ext-configure \
    intl \
    &&  docker-php-ext-install \
    pdo pdo_pgsql intl zip calendar mbstring \
    &&  pecl install apcu && docker-php-ext-enable apcu opcache \
    && pecl install xdebug && docker-php-ext-enable xdebug

RUN useradd -d /home/dev -m -s /bin/bash dev
USER dev