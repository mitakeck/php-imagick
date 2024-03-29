FROM php:7.2-fpm-alpine

RUN apk add --update php7-mbstring openssh libpng-dev libjpeg-turbo-dev libxml2-dev autoconf g++ imagemagick-dev libtool make pcre-dev icu-dev \
  apache2 php7-apache2 php7-pdo php7-session php7-openssl php7-mbstring php7-apcu php7-intl php7-mcrypt php7-json php7-gd php7-curl php7-fpm php7-mysqlnd php7-pgsql php7-sqlite3 php7-phar php7-imagick \
  && docker-php-ext-install pdo pdo_mysql soap intl \
  && docker-php-source extract \
  && pecl install xdebug \
  && pecl install imagick \
  && docker-php-ext-enable imagick \
  && docker-php-ext-install mbstring \
  && echo "extension=intl" >> /etc/php7/php.ini \
  && echo "extension=mbstring" >> /etc/php7/php.ini \
  && echo "extension=pdo_mysql" >> /etc/php7/php.ini \
  && rm -rf /tmp/* /var/cache/apk/* \
  && sed -i '/LoadModule rewrite_module/s/^#//g' /etc/apache2/httpd.conf \
  && sed -i -e "s/AllowOverride None/AllowOverride ALL/" /etc/apache2/httpd.conf

RUN rm -rf /var/www/localhost/htdocs \
  && ln -s /var/www/html /var/www/localhost/htdocs 

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer config -g repositories.packagist composer https://packagist.jp \
  && composer self-update

EXPOSE 80
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]