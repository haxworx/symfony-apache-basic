FROM php:8.3-apache

ENV TZ=Europe/London
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite
RUN a2enmod ssl

RUN apt-get update \
  && apt-get install -y libzip-dev git wget --no-install-recommends \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
 
RUN docker-php-ext-install pdo mysqli pdo_mysql zip;

RUN set -eux; \
    apt-get update && apt-get -y upgrade; \
    mkdir /etc/apache2/ssl && cd /etc/apache2/ssl; \
    apt-get -y install openssl; \
    openssl req -x509 -nodes -new -sha256 -days 1024 -newkey rsa:2048 -keyout localhost.key -out localhost.pem -subj "/C=FR/CN=localhost"; \
    openssl x509 -outform pem -in localhost.pem -out localhost.crt;
