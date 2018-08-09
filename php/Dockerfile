#Usaremos como imagen base la de php, para este caso la 7.2.8-fpm
FROM php:7.2.8-apache

MAINTAINER Código Electrónica <codigoelectronica@gmail.com>

#Instalamos las dependencias de php
RUN docker-php-ext-install pdo pdo_mysql

#Creamos la variable de la version de phalcon que vamos a usar
ENV PHALCON_VERSION=3.1.1

#Como realizaremos varias descargas, nos ubicamos en la carpeta tmp de la imagen de PHP
WORKDIR /var/tmp

#Realizamos la instalación de composer para el manejo de las dependencias
RUN curl -sS https://getcomposer.org/installer | php && \
	mv composer.phar /usr/local/bin/composer

#Install Source Dependancias
RUN apt-get -y update && apt-get -y install unzip

#Descargamos la libreria de phalcon con su herramienta phalcon dev tools
RUN set -xe && \
        curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz && \
        tar xzf v${PHALCON_VERSION}.tar.gz && cd cphalcon-${PHALCON_VERSION}/build && ./install && \
        echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini && \
        cd ../.. && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION} && \
        # Insall Phalcon Devtools, see https://github.com/phalcon/phalcon-devtools/
        curl -LO https://github.com/phalcon/phalcon-devtools/archive/v${PHALCON_VERSION}.tar.gz && \
        tar xzf v${PHALCON_VERSION}.tar.gz && \
        mv phalcon-devtools-${PHALCON_VERSION} /usr/local/phalcon-devtools && \
        ln -s /usr/local/phalcon-devtools/phalcon.php /usr/local/bin/phalcon

#Copiar php.ini
COPY php.ini /usr/local/etc/php/conf.d

# Enable Apache modules
RUN a2enmod headers rewrite

#Nos posicionamos en el directorio que deseamos trabajar
WORKDIR /var/www/html