FROM php:8.3-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy application code
COPY . /var/www/html
WORKDIR /var/www/html

# Install Laravel dependencies
RUN composer install --no-scripts --no-autoloader

# Set up the environment
ENV APP_ENV=production
RUN key=`php artisan key:generate --show --no-interaction` && \
    sed -i "s/APP_KEY=.*/APP_KEY=$key/" .env
RUN chown -R www-data:www-data /var/www/html

# Expose the application port
EXPOSE 8000

# Start the application
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]

