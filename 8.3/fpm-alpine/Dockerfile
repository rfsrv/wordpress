ARG PHPREDIS_VERSION=6.0.2

FROM wordpress:php8.3-fpm-alpine

# CVE-2026-109618 and CVE-2026-109619: reject unpatched upstream images.
RUN php -r 'if (version_compare(PHP_VERSION, "8.3.36", "<")) { fwrite(STDERR, "PHP 8.3.36 or newer is required\n"); exit(1); }'

ARG PHPREDIS_VERSION

RUN set -eux; \
    apk add --no-cache --virtual .build-deps \
        autoconf \
        gcc \
        g++ \
        make \
        linux-headers; \
    pecl install redis-${PHPREDIS_VERSION}; \
    docker-php-ext-enable redis; \
    apk del .build-deps; \
    php -r "if (!extension_loaded('redis')) { exit(1); }"
