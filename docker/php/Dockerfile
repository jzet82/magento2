FROM php:8.2-fpm-buster

RUN echo "Acquire::http::Pipeline-Depth 0;" > /etc/apt/apt.conf.d/privet && echo "Acquire::http::No-Cache true;" >> /etc/apt/apt.conf.d/privet && echo "Acquire::BrokenProxy    true;" >> /etc/apt/apt.conf.d/privet

RUN apt-get update
RUN apt-get -qq install -y \
    apt-utils \
    git \
    curl \
    unzip \
    vim \
    nano \
    libzip-dev \
    libmcrypt-dev \
    libicu-dev \
    libxml2-dev libxslt1-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    mariadb-client \
    mailutils \
    golang-go \
    && docker-php-ext-configure hash --with-mhash \
    && docker-php-ext-configure gd --with-jpeg --with-freetype \
    && docker-php-ext-install -j$(nproc) intl xsl gd zip pdo_mysql opcache soap bcmath sockets exif

RUN docker-php-ext-install mysqli

# Configuring php.ini
RUN apt-get install -y vim \
    && cd /usr/local/etc/php \
    && cp php.ini-development php.ini

# Memory Limit, Execution time and opcache settings - php.ini
RUN echo "memory_limit = 4G" > /usr/local/etc/php/conf.d/memory-limit.ini
RUN echo "max_execution_time = 1800" > /usr/local/etc/php/conf.d/max-execution-time.ini
RUN echo "opcache.enable = 1" > /usr/local/etc/php/conf.d/opcache.ini
RUN echo "opcache.save_comments = 1" > /usr/local/etc/php/conf.d/opcache.ini
RUN echo "opcache.memory_consumption=2048" > /usr/local/etc/php/conf.d/opcache.ini
RUN echo "upload_max_filesize = 1024M" > /usr/local/etc/php/conf.d/upload-max-size.ini
RUN echo "realpath_cache_size=10M" > /usr/local/etc/php/conf.d/realpath_cache_size.ini
RUN echo "realpath_cache_ttl=7200" > /usr/local/etc/php/conf.d/realpath_cache_ttl.ini

# Install xdebug extension
 RUN pecl install xdebug && docker-php-ext-enable xdebug \
    && echo "xdebug.mode=develop,debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.start_with_request = yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Install Composer
RUN cd /usr/src && curl -sS http://getcomposer.org/installer | php
RUN cd /usr/src && mv composer.phar /usr/bin/composer
