ARG PHP_VERSION
FROM php:${PHP_VERSION:-8.3-fpm-alpine}

ARG ALPINE_MIRROR
RUN if [ ${ALPINE_MIRROR} != "" ]; then \
        alpine_mirror_url="${ALPINE_MIRROR}"; \
    else \
        alpine_mirror_url="dl-cdn.alpinelinux.org"; \
    fi; \
    sed -i "s/dl-cdn.alpinelinux.org/${alpine_mirror_url}/g" /etc/apk/repositories

RUN apk update && apk --no-cache add tzdata shadow ghostscript imagemagick

COPY install-php-extensions /usr/local/bin/install-php-extensions
RUN chmod +x /usr/local/bin/install-php-extensions

# Install PHP extensions
ARG PHP_EXTENSIONS
RUN php_extension_list=$(echo "${PHP_EXTENSIONS}" | sed 's/,/ /g') && install-php-extensions ${php_extension_list}

# Install Composer
ARG ALPINE_MIRROR
RUN if [ ${ALPINE_MIRROR} != "" ]; then \
        alpine_mirror_url="${ALPINE_MIRROR}"; \
    else \
        alpine_mirror_url="getcomposer.org"; \
    fi; \
    curl -o /usr/bin/composer https://${alpine_mirror_url}/composer.phar && chmod +x /usr/bin/composer

ENV COMPOSER_HOME=/tmp/composer

RUN groupmod -o -g 1000 www-data && usermod -o -u 1000 -g www-data www-data

WORKDIR /var/www

CMD ["php-fpm"]
