FROM composer:2.0.2 as vendor

COPY composer.* ./
COPY database/ database/

RUN composer install \
    --no-interaction \
    --no-plugins \
    --no-scripts \
    --no-dev \
    --prefer-dist
    
FROM php:7.4-fpm

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libonig-dev \
    libzip-dev \
    libmagickwand-dev --no-install-recommends

# PHP Extensions
RUN docker-php-ext-install mbstring zip exif pcntl bcmath

# Clear apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY . /var/www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

COPY --from=vendor app/vendor/ ./vendor/
COPY entrypoint.sh ./
RUN chmod +x /var/www/entrypoint.sh

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
# EXPOSE 9000

ENTRYPOINT [ "/var/www/entrypoint.sh" ]