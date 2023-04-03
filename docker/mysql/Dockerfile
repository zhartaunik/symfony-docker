FROM mysql:latest

ENV MYSQL_ROOT_PASSWORD=secret

COPY docker/mysql/init/init.sql /docker-entrypoint-initdb.d/
