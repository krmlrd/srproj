FROM nginx:alpine

RUN adduser -u 1000 -D -S -G www-data www-data

WORKDIR /var/www

COPY nginx.conf /etc/nginx/
COPY sites/default.conf /etc/nginx/conf.d/

CMD ["nginx"]

EXPOSE 80 443