# Utilizamos una imagen oficial de PHP 8.2 como imagen base
FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    git \
    curl \
    vim \
    unzip \
    libicu-dev \
    libzip-dev \
    && rm -rf /var/lib/apt/lists/* \


RUN apt-get install -y php-pear

RUN pecl install raphf

RUN pecl install pecl_http \
    && docker-php-ext-enable http

COPY apache-config.conf /etc/apache2/conf-available/apache-config.conf
RUN a2enconf apache-config

RUN docker-php-ext-install pdo pdo_mysql intl zip

RUN a2enmod rewrite

ENV APACHE_DOCUMENT_ROOT /var/www/PaymentSystem/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN docker-php-ext-install opcache
RUN echo "opcache.enable=1\n\
opcache.memory_consumption=128\n\
opcache.interned_strings_buffer=8\n\
opcache.max_accelerated_files=10000\n\
opcache.revalidate_freq=0\n\
opcache.validate_timestamps=1" > /usr/local/etc/php/conf.d/opcache.ini

COPY apache-config.conf /etc/apache2/conf-available/apache-config.conf
RUN a2enconf apache-config

WORKDIR /var/www/PaymentSystem

RUN groupadd -r raul && \
    useradd -r -g raul raul && \
    chown -R raul:raul /var/www/PaymentSystem

EXPOSE 80

CMD ["apache2-foreground"]
