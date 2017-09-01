FROM nginx:alpine

MAINTAINER dojomi

ENV DOCKERIZE_VERSION v0.5.0
# variable which is handed over to the conf/nginx.conf.tmpl
ENV WORKER_CONNECTIONS="1024"
ENV WORKER_PROCESSES="2"

# variable which is handed over to the conf/default.conf.tmpl
ENV CERT_NAME="jenkins.local.io"

ARG CERTIFICATE_DIR=/etc/nginx/ssl
ARG COUNTRY_NAME=
ARG STATE=
ARG CITY= 
ARG ORGANIZATION_NAME= 
ARG COMMON_NAME=jenkins.local.io 
ARG CERT_NAME_OUTPUT=jenkins.local.io

RUN apk update
RUN apk --no-cache add --update openssl \
    && wget -O /tmp/dockerize.tar.gz https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-linux-amd64-${DOCKERIZE_VERSION}.tar.gz \
    && tar -C /usr/local/bin -xzvf /tmp/dockerize.tar.gz \
    && rm -rf /tmp/dockerize.tar.gz \
    && rm -rf /var/cache/apk/*

RUN rm -f /etc/nginx/conf.d/default.conf
RUN rm -f /etc/nginx/conf.d/example_ssl.conf

RUN mkdir -p ${CERTIFICATE_DIR} && \
    /usr/bin/openssl genrsa -out ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.key 1024 && \
    /usr/bin/openssl req -new -newkey rsa:4096 -days 365 -nodes -subj "/C=${COUNTRY_NAME}/ST=${STATE}/L=${CITY}/O=${ORGANIZATION_NAME}/CN=${COMMON_NAME}" -keyout ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.key -out ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.csr && \
    /usr/bin/openssl x509 -req -days 365 -in ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.csr -signkey ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.key -out ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.crt

VOLUME ["/var/cache/nginx"]

EXPOSE 443

COPY conf/nginx.conf.tmpl /etc/nginx/nginx.conf.tmpl
COPY conf/default.conf.tmpl /etc/nginx/conf.d/default.conf.tmpl

CMD dockerize -template /etc/nginx/nginx.conf.tmpl:/etc/nginx/nginx.conf -template /etc/nginx/conf.d/default.conf.tmpl:/etc/nginx/conf.d/default.conf -stdout /var/log/nginx/access.log -stderr /var/log/nginx/error.log /usr/sbin/nginx
