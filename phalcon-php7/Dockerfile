FROM alpine:latest

MAINTAINER Fernando Fabricio dos Santos <ferfabricio@gmail.com>

ENV TIMEZONE America/Sao_Paulo
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST 100M

RUN echo "@testing http://dl-4.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
RUN apk update && \
	apk upgrade && \
	apk add --update tzdata && \
	cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
	echo "${TIMEZONE}" > /etc/timezone && \
	apk upgrade && \
	apk add --update tzdata && \
	apk add --update \
	make \
	gcc \
	re2c \
	g++ \
	autoconf \
	bash \
	git \
	curl \
	php7-mcrypt@testing \
	php7-mbstring@testing \
	php7-soap@testing \
	php7-openssl@testing \
	php7-gmp@testing \
	php7-pdo_odbc@testing \
	php7-json@testing \
	php7-dom@testing \
	php7-pdo@testing \
	php7-zip@testing \
	php7-pdo_mysql@testing \
	php7-sqlite3@testing \
	php7-pdo_pgsql@testing \
	php7-bcmath@testing \
	php7-odbc@testing \
	php7-pdo_mysql@testing \
	php7-pdo_sqlite@testing \
	php7-gettext@testing \
	php7-xmlreader@testing \
	php7-xmlrpc@testing \
	php7-bz2@testing \
	php7-iconv@testing \
	php7-pdo_dblib@testing \
	php7-curl@testing \
	php7-ctype@testing \
	php7-phar@testing \
	php7-xdebug@testing \
	php7-dev@testing

RUN ln -s /usr/bin/php7 /usr/bin/php && \
	ln -s /usr/bin/php-config7 /usr/bin/php-config && \
	ln -s /usr/bin/phpize7 /usr/bin/phpize && \
	cd /var/opt && \
	git clone https://github.com/phalcon/zephir.git --depth=1 && \
	cd zephir && \
	./install-nosudo && \
	cd /var/opt && \
	git clone https://github.com/phalcon/cphalcon.git --depth=1 -b 2.1.x && \
	cd cphalcon && \
	~/bin/zephir build --backend=ZendEngine3 && \
	cp ext/modules/phalcon.so /usr/lib/php7/modules/ && \
	echo "extension=phalcon.so" > /etc/php7/conf.d/30-phalcon.ini

RUN mkdir /app && \
	apk del tzdata && \
	rm -rf /var/cache/apk/*

RUN sed -i "s|;date.timezone =.*|date.timezone = ${TIMEZONE}|" /etc/php7/php.ini && \
	sed -i "s|memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|" /etc/php7/php.ini && \
	sed -i "s|upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|" /etc/php7/php.ini && \
	sed -i "s|max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|" /etc/php7/php.ini && \
	sed -i "s|post_max_size =.*|max_file_uploads = ${PHP_MAX_POST}|" /etc/php7/php.ini && \
	sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php7/php.ini

ENV COMPOSER_HOME /composer

ENV PATH /composer/vendor/bin:$PATH

ENV COMPOSER_ALLOW_SUPERUSER 1

ENV COMPOSER_VERSION 1.1.3

RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer && \
    php7 /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION} && rm -rf /tmp/composer-setup.php

WORKDIR /app

VOLUME ["/app"]

CMD ["-"]
ENTRYPOINT ["composer"]