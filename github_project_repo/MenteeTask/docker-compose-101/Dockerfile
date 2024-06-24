# Base image
FROM ubuntu:20.04

# Metadata
LABEL maintainer="hello@kesaralive.com"
LABEL description="Apache / PHP development environment"

# Set environment variable to avoid prompts during package installation
ARG DEBIAN_FRONTEND=noninteractive

# Update and install necessary packages
RUN apt-get update && apt-get install -y \
    lsb-release \
    ca-certificates \
    apt-transport-https \
    software-properties-common \
    locales \
    && apt-get clean all

# Add the PHP repository
RUN add-apt-repository ppa:ondrej/php

# Update and install Apache, PHP, and necessary PHP extensions
RUN apt-get update && apt-get install -y \
    apache2 \
    php8.0 \
    libapache2-mod-php8.0 \
    php8.0-bcmath \
    php8.0-gd \
    php8.0-sqlite \
    php8.0-mysql \
    php8.0-curl \
    php8.0-xml \
    php8.0-mbstring \
    php8.0-zip \
    nano \
    && apt-get clean all

# Configure locales
RUN locale-gen fr_FR.UTF-8 \
    && locale-gen en_US.UTF-8 \
    && locale-gen de_DE.UTF-8

# Configure PHP
RUN sed -i -e 's/^error_reporting\s*=.*/error_reporting = E_ALL/' /etc/php/8.0/apache2/php.ini \
    && sed -i -e 's/^display_errors\s*=.*/display_errors = On/' /etc/php/8.0/apache2/php.ini \
    && sed -i -e 's/^zlib.output_compression\s*=.*/zlib.output_compression = Off/' /etc/php/8.0/apache2/php.ini

# Set environment variable for nano usage
ENV TERM xterm

# Configure Apache
RUN a2enmod rewrite \
    && echo "ServerName localhost" >> /etc/apache2/apache2.conf \
    && sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

# Set permissions for /var/www
RUN chgrp -R www-data /var/www \
    && find /var/www -type d -exec chmod 775 {} + \
    && find /var/www -type f -exec chmod 664 {} +

# Expose port 80
EXPOSE 80

# Start Apache in the foreground
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

