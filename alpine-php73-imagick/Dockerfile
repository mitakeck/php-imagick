FROM php:7.3-fpm-alpine

RUN set -x \
  && apk add --update apache2 php7-apache2 openssh libpng-dev libjpeg-turbo-dev libxml2-dev autoconf g++ imagemagick-dev libtool make pcre-dev \
  && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
  && docker-php-ext-install -j${NPROC} pdo pdo_mysql soap \
  && pecl install xdebug \
  && docker-php-ext-enable -j${NPROC} xdebug \
  && pecl install imagick \
  && docker-php-ext-install -j${NPROC} intl \
  && docker-php-ext-enable imagick \
  && apk del --no-cache --purge build-dependencies \
  && rm -rf /tmp/* /var/cache/apk/*

RUN rm -rf /var/www/localhost/htdocs \
  && ln -s /var/www/html /var/www/localhost/htdocs 

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer config -g repositories.packagist composer https://packagist.jp \
  && composer self-update

EXPOSE 80
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]