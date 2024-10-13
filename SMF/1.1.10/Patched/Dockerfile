FROM ezerous/php-apache:5.4-latest

LABEL version=0.1.0 maintainer="ezerous@gmail.com"

WORKDIR /var/www/html

ARG SMF_ZIP=smf_1-1-10-patched_install.zip

COPY $SMF_ZIP .

RUN unzip $SMF_ZIP && rm -f $SMF_ZIP