FROM php:7.1-apache-buster

# Instalamos las extensiones de php, composer, yarn
RUN apt-get update && apt-get install -y \ 
    libxml2-dev libpng-dev libpq-dev libcurl4-openssl-dev \
    libmcrypt-dev gnupg2  git && \
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
    apt-get install -y yarn

# Parámetros de la imagen
ARG APP_ENVIRONMENT
ARG APP_DIRECTORY

# Usamos la configuración por defecto que trae la imagen
RUN if [ $APP_ENVIRONMENT = "PROD" ]; then export DEFAULT_PHP_INI_ENV='production'; else export DEFAULT_PHP_INI_ENV='development'; fi && mv "$PHP_INI_DIR/php.ini-${DEFAULT_PHP_INI_ENV}" "$PHP_INI_DIR/php.ini"

# Copiamos el código fuente
WORKDIR $APP_DIRECTORY
COPY . . 
RUN composer install --no-dev
RUN chmod +x bin/instalador
COPY instalador.env .
RUN ./bin/instalador proyecto:instalar --crear-db

