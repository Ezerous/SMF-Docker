FROM php:5.4-apache

LABEL version=0.2.0 maintainer="ezerous@gmail.com"

RUN echo "deb http://archive.debian.org/debian/ jessie main non-free contrib" > /etc/apt/sources.list \
    && echo "deb http://archive.debian.org/debian-security/ jessie/updates main" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y --force-yes unzip less nano && rm -r /var/lib/apt/lists/*

# Apache
COPY apache2.conf /etc/apache2/apache2.conf

# PHP
RUN docker-php-ext-install mysql mysqli pdo_mysql

COPY php.ini /usr/local/lib/php.ini

# For correct permissions and improved security
USER www-data

EXPOSE 80

CMD ["apache2-foreground"]
