FROM php:7.1-alpine

LABEL org.opencontainers.image.source="https://github.com/104lab/docker-lazy-php"

ENV COMPOSER_PATH /usr/local/bin/composer

# Install extensions
RUN set -xe && \
        # Runtime dependency
        apk add --no-cache \
            libgmpxx \
            libjpeg-turbo \
            libmcrypt \
            libmemcached-libs \
            libpng \
            make \
            unzip \
        && \
        apk add --no-cache --virtual .build-deps \
            autoconf \
            cyrus-sasl-dev \
            g++ \
            gmp-dev \
            libjpeg-turbo-dev \
            libmcrypt-dev \
            libmemcached-dev \
            libpng-dev \
            libxml2-dev \
            zlib-dev \
        && \
        docker-php-ext-configure gd \
            --with-jpeg-dir=/usr/include/ \
            --with-png-dir=/usr/include/ \
        && \
        docker-php-ext-install -j $(getconf _NPROCESSORS_ONLN) \
            bcmath \
            exif \
            gd \
            gmp \
            mcrypt \
            pdo_mysql \
            soap \
            sockets \
            zip \
        && \
        pecl install \
            memcached \
            mongodb-1.11.1 \
            redis \
        && \
        docker-php-ext-enable \
            memcached \
            mongodb \
            redis \
        && \
        apk del .build-deps \
        && \
        php -m

# Install Composer v2
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer
