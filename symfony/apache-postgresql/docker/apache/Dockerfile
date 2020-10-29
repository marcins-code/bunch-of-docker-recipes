FROM php:7.3-apache
ARG TIMEZONE

RUN apt-get update && apt-get install -y \
    gnupg \
    g++ \
    procps \
    openssl \
    git \
    unzip \
    zlib1g-dev \
    libzip-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libicu-dev  \
    libonig-dev \
    libxslt1-dev \
    acl \
    vim \
    nano \
    && echo 'alias sc="php bin/console"' >> ~/aliases.txt



RUN docker-php-ext-install \
    pdo pdo_mysql zip xsl gd intl opcache exif mbstring

RUN apt-get update && apt-get install -y libpq-dev && docker-php-ext-install  pdo_pgsql

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
    && "date"


RUN pecl install xdebug
RUN echo "xdebug.remote_enable=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.coverage_enable=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.idekey=\"PHPSTORM\"\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.remote_port=9001\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN docker-php-ext-enable xdebug

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

