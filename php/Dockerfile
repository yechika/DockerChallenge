# Builder stage
FROM php:8.2-cli-alpine as builder

WORKDIR /var/www/html

# Install PHP extensions and build dependencies
RUN apk add --no-cache \
    zip \
    libzip-dev \
    libpng-dev \
    libxml2-dev \
    oniguruma-dev \
    postgresql-dev && \
    docker-php-ext-configure zip && \
    docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    zip \
    exif \
    pcntl \
    gd \
    bcmath

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy composer files first for dependency caching
COPY composer.* ./

# Install dependencies (no dev, optimized for production)
RUN composer install --no-dev --no-interaction --no-progress --no-scripts --no-suggest --optimize-autoloader

# Copy application files
COPY . .

# Production stage
FROM php:8.2-fpm-alpine

# Install runtime dependencies
RUN apk add --no-cache \
    libzip \
    libpng \
    libxml2 \
    oniguruma \
    postgresql-libs \
    nginx \
    supervisor

# Copy PHP extensions from builder
COPY --from=builder /usr/local/lib/php/extensions/ /usr/local/lib/php/extensions/

# Copy configuration files
COPY docker/nginx.conf /etc/nginx/nginx.conf
COPY docker/fpm-pool.conf /usr/local/etc/php-fpm.d/www.conf
COPY docker/php.ini /usr/local/etc/php/conf.d/custom.ini
COPY docker/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Copy application from builder
COPY --from=builder --chown=www-data:www-data /var/www/html /var/www/html

# Optimize PHP configuration
RUN echo "opcache.enable=1" >> /usr/local/etc/php/conf.d/opcache.ini && \
    echo "opcache.memory_consumption=128" >> /usr/local/etc/php/conf.d/opcache.ini

WORKDIR /var/www/html

# Set permissions
RUN mkdir -p storage/framework/{cache,sessions,views} bootstrap/cache && \
    chmod -R 775 storage bootstrap/cache && \
    chown -R www-data:www-data storage bootstrap/cache

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]