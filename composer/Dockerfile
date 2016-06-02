FROM alpine:latest

MAINTAINER Fernando Fabricio dos Santos <ferfabricio@gmail.com>

ENV TIMEZONE America/Sao_Paulo
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST 100M

RUN echo "@testing http://dl-4.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
RUN     apk update && \
        apk upgrade && \
        apk add --update tzdata && \
        cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
        echo "${TIMEZONE}" > /etc/timezone && \
        apk add --update \
        bash \
        git \
                php-mcrypt \
                php-soap \
                php-openssl \
                php-gmp \
                php-pdo_odbc \
                php-json \
                php-dom \
                php-pdo \
                php-zip \
                php-mysql \
                php-sqlite3 \
                php-apcu \
                php-pdo_pgsql \
                php-bcmath \
                php-gd \
                php-xcache \
                php-odbc \
                php-pdo_mysql \
                php-pdo_sqlite \
                php-gettext \
                php-xmlreader \
                php-xmlrpc \
                php-bz2 \
                php-memcache \
                php-mssql \
                php-iconv \
                php-pdo_dblib \
                php-curl \
                php-ctype \
                php-phar \
                php-phalcon \
                php-cli \
                php-xdebug@testing && \
        sed -i "s|;date.timezone =.*|date.timezone = ${TIMEZONE}|" /etc/php/php.ini && \
        sed -i "s|memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|" /etc/php/php.ini && \
        sed -i "s|upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|" /etc/php/php.ini && \
        sed -i "s|max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|" /etc/php/php.ini && \
        sed -i "s|post_max_size =.*|max_file_uploads = ${PHP_MAX_POST}|" /etc/php/php.ini && \
        sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php/php.ini && \
        sed -i "s/extension=xdebug.so/zend_extension=xdebug.so/" /etc/php/conf.d/xdebug.ini && \
        mkdir /www && \
        apk del tzdata && \
        rm -rf /var/cache/apk/*

ENV COMPOSER_HOME /composer

ENV PATH /composer/vendor/bin:$PATH

ENV COMPOSER_ALLOW_SUPERUSER 1

RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
  && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
  && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }"

ENV COMPOSER_VERSION 1.1.0

RUN php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION} && rm -rf /tmp/composer-setup.php

WORKDIR /app

VOLUME ["/app"]

CMD ["-"]
ENTRYPOINT ["composer"]