FROM ezerous/php-5.3.29-apache-2:latest

LABEL version=0.1.0 maintainer="ezerous@gmail.com"

WORKDIR /var/www/html

ARG SMF_ZIP=smf_1-1-10_install.zip

COPY $SMF_ZIP .

RUN unzip $SMF_ZIP && rm -f $SMF_ZIP