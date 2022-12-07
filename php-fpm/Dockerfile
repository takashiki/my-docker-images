FROM php:8.1-fpm

LABEL maintainer="takashiki <857995137@qq.com>"

RUN apt-get update -yqq && \
    apt-get install -y apt-utils && \
    pecl channel-update pecl.php.net

# Install base vendor extensions
RUN apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    zlib1g-dev \
    libicu-dev \
    g++ \
    libmagickwand-dev \
    imagemagick \
    libgmp-dev \
    zip unzip

# Install base php extensions
RUN docker-php-ext-install pcntl && \
    docker-php-ext-install bcmath && \
    docker-php-ext-install exif && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install tokenizer && \
    docker-php-ext-install gmp && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install pdo_pgsql && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl && \
    docker-php-ext-configure gd \
            --prefix=/usr \
            --with-jpeg \
            --with-freetype; \
    docker-php-ext-install gd

RUN pecl install -o -f redis && \
    docker-php-ext-enable redis && \
    pecl install swoole && \
    docker-php-ext-enable swoole

RUN curl -L https://github.com/Imagick/imagick/archive/master.zip -o /tmp/imagick.zip && \
    unzip -q /tmp/imagick.zip -d /tmp && \
    cd /tmp/imagick-master && \
    phpize && ./configure && \
    make && make install && \
    docker-php-ext-enable imagick

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

WORKDIR /var/www
