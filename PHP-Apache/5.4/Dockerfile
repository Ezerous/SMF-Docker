FROM php:5.4-apache

LABEL version=0.2.1 maintainer="ezerous@gmail.com"

RUN echo "deb http://archive.debian.org/debian/ jessie main non-free contrib" > /etc/apt/sources.list \
    && echo "deb http://archive.debian.org/debian-security/ jessie/updates main" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y --force-yes unzip less nano && rm -r /var/lib/apt/lists/*

# Apache
COPY apache2.conf /etc/apache2/apache2.conf

# PHP
RUN docker-php-ext-install mysql mysqli pdo_mysql

RUN mkdir -p /usr/local/lib/php/session && chmod 733 /usr/local/lib/php/session

COPY php.ini /usr/local/etc/php/php.ini

# For correct permissions and improved security
USER www-data

EXPOSE 80

CMD ["apache2-foreground"]
