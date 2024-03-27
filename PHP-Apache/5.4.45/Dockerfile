FROM php:5.4-apache

LABEL version=0.2.0 maintainer="ezerous@gmail.com"

# Apache
COPY apache2.conf /etc/apache2/apache2.conf

# PHP
RUN docker-php-ext-install mysql mysqli pdo_mysql

COPY php.ini /usr/local/lib/php.ini

# For correct permissions and improved security
USER www-data

EXPOSE 80

CMD ["apache2-foreground"]
