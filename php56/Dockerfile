FROM php:5.6-fpm

# update
RUN apt-get update 

# extension - except: imagick apc xdebug geoip redis
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng12-dev
RUN docker-php-ext-install iconv mcrypt mysql mysqli pdo pdo_mysql mbstring gd
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ 

RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*
RUN pecl install imagick && docker-php-ext-enable imagick

# extension - redis
ENV PHP_REDIS_VERSION 2.2.7
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/${PHP_REDIS_VERSION}.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mv phpredis-${PHP_REDIS_VERSION} /usr/src/php/ext/redis \
    && docker-php-ext-install redis

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=/usr/local/bin
    
RUN usermod -u 1000 www-data

# 指定工作目录
WORKDIR /apps

# COPY config/php.ini /usr/local/etc/php/


EXPOSE 9000

VOLUME ["/usr/local/etc/php/conf.d/app.ini"]
VOLUME ["/etc/php-fpm.conf"]

CMD ["php-fpm", "-F"]
