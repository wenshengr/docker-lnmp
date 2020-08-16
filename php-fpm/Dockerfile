ARG PHP_VERSION

FROM php:${PHP_VERSION}-fpm-alpine

LABEL maintainer="wenshengr <wenshengr@163.com>"

## Change Aliyun Mirrors:
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

## Install System Extensions:
RUN apk add --no-cache --virtual .build-deps \
        autoconf \
        build-base \
        linux-headers \
        libaio-dev \
        zlib-dev \
        curl \
        git \
        freetype-dev \
        libjpeg-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libtool \
        libbz2 \
        bzip2 \
        bzip2-dev \
        libstdc++ \
        libxslt-dev \
        openldap-dev \
        imagemagick-dev \
        make \
        unzip \
        wget \
        libmemcached-dev \
        libzip-dev \
        yaml-dev \
        rabbitmq-c-dev



# Install PHP Core Extensions:

# Core Extensions For Default Installation:
RUN docker-php-ext-install \
        bcmath \
        pdo_mysql \
        mysqli \
        sockets \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd


# Core Extensions pcntl:
ARG INSTALL_PCNTL=true
RUN if [ ${INSTALL_PCNTL} = true ]; then \
    docker-php-ext-install pcntl \
;fi


# Core Extensions opcache:
ARG INSTALL_OPCACHE=true
RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi



# PECL Install PHP Extensions

# PECL Extensions reids:
ARG INSTALL_REDIS=true
ARG INSTALL_REDIS_VERSION
RUN if [ ${INSTALL_REDIS} = true ]; then \
    pecl install redis-${INSTALL_REDIS_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
;fi


# PECL Extensions mongodb:
ARG INSTALL_MONGODB=true
ARG INSTALL_MONGODB_VERSION
RUN if [ ${INSTALL_MONGODB} = true ]; then \
    pecl install mongodb-${INSTALL_MONGODB_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable mongodb \
;fi


# PECL Extensions memcached:
ARG INSTALL_MEMCACHED=true
ARG INSTALL_MEMCACHED_VERSION
RUN if [ ${INSTALL_MEMCACHED} = true ]; then \
    pecl install memcached-${INSTALL_MEMCACHED_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable memcached \
;fi


# PECL Extensions swoole:
ARG INSTALL_SWOOLE=true
ARG INSTALL_SWOOLE_VERSION
RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    pecl install swoole-${INSTALL_SWOOLE_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable swoole \
;fi


# PECL Extensions yaconf:
ARG INSTALL_YACONF=true
ARG INSTALL_YACONF_VERSION
RUN if [ ${INSTALL_YACONF} = true ]; then \
    pecl install yaconf-${INSTALL_YACONF_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable yaconf \
;fi

# PECL Extensions yaf:
ARG INSTALL_YAF=true
ARG INSTALL_YAF_VERSION
RUN if [ ${INSTALL_YAF} = true ]; then \
    pecl install yaf-${INSTALL_YAF_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable yaf \
;fi


# PECL Extensions SeasLog:
ARG INSTALL_SEASLOG=true
ARG INSTALL_SEASLOG_VERSION
RUN if [ ${INSTALL_SEASLOG} = true ]; then \
    pecl install SeasLog-${INSTALL_SEASLOG_VERSION} \
    && rm -rf /tmp/pear \
#&& docker-php-ext-configure /usr/src/php/ext/SeasLog
#&& docker-php-ext-install /usr/src/php/ext/SeasLog
    && docker-php-ext-enable seaslog \
;fi


# PECL Extensions zip:
ARG INSTALL_ZIP=true
ARG INSTALL_ZIP_VERSION
RUN if [ ${INSTALL_ZIP} = true ]; then \
    pecl install zip-${INSTALL_ZIP_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable zip \
;fi

# PECL Extensions zip:
ARG INSTALL_YAML=true
ARG INSTALL_YAML_VERSION
RUN if [ ${INSTALL_YAML} = true ]; then \
    pecl install yaml-${INSTALL_YAML_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable yaml \
;fi

# PECL Extensions amqp:
ARG INSTALL_AMQP=true
ARG INSTALL_AMQP_VERSION
RUN if [ ${INSTALL_AMQP} = true ]; then \
    pecl install amqp-${INSTALL_AMQP_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable amqp \
;fi


# Install Composer:
ARG INSTALL_COMPOSER=true
RUN if [ ${INSTALL_COMPOSER} = true ]; then \
    curl -sS http://getcomposer.org/installer | php \
    && mv ./composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer \
;fi

RUN apk del build-base \
    	linux-headers \
    	libaio-dev \
    && rm -rf /var/cache/apk/*


VOLUME /var/www
WORKDIR /var/www

EXPOSE 9000
CMD ["php-fpm"]