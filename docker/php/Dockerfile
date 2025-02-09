FROM php:8.3-apache

WORKDIR /var/www/html

# Install required PHP extensions
RUN apt-get update && apt-get install -y \
        libpng-dev \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        zip \
        curl \
        unzip \
        && docker-php-ext-configure gd \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install mysqli \
        && docker-php-ext-install zip \
        && docker-php-ext-install zip opcache \
        && docker-php-source delete

# Enable Apache mod_rewrite
RUN  a2enmod rewrite


# Install Composer globally
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer


# Start Apache
CMD ["apache2-foreground"]