FROM nginx:latest

COPY ./conf/nginx.conf /etc/nginx/nginx.conf
COPY ./conf/default.conf /etc/nginx/conf.d/default.conf

WORKDIR /var/www/html/public

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
