FROM debian:oldstable

ARG DOMAIN_NAME

ENV DOMAIN_NAME=${DOMAIN_NAME}

RUN apt update -y && apt upgrade -y \
    && apt install nginx openssl gettext -y \
    && mkdir -p /run/nginx /etc/nginx/ssl /usr/share/nginx/html \
    && openssl req \
    -x509 \
    -sha256 \
    -nodes \
    -newkey rsa:2048 \
    -days 365 \
    -keyout /etc/nginx/ssl/private.key \
    -out /etc/nginx/ssl/certificate.crt \
    -subj "/C=BR/ST=SP/L=Sao Paulo/O=42sp/CN=${DOMAIN_NAME}/streetAddress=R. Aspicuelta, 422 - Alto de Pinheiros/postalCode=05433-010" \
    && chown -R www-data:www-data /etc/nginx/ssl \
    && chmod 755 /etc/nginx/ssl/private.key

COPY conf/nginx.conf.template /etc/nginx/nginx.conf.template
RUN envsubst '$DOMAIN_NAME' < /etc/nginx/nginx.conf.template > /etc/nginx/nginx.conf

EXPOSE 443

ENTRYPOINT ["nginx", "-g", "daemon off;"]
