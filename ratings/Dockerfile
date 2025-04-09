# ---- Base PHP Apache Stage ----
FROM php:8.1-apache AS base

# Install required dependencies and enable Apache/PHP modules
RUN apt-get update && apt-get install -yqq unzip libzip-dev \
    && docker-php-ext-install pdo_mysql opcache zip \
    && a2enmod rewrite status \
    && rm -rf /var/lib/apt/lists/*

# Enable Instana PHP AutoProfile (optional)
RUN echo "instana.enable_auto_profile=1" > "/usr/local/etc/php/conf.d/zzz-instana-extras.ini"

# Set working directory
WORKDIR /var/www/html

# Copy Apache status config
COPY status.conf /etc/apache2/mods-available/status.conf

# ---- Composer Dependencies Stage ----
FROM composer:latest AS composer_stage

WORKDIR /app

# Copy composer files
COPY html/composer.json ./
COPY html/composer.lock ./  # ✅ You now have this file

# Install PHP dependencies
RUN composer install --no-dev --no-interaction --prefer-dist

# ---- Final Application Stage ----
FROM base AS final

WORKDIR /var/www/html

# Copy application source
COPY --chown=www-data:www-data html/ /var/www/html

# Copy vendor folder from composer build
COPY --from=composer_stage /app/vendor /var/www/html/vendor

# Set file permissions
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

# Expose Apache port
EXPOSE 80

# Start Apache
CMD ["apache2-foreground"]

