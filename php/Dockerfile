FROM php:7.4-fpm-alpine

COPY  ./app /usr/share/nginx/html/
# Set working directory
WORKDIR /usr/share/nginx/html/

RUN docker-php-source extract \
    # do important things \
    && docker-php-source delete

ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/
#Extensiones
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd xdebug http bz2 mcrypt mysqli pdo_mysql zip xml
# Install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '795f976fe0ebd8b75f26a6dd68f78fd3453ce79f32ecb33e7fd087d39bfeb978342fb73ac986cd4f54edd0dc902601dc') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

# Copy existing application directory contents
COPY ./app /usr/share/nginx/html/

# Copy existing application directory permissions
COPY --chown=www-data:www-data ./app /usr/share/nginx/html/

# Change current user to www
USER www-data

# Expose port 9000 and start php-fpm server
EXPOSE 9000
