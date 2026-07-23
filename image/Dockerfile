# osTicket on a current, supported runtime.
#
# The community images on Docker Hub (osticket/osticket, campbellsoftwaresolutions/osticket)
# were last built in 2020 on PHP 7.2/7.3 — both long past end-of-life. Worse, the official
# one git-clones osTicket master at container start, so a 2020 PHP runtime ends up executing
# 2026 osTicket code and dies with a parse error. Running EOL PHP in a "helpdesk" is exactly
# the kind of thing this portfolio is meant to show I would NOT do, so I build my own image
# on a supported PHP 8.2 base and pin the osTicket release.
#
# Build:  docker compose build     (compose passes OSTICKET_VERSION)
# Pin the Debian 12 (bookworm) variant explicitly: Debian 13 (trixie) dropped
# libc-client-dev (UW-IMAP), which the PHP imap extension links against. Bookworm
# is supported into 2028 and still ships it, so IMAP mail-fetching keeps working.
ARG PHP_VERSION=8.2
FROM php:${PHP_VERSION}-apache-bookworm

ARG OSTICKET_VERSION=1.18.4

# --- system libs needed to compile the PHP extensions osTicket needs ---
RUN apt-get update && apt-get install -y --no-install-recommends \
        libpng-dev libjpeg-dev libfreetype6-dev \
        libicu-dev \
        libc-client-dev libkrb5-dev \
        libonig-dev \
        libzip-dev \
        unzip \
    && rm -rf /var/lib/apt/lists/*

# --- PHP extensions required / recommended by osTicket 1.18 ---
#   mysqli  – database driver
#   gd      – image/captcha rendering (needs freetype + jpeg)
#   imap    – inbound email fetching
#   intl    – internationalisation
#   mbstring– multibyte strings
#   opcache – bytecode cache (performance)
#   zip     – plugin/language pack handling
# dom/xml, fileinfo, iconv, phar are already enabled in the base image.
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j"$(nproc)" \
        mysqli gd imap intl mbstring opcache zip \
    && pecl install apcu \
    && docker-php-ext-enable apcu

# osTicket wants URL rewriting for clean paths.
RUN a2enmod rewrite

# Set a global ServerName so Apache doesn't log the "could not determine FQDN" warning.
RUN echo 'ServerName localhost' > /etc/apache2/conf-available/servername.conf \
    && a2enconf servername

# Sensible PHP settings for a ticketing system (large mail attachments, etc.).
RUN { \
        echo 'upload_max_filesize=25M'; \
        echo 'post_max_size=30M'; \
        echo 'memory_limit=256M'; \
        echo 'max_execution_time=120'; \
        echo 'expose_php=Off'; \
    } > /usr/local/etc/php/conf.d/osticket.ini

# --- fetch and lay down the pinned osTicket release ---
# The release zip contains upload/ (the webroot) and scripts/ (cron helper).
ADD https://github.com/osTicket/osTicket/releases/download/v${OSTICKET_VERSION}/osTicket-v${OSTICKET_VERSION}.zip /tmp/osticket.zip
RUN unzip -q /tmp/osticket.zip -d /tmp/ost \
    && rm -rf /var/www/html \
    && mv /tmp/ost/upload /var/www/html \
    && mkdir -p /var/www/scripts \
    && cp -r /tmp/ost/scripts/* /var/www/scripts/ 2>/dev/null || true \
    && rm -rf /tmp/osticket.zip /tmp/ost \
    # osTicket reads config from include/ost-config.php; the installer writes it on first run.
    # Seed it from the sample and make it writable so the web installer can populate it.
    && cp /var/www/html/include/ost-sampleconfig.php /var/www/html/include/ost-config.php \
    && chown -R www-data:www-data /var/www/html \
    && chmod 0666 /var/www/html/include/ost-config.php

WORKDIR /var/www/html
EXPOSE 80

HEALTHCHECK --interval=15s --timeout=5s --start-period=40s --retries=5 \
    CMD php -r 'exit(extension_loaded("mysqli") && is_file("/var/www/html/main.inc.php") ? 0 : 1);'
