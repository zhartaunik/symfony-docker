FROM php:8.1-fpm

# Install symfony required applications
RUN apt-get update && apt-get install -y \
        libzip-dev \
        libpq-dev \
        libicu-dev \
        libonig-dev \
        libpng-dev \
        libjpeg-dev \
        libxslt-dev \
        libfreetype6-dev \
        unzip \
        git \
        sudo \
        iputils-ping \
        vim \
        telnet \
        procps \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) intl pdo_pgsql pdo_mysql xsl gd zip mbstring opcache \
    && pecl install apcu

# Install Xdebug
RUN pecl install xdebug \
  && docker-php-ext-enable xdebug

# Composer installation
COPY --from=composer:2.3 /usr/bin/composer /usr/local/bin/composer

# Install Symfony CLI
RUN curl -sS https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh -o setup.deb.sh \
    && chmod +x setup.deb.sh \
    && ./setup.deb.sh \
    && apt-get install -y symfony-cli \
    && rm setup.deb.sh

# install node.js
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get install -y nodejs

# Copy php.ini base configuration
ADD ./ini/php.ini $PHP_INI_DIR/conf.d/php.ini
RUN chmod -R +rw $PHP_INI_DIR/conf.d

WORKDIR /var/www/html

ADD docker-entrypoint.sh /docker-entrypoint.sh

RUN ["chmod", "+x", "/docker-entrypoint.sh"]
ENTRYPOINT ["/docker-entrypoint.sh"]

ENV USER_ID=1001
ENV GROUP_ID=1001

# Create the symfony user
RUN groupadd -g ${GROUP_ID} symfony \
    && useradd -u ${USER_ID} -ms /bin/bash -g ${GROUP_ID} symfony

USER symfony

CMD ["php-fpm"]
