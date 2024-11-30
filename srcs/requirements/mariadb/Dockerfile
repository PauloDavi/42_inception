FROM debian:oldstable

ARG DB_NAME
ARG DB_USER
ARG DB_PASSWORD

ENV DB_NAME=${DB_NAME}
ENV DB_USER=${DB_USER}
ENV DB_PASSWORD=${DB_PASSWORD}

RUN apt-get update \
    && apt-get install -y mariadb-server \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY ./conf/my.cnf /etc/mysql/my.cnf
COPY ./tool/db_init.sh /usr/local/bin/db_init.sh

RUN mkdir -p /var/run/mysqld /run/mysqld \
    && chown -R mysql:mysql /var/run/mysqld /run/mysqld \
    && chmod 755 /var/run/mysqld /run/mysqld \
    && chmod +x /usr/local/bin/db_init.sh \
    && /usr/local/bin/db_init.sh

USER mysql

CMD ["mysqld_safe"]
