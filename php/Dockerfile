FROM php:7.4-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid

COPY wait-for-it.sh /usr/bin/wait-for-it
RUN chmod +x /usr/bin/wait-for-it

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql

RUN pecl install xdebug-2.9.6 \
    && docker-php-ext-enable xdebug

# Get latest Composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Symfony commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

COPY xdebug.ini  /etc/php7/conf.d/

WORKDIR /var/www

CMD composer install ; wait-for-it database:3306 -- bin/console doctrine:migrations:migrate ;  php-fpm
EXPOSE 9000