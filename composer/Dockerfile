FROM alpine:latest

MAINTAINER Fernando Fabricio dos Santos <ferfabricio@gmail.com>

ENV TIMEZONE America/Sao_Paulo
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST 100M

RUN     apk update && \
        apk upgrade && \
        apk add --update tzdata && \
        cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
        echo "${TIMEZONE}" > /etc/timezone && \
        apk add --update \
        bash \
        git \
                php-apcu \
                php-bcmath \
                php-bz2 \
                php-calendar \
                php-cli \
                php-ctype \
                php-curl \
                php-dom \
                php-gd \
                php-gettext \
                php-gmp \
                php-iconv \
                php-json \
                php-mcrypt \
                php-memcache \
                php-mssql \
                php-mysql \
                php-mysqli \
                php-odbc \
                php-openssl \
                php-pdo \
                php-pdo_dblib \
                php-pdo_mysql \
                php-pdo_odbc \
                php-pdo_pgsql \
                php-pdo_sqlite \
                php-phalcon \
                php-phar \
                php-soap \
                php-sqlite3 \
                php-xcache \
                php-xmlreader \
                php-xmlrpc \
                php-xdebug \
                php-zip && \
        sed -i "s|;date.timezone =.*|date.timezone = ${TIMEZONE}|" /etc/php/php.ini && \
        sed -i "s|memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|" /etc/php/php.ini && \
        sed -i "s|upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|" /etc/php/php.ini && \
        sed -i "s|max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|" /etc/php/php.ini && \
        sed -i "s|post_max_size =.*|max_file_uploads = ${PHP_MAX_POST}|" /etc/php/php.ini && \
        sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php/php.ini && \
        mkdir /www && \
        apk del tzdata && \
        rm -rf /var/cache/apk/*

ENV COMPOSER_HOME /composer

ENV PATH /composer/vendor/bin:$PATH

ENV COMPOSER_ALLOW_SUPERUSER 1

RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
  && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
  && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }"

ENV COMPOSER_VERSION 1.1.2

RUN php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION} && rm -rf /tmp/composer-setup.php

WORKDIR /app

VOLUME ["/app"]

CMD ["-"]
ENTRYPOINT ["composer"]
