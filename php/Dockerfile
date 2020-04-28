FROM php:7.4-fpm-alpine

RUN mkdir -p /var/www/
WORKDIR /var/www

RUN apk add --no-cache \
        aspell \
        autoconf \
        automake \
        bzip2-dev \
        curl \
        cyrus-sasl-dev \
        freetype-dev \
        ghostscript \
        graphviz \
        g++ \
        libjpeg-turbo-dev \
        libmcrypt-dev \
        libmemcached-dev \
        libpng-dev \
        libreoffice-common \
        libreoffice-writer \
        libzip-dev \
        make \
        python3 \
    && ln -s /usr/bin/python3 /usr/bin/python \
    && curl -Ls https://raw.githubusercontent.com/dagwieers/unoconv/0.7/unoconv -o /usr/local/bin/unoconv \
    && chmod +x /usr/local/bin/unoconv

RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install bcmath bz2 gd mysqli opcache pcntl pdo_mysql zip \
    && pecl install memcached redis \
    && docker-php-ext-enable memcached redis

RUN curl -sS https://getcomposer.org/installer | php \ 
    && mv composer.phar /usr/local/bin/composer \
    && composer global require hirak/prestissimo --no-plugins --no-scripts

RUN cd /tmp \
	&& curl -Ls https://moodle.org/plugins/download.php/21420/moosh_moodle38_2020042300.zip -o moosh.zip \
	&& unzip moosh.zip \
	&& rm -f moosh.zip \
	&& mv moosh /usr/local/share/moosh \
	&& ln -s /usr/local/share/moosh/moosh.php /usr/local/bin/moosh

COPY moodle/composer.json moodle/composer.lock /var/www/public/
RUN cd public \
    && composer install \
        --prefer-dist \
        --no-autoloader \
        --no-scripts \
        --no-suggest \
        --no-interaction \
    && composer clear-cache

COPY moodle /var/www/public

RUN rmdir /var/www/html \
    && ln -sf /var/www/public /var/www/html

RUN cd public \
    && composer dump-autoload

RUN mkdir /mnt/moodle \
    && mkdir /mnt/moodle/moodledata \
    && chown -R www-data:www-data /mnt/moodle

RUN chown -R www-data:www-data /var/www/ \
    && find /var/www -type d -exec chmod 700 {} \; \
    && find /var/www -type f -exec chmod 600 {} \; \
    && chmod 400 /var/www/public/config-dist.php \
    && chmod 500 /var/www/public/vendor/bin/*
