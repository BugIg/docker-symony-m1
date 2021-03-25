ARG PHP_VERSION=""
FROM php:${PHP_VERSION}-apache

# install all the dependencies and enable PHP modules
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
      procps \
      nano \
      git \
      unzip \
      libicu-dev \
      zlib1g-dev \
      libxml2 \
      libxml2-dev \
      libreadline-dev \
      libpng-dev \
      libonig-dev \
      libxml2-dev \
      supervisor \
      cron \
      sudo \
      libzip-dev \
      wget \
      librabbitmq-dev \
    && pecl install amqp \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-configure intl \
    && docker-php-ext-install \
      pdo_mysql \
      sockets \
      intl \
      opcache \
      zip \
      gd \
      mbstring \
      exif \
      pcntl \
      bcmath \
    && docker-php-ext-enable amqp \
    && rm -rf /tmp/* \
    && rm -rf /var/list/apt/* \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean


# Configure apache
RUN a2enmod rewrite
RUN a2enmod proxy
RUN a2enmod headers

#RUN pecl install xdebug \
#    && docker-php-ext-enable xdebug

RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
       mv composer.phar /usr/local/bin/composer

RUN groupadd dev -g 999
RUN useradd dev -g dev -d /var/www/html -m
RUN chown -R dev:dev /var/www/html

RUN rm -rf /var/lib/apt/lists/*

RUN service apache2 restart

EXPOSE 80

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
