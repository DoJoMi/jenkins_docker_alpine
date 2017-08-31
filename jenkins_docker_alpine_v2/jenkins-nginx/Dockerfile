FROM nginx:alpine

MAINTAINER dojomi

RUN apk update
RUN apk --no-cache add --update \
    && rm -rf /var/cache/apk/*

RUN rm -f /etc/nginx/conf.d/default.conf
RUN rm -f /etc/nginx/conf.d/example_ssl.conf

COPY conf/jenkins.conf /etc/nginx/conf.d/default.conf
COPY conf/nginx.conf /etc/nginx/nginx.conf

VOLUME /var/log/nginx/

EXPOSE 443

CMD ["nginx"]
