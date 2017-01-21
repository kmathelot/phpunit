FROM php:5.6-alpine
MAINTAINER KevinM <kevin.mathelot@gmail.com>

# update base image
RUN apk update \
    && apk add ca-certificates \
    && update-ca-certificates \
    && apk add openssl

# install stuff
RUN apk add --update \
    php5-dev \
    autoconf \
    build-base \
    libtool \
    git

# Xdebug 2.5.0
RUN wget -qO- http://xdebug.org/files/xdebug-2.5.0.tgz | tar xz -C /tmp/ && \
    cd /tmp/xdebug-2.5.0 && \
    phpize && \
    ./configure && \
    make && \
    cp modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20131226 && \
    echo 'zend_extension = /usr/local/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so' > /usr/local/etc/php/conf.d/xdebug.ini

# Php extensions
RUN docker-php-ext-install \
    pdo \
    pdo_mysql

# composer
RUN cd /tmp && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    composer require "phpunit/phpunit:~5.0.7" --prefer-source --no-interaction && \
    ln -s /tmp/vendor/bin/phpunit /usr/local/bin/phpunit

# Set up the application directory.
VOLUME ["/app"]
WORKDIR /app

# Set up the command arguments.
ENTRYPOINT ["/usr/local/bin/phpunit"]
CMD ["--help"]
