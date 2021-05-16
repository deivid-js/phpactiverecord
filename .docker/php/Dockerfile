FROM php:8.0-apache

# composer
RUN apt-get update \
  && curl -sS https://getcomposer.org/installer |php \
  && mv composer.phar /usr/local/bin/composer

# libs
RUN apt-get update
RUN apt-get install -y \
  wget \
  unzip \
  curl \
  libzip-dev \
  libicu-dev \
  libpq-dev \
  vim
RUN docker-php-ext-configure \
  #intl \
  pgsql -with-pgsql=/usr/local/pgsql
RUN docker-php-ext-install \
  opcache \
  #intl \
  pdo_pgsql \
  pdo \
  pgsql \
  zip
RUN docker-php-ext-enable \
  opcache

# a nojeira do xdebug
RUN BEFORE_PWD=$(pwd) \
  && mkdir -p /opt/xdebug \
  && cd /opt/xdebug \
  && curl -k -L http://xdebug.org/files/xdebug-3.0.4.tgz | tar zx \
  && cd xdebug-3.0.4 \
  && phpize \
  && ./configure --enable-xdebug \
  && make clean \
  && sed -i 's/-O2/-O0/g' Makefile \
  && make \
  && make install \
  && cd "${BEFORE_PWD}" \
  && rm -r /opt/xdebug
RUN docker-php-ext-enable xdebug

RUN chown -R www-data:www-data /var/www \
  && a2enmod rewrite

COPY / /var/www/html