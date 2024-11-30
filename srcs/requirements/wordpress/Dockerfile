FROM debian:oldstable

ARG DB_NAME
ARG DB_USER
ARG DB_PASSWORD
ARG WP_ROOT_USER
ARG WP_ROOT_EMAIL
ARG WP_ROOT_PASSWORD
ARG WP_USER
ARG WP_EMAIL
ARG WP_PASSWORD

ENV DB_NAME=${DB_NAME}
ENV DB_USER=${DB_USER}
ENV DB_PASSWORD=${DB_PASSWORD}
ENV WP_ROOT_USER=${WP_ROOT_USER}
ENV WP_ROOT_EMAIL=${WP_ROOT_EMAIL}
ENV WP_ROOT_PASSWORD=${WP_ROOT_PASSWORD}
ENV WP_USER=${WP_USER}
ENV WP_EMAIL=${WP_EMAIL}
ENV WP_PASSWORD=${WP_PASSWORD}
ENV PHP_VERSION=7.4
ENV WORDPRESS_PATH=/var/www/wordpress

RUN apt update \
    && apt install -y wget lsb-release \
    && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
    && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list \
    && apt update \
    && apt install -y php${PHP_VERSION} php${PHP_VERSION}-fpm php${PHP_VERSION}-mysql mariadb-client \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && wget https://wordpress.org/latest.tar.gz -P /var/www \
    && cd /var/www && tar -xvf latest.tar.gz && rm latest.tar.gz \
    && chown -R www-data:www-data ${WORDPRESS_PATH}

COPY ./conf/www.conf /etc/php/${PHP_VERSION}/fpm/pool.d/www.conf
COPY ./conf/wp-config.php ${WORDPRESS_PATH}/wp-config.php
COPY ./tool/wp_setup.sh /wp_setup.sh

RUN wget https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar && mv wp-cli.phar /usr/local/bin/wp \
    && chmod +x /wp_setup.sh \
    && mkdir /run/php \
    && chown -R www-data:www-data /run/php /var/log \
    && ln -s $(find /usr/sbin -name 'php-fpm*') /usr/bin/php-fpm

USER www-data

EXPOSE 9000

CMD ["/wp_setup.sh"]
