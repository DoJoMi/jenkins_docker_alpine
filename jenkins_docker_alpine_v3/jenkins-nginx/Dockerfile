FROM nginx:alpine

MAINTAINER dojomi

ARG CERTIFICATE_DIR=/etc/nginx/ssl
ARG COUNTRY_NAME=
ARG STATE=
ARG CITY= 
ARG ORGANIZATION_NAME= 
ARG COMMON_NAME=jenkins.local.io 
ARG CERT_NAME_OUTPUT=jenkins.local.io 

RUN apk update
RUN apk --no-cache add --update openssl \
    && rm -rf /var/cache/apk/*

RUN rm -f /etc/nginx/conf.d/default.conf
RUN rm -f /etc/nginx/conf.d/example_ssl.conf

COPY conf/jenkins.conf /etc/nginx/conf.d/default.conf
COPY conf/nginx.conf /etc/nginx/nginx.conf

RUN mkdir -p ${CERTIFICATE_DIR} && \
    /usr/bin/openssl genrsa -out ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.key 1024 && \
    /usr/bin/openssl req -new -newkey rsa:4096 -days 365 -nodes -subj "/C=${COUNTRY_NAME}/ST=${STATE}/L=${CITY}/O=${ORGANIZATION_NAME}/CN=${COMMON_NAME}" -keyout ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.key -out ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.csr && \
    /usr/bin/openssl x509 -req -days 365 -in ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.csr -signkey ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.key -out ${CERTIFICATE_DIR}/${CERT_NAME_OUTPUT}.crt


VOLUME /var/log/nginx/

EXPOSE 443

CMD ["nginx"]
