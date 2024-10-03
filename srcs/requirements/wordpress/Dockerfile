FROM debian:oldstable

ARG WORDPRESS_DATABASE=wordpress
ARG WORDPRESS_USER=wordpress
ARG WORDPRESS_PASSWORD=wordpress

ENV WORDPRESS_DATABASE=${WORDPRESS_DATABASE}
ENV WORDPRESS_USER=${WORDPRESS_USER}
ENV WORDPRESS_PASSWORD=${WORDPRESS_PASSWORD}

WORKDIR /var/www/html/

RUN apt update -y \
    && apt upgrade -y \
    && apt install -y php7.4 php-fpm php-mysql mariadb-client wget

RUN wget -O /usr/local/bin/wp https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x /usr/local/bin/wp \
    && wp core download --allow-root --path=/var/www/html

RUN mkdir /run/php

RUN chown -R www-data:www-data /var/www/html \
    && chmod 755 /var/www/html

COPY conf/www.conf /etc/php/7.4/fpm/pool.d/www.conf
COPY tools/script.sh /tmp/wp_setup.sh

RUN chmod +x /tmp/wp_setup.sh

ENTRYPOINT ["/tmp/wp_setup.sh"]
