FROM debian:stretch-slim

LABEL version=0.2.0 maintainer="ezerous@gmail.com"

RUN echo "deb http://archive.debian.org/debian/ stretch main non-free contrib" > /etc/apt/sources.list \
    && echo "deb http://archive.debian.org/debian-security/ stretch/updates main" >> /etc/apt/sources.list

RUN apt-get update && apt-get install -y curl unzip gnupg nano libgcrypt11-dev zlib1g-dev && rm -r /var/lib/apt/lists/*

##<apache2>##
RUN apt-get update && apt-get install -y apache2-bin apache2-dev apache2 --no-install-recommends && rm -rf /var/lib/apt/lists/*

RUN rm -rf /var/www/html && mkdir -p /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html && chown -R www-data:www-data /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html

# Apache + PHP requires preforking Apache for best results
RUN a2dismod mpm_event && a2enmod mpm_prefork

RUN mv /etc/apache2/apache2.conf /etc/apache2/apache2.conf.dist
COPY apache2.conf /etc/apache2/apache2.conf
##</apache2>##

# Add key for PHP verification
RUN gpg --keyserver keyserver.ubuntu.com --recv-keys 0B96609E270F565C13292B24C13C70B87267B52D 0A95E9A026542D53835E3F3A7DEC4E69FC9C83D7

ENV PHP_VERSION 5.3.29

# PHP 5.3 needs older autoconf
RUN set -x \
	&& apt-get update && apt-get install -y autoconf2.13 && rm -r /var/lib/apt/lists/* \
	&& autoconf --version \
	&& whereis autoconf2.13 \
	&& curl -SLO http://launchpadlibrarian.net/140087283/libbison-dev_2.7.1.dfsg-1_amd64.deb \
	&& curl -SLO http://launchpadlibrarian.net/140087282/bison_2.7.1.dfsg-1_amd64.deb \
	&& dpkg -i libbison-dev_2.7.1.dfsg-1_amd64.deb \
	&& dpkg -i bison_2.7.1.dfsg-1_amd64.deb \
	&& rm *.deb \
	&& curl -SL "http://php.net/get/php-$PHP_VERSION.tar.bz2/from/this/mirror" -o php.tar.bz2 \
	&& curl -SL "http://php.net/get/php-$PHP_VERSION.tar.bz2.asc/from/this/mirror" -o php.tar.bz2.asc \
	&& gpg --verify php.tar.bz2.asc

ENV PHP_AUTOCONF /usr/bin/autoconf2.13

RUN curl -O https://www.openssl.org/source/old/1.0.2/openssl-1.0.2u.tar.gz \
	&& tar xf openssl-1.0.2u.tar.gz \
	&& cd openssl-1.0.2u \
	&& ./config -fPIC shared --prefix=/opt/openssl-1.0.2u \
	&& make \
	&& make install \
	&& cd ..

RUN apt-get update && apt-get install -y autoconf2.13 libxml2-dev default-libmysqlclient-dev && rm -r /var/lib/apt/lists/* \
	&& mkdir -p /usr/src/php \
	&& tar -xf php.tar.bz2 -C /usr/src/php --strip-components=1 \
	&& rm php.tar.bz2* \
	&& cd /usr/src/php \
	&& ./buildconf --force \
	&& ./configure --disable-cgi \
		$(command -v apxs2 > /dev/null 2>&1 && echo '--with-apxs2' || true) \
		--with-mysql \
		--with-mysqli \
		--with-pdo-mysql \
		--with-openssl=/opt/openssl-1.0.2u \
		--with-zlib \
	&& make -j"$(nproc)" \
	&& make install \
	&& dpkg -r bison libbison-dev \
	&& apt-get purge -y --auto-remove autoconf2.13 \
	&& rm -r /usr/src/php

RUN mkdir -p /usr/local/lib/php/session && chmod 733 /usr/local/lib/php/session

COPY php.ini /usr/local/lib/php.ini

WORKDIR /var/www/html

# For correct permissions and improved security
USER www-data

EXPOSE 80
CMD ["apache2", "-DFOREGROUND"]
