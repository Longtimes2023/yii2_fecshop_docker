# Use PHP 7.4 FPM image based on Debian 12 Bookworm
FROM php:7.4-fpm

# Install dependencies and required libraries
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libicu-dev \
        libxml2-dev \
        libssl-dev \
        git \
        wget \
        ssh \
        libpcre3-dev \
        pkg-config \
        libonig-dev \
        libzip-dev \
        librabbitmq-dev \
        --no-install-recommends

# Install PHP extensions
RUN docker-php-ext-install bcmath intl mbstring mysqli pdo_mysql exif zip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

# Install additional PHP extensions via PECL
RUN pecl install amqp \
    && pecl install oauth \
    && docker-php-ext-enable amqp oauth

# Clean up unnecessary packages and files to reduce image size
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/*
    # Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer

# Install the Composer Asset Plugin globally
#RUN composer global require "fxp/composer-asset-plugin:^1.4.6"

# Update www-data user to match your host UID (if needed)
RUN usermod -u 1000 www-data

# Expose port 9000 for PHP-FPM
EXPOSE 9000

# Command to start PHP-FPM
CMD ["php-fpm"]
