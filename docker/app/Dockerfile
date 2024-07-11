FROM php:8.2-fpm
ARG NODE_VERSION=18
ARG APP_PATH='/var/www/html'

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN apt-get update  \
    && groupadd -g ${PGID} admin \
    && useradd -l -u ${PUID} -g admin -m admin -G admin \
    && usermod -a -G www-data admin \
    && usermod -p "*" admin -s /bin/bash \
    && mkdir -p ${APP_PATH} \
    && chgrp www-data ${APP_PATH} \
    && chmod -R g+rwxs ${APP_PATH} \
    && mkdir -p /etc/apt/keyrings \
    && apt-get install -y \
      ca-certificates \
      curl  \
      gnupg \
      apt-utils \
      libpq-dev \
      libpng-dev \
      libzip-dev \
      zip unzip \
      git  \
      nano \
      cron  \
      && docker-php-ext-install pdo_mysql \
      && docker-php-ext-install bcmath \
      && docker-php-ext-install gd \
      && docker-php-ext-install zip \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \


# Копирования конфиг файла docker/app/php.ini
COPY php.ini /usr/local/etc/php/conf.d/php.ini

# Install composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php -- \
    --filename=composer \
    --install-dir=/usr/local/bin

# NodeJS
RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_VERSION.x nodistro main" > /etc/apt/sources.list.d/nodesource.list \
    && apt-get update \
    && apt-get install -y nodejs \
    && npm install -g npm \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www/html
