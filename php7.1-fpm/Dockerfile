# Created 27.04.2017
FROM php:7.1.4-fpm

MAINTAINER Vitalii Sydorenko <vetal.sydo@gmail.com>

RUN apt-get update && apt-get install -y \
    git \
    unzip \
    nano \
    curl \
    tree

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime
RUN "date"


# Install GD
RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng12-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

# Install MCrypt
RUN apt-get update \
    && apt-get install -y libmcrypt-dev \
    && docker-php-ext-install mcrypt

# Install Intl
RUN apt-get update \
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl

# Install Mysql
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Install soap
RUN apt-get update \
    && apt-get install -y libxml2-dev \
    && docker-php-ext-install soap

# Install PHP zip extension
RUN docker-php-ext-install zip

## Install xdebug
#RUN pecl install xdebug
#RUN docker-php-ext-enable xdebug
#RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
##RUN echo "xdebug.remote_host=172.17.0.1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

ENV XDEBUG_ENABLE 0
RUN pecl install xdebug \
    && rm -rf /tmp/pear
COPY ./xdebug.ini.disabled /usr/local/etc/php/conf.d/

RUN echo 'alias ll="ls -l"' >> ~/.bashrc
RUN echo 'alias sf="php app/console"' >> ~/.bashrc
RUN echo 'alias sf3="php bin/console"' >> ~/.bashrc

WORKDIR /var/www/symfony

# Start!
COPY ./start /usr/local/bin/
CMD ["start"]
