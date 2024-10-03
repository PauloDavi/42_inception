FROM alpine:3.19.2

ARG DOMAIN_NAME=pdavi-al.42.fr
ENV DOMAIN_NAME=${DOMAIN_NAME}

RUN apk update \
    && apk add --no-cache nginx openssl \
    && mkdir -p /run/nginx /etc/nginx/ssl /usr/share/nginx/html \
    && openssl req \
        -x509 \
        -sha256 \
        -nodes \
        -newkey rsa:2048 \
        -days 365 \
        -keyout /etc/nginx/ssl/private.key \
        -out /etc/nginx/ssl/certificate.crt \
        -subj "/C=BR/ST=SP/L=Sao Paulo/O=42sp/CN=${DOMAIN_NAME}/streetAddress=R. Aspicuelta, 422 - Alto de Pinheiros/postalCode=05433-010"

COPY conf/nginx.conf /etc/nginx/nginx.conf

EXPOSE 443

ENTRYPOINT ["nginx", "-g", "daemon off;"]
