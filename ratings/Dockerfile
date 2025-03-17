# Use official PHP Apache Image
FROM php:7.4-apache AS base

# Install dependencies & Apache modules
RUN apt-get update && apt-get install -yqq unzip libzip-dev \
    && docker-php-ext-install pdo_mysql opcache zip \
    && a2enmod rewrite status \
    && rm -rf /var/lib/apt/lists/*

# Enable AutoProfile for PHP (opt-in beta)
RUN echo "instana.enable_auto_profile=1" > "/usr/local/etc/php/conf.d/zzz-instana-extras.ini"

# Set working directory
WORKDIR /var/www/html

# Copy Apache status config
COPY status.conf /etc/apache2/mods-available/status.conf

# ---- Composer Stage ----
FROM composer:latest AS composer_stage
WORKDIR /app
COPY html/composer.json html/composer.lock ./
RUN composer install --no-dev --no-interaction --prefer-dist

# ---- Application Stage ----
FROM base AS final
WORKDIR /var/www/html

# Copy application source files
COPY --chown=www-data:www-data html/ /var/www/html

# Copy installed dependencies from composer stage
COPY --from=composer_stage /app/vendor /var/www/html/vendor

# Set correct permissions
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

# Expose Apache port
EXPOSE 80

# Start Apache
CMD ["apache2-foreground"]

