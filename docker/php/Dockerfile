FROM php:8.1-fpm

# Install symfony required applications
RUN apt-get update && apt-get install -y \
        libzip-dev \
        libpq-dev \
        libicu-dev \
        libonig-dev \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo_mysql zip intl mbstring opcache \
    && pecl install apcu

# Install Xdebug
ARG XDEBUG_VERSION=3.1.2
RUN pecl install xdebug-$XDEBUG_VERSION \
  && docker-php-ext-enable xdebug

# Set Xdebug options
ENV XDEBUG_MODE=${XDEBUG_MODE} \
    XDEBUG_CLIENT_HOST=${XDEBUG_CLIENT_HOST} \
    XDEBUG_CLIENT_PORT=${XDEBUG_CLIENT_PORT} \
    XDEBUG_START_WITH_REQUEST=${XDEBUG_START_WITH_REQUEST}

# Copy php.ini base configuration
COPY docker/php/ini/php.ini /usr/local/etc/php/php.ini

# Composer installation
COPY --from=composer:2.3 /usr/bin/composer /usr/local/bin/composer

ENV USER_ID=1001
ENV GROUP_ID=1001

# Create the symfony user
RUN groupadd -g ${GROUP_ID} symfony \
    && useradd -u ${USER_ID} -ms /bin/bash -g ${GROUP_ID} symfony

USER symfony

WORKDIR /var/www/html

CMD ["php-fpm"]
