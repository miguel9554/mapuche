FROM php:7.1-apache-buster

# Instalamos las extensiones de php, composer, yarn, habilitamos apache rewrite
RUN apt-get update && apt-get install -y \ 
    libxml2-dev libpng-dev libpq-dev libcurl4-openssl-dev \
    libmcrypt-dev gnupg2 git unzip && \
    docker-php-ext-install xml && \
    docker-php-ext-install gd && \
    docker-php-ext-install pdo_pgsql && \
    docker-php-ext-install curl && \
    docker-php-ext-install zip && \
    docker-php-ext-install soap && \
    docker-php-ext-install mcrypt && \
    curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/local/bin --filename=composer && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | \
    tee /etc/apt/sources.list.d/yarn.list && apt-get update && \
    apt-get install -y yarn && \
    a2enmod rewrite && service apache2 restart

# Parámetros de la imagen
ARG APP_ENVIRONMENT
ARG PROYECTO_DIR

# Usamos la configuración por defecto que trae la imagen
RUN if [ $APP_ENVIRONMENT = "prod" ]; then export DEFAULT_PHP_INI_ENV='production'; else export DEFAULT_PHP_INI_ENV='development'; fi && mv "$PHP_INI_DIR/php.ini-${DEFAULT_PHP_INI_ENV}" "$PHP_INI_DIR/php.ini"

# Copiamos el código fuente
WORKDIR $PROYECTO_DIR
ADD bin $PROYECTO_DIR/bin
ADD deploy $PROYECTO_DIR/deploy
ADD metadatos $PROYECTO_DIR/metadatos
ADD personalizacion $PROYECTO_DIR/personalizacion
ADD php $PROYECTO_DIR/php
ADD sql $PROYECTO_DIR/sql
ADD src $PROYECTO_DIR/src
ADD temp $PROYECTO_DIR/temp
ADD www $PROYECTO_DIR/www
ADD menu.ini $PROYECTO_DIR
ADD arai.json $PROYECTO_DIR
ADD composer.json $PROYECTO_DIR
ADD composer.lock $PROYECTO_DIR
ADD proyecto.ini $PROYECTO_DIR

# Instalamos las dependencias
RUN composer install --no-dev && chmod +x bin/instalador

# Para tener color en la terminal
ENV TERM xterm-256color

# Hacemos coincidir el UID de www-data con el del host para evitar
# problemas de propiedad con el bind
ARG UID
RUN usermod -u $UID www-data && groupmod -g $UID www-data && service apache2 restart
