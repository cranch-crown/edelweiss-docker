FROM php:7.3.5-fpm
LABEL maintainer="ymzlgcranch@gmail.com"

COPY php.ini /usr/local/etc/php/

#7.3fpmにはbcmathが含まれていないためPHPビルド
RUN docker-php-ext-install bcmath

#zip拡張
RUN apt-get update && apt-get install -y \
        zlib1g-dev \
        libzip-dev \
    && docker-php-ext-install zip
#OSにzipコマンドのインストール
RUN apt-get install zip unzip

#composerインストール
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && mv /var/www/html/composer /usr/local/bin/composer \
    && chmod a+x /usr/local/bin/composer 

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

#Laravelインストール
RUN composer global require "laravel/installer"

RUN /usr/local/etc/php-fpm.d/www.conf