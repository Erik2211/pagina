# lamp con docker compose

Primero tendremos una carpeta **db** con la base de datos y otra carpeta **www** con la pagina de publicacion.

## Fichero docker-compose
```
version: "3.1"
services:
      db:
         image: mysql
         ports:
              - "3306:3306"
        command: --default-authentication-plugin=myql_native_password
        environment:
            MYSQL_DATABASE: nombre
            MYSQL_ROOT_PASSWORD: pass
            MYSQL_USER: user
            MYSQL_PASSWORD: pass
        volumes:
            - ./db:/docker-entrypoint-initdb.d
            - ./conf:/etc/mysql/conf.d
            - ./persistent:/var/lib/mysql
      web:
        build: .
        ports:
            - "80:80"
        volumes:
            - ./www:/var/www/html
        links:
            - db
      phpmyadmin:
        image: phpmyadmin/phpmyadmin
        links:
            - db:db
        ports:
            - 8000:80
        environment:
            MYSQL_USER: user
            MYSQL_PASSWORD: pass
            MYSQL_ROOT_PASSWORD: pass
        ports:
            - 8080:8080
volumes:
    persistent:
```
## dockerfile
```
FROM php:8.0.0-apache
RUN docker-php-ext-install mysqli
RUN apt update \
    && rm -rf /var/lib/apt/lists/*
```
