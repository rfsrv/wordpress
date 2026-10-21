ARG PHPREDIS_VERSION=6.0.2

FROM wordpress:php8.2-apache

# CVE-2026-109618 and CVE-2026-109619: reject unpatched upstream images.
RUN php -r 'if (version_compare(PHP_VERSION, "8.2.35", "<")) { fwrite(STDERR, "PHP 8.2.35 or newer is required\n"); exit(1); }'

ARG PHPREDIS_VERSION

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends libssl-dev; \
    pecl install redis-${PHPREDIS_VERSION}; \
    docker-php-ext-enable redis; \
    apt-get purge -y --auto-remove libssl-dev; \
    rm -rf /var/lib/apt/lists/*; \
    php -r "if (!extension_loaded('redis')) { exit(1); }"
