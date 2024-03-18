## IISSDOCKER
En este repositorio se subiran los resultados de la práctica de DOCKER de la asignatura IISS.

## Introducción

A continuación se explicaran los dos Docker compose pedidos a configurar.

# Parte 1: Docker compose para Drupal y Mysql

## Contenido de docker-compose.yml

```yaml
version: '3'
services:
  drupal:
    image: drupal:latest
    ports:
      - "81:80"
    volumes:
      - drupaldata:/var/www/html
    depends_on:
      - mysql
  mysql:
    image: mysql:latest
    environment:
      MYSQL_DATABASE: drupal
      MYSQL_USER: drupal
      MYSQL_PASSWORD: drupal
    volumes:
      - mysqldata:/var/lib/mysql
volumes:
  drupaldata:
  mysqldata:
```

## Explicación de la configuración de Drupal

- `image: drupal:latest`: Utiliza la última versión de Drupal disponible en Docker Hub.
- `ports`: Mapea el puerto 81 del host al puerto 80 del contenedor, permitiendo acceder a Drupal mediante `localhost:81`.
- `volumes`: Monta un volumen llamado `drupaldata` en `/var/www/html` () dentro del contenedor para persistir los datos de Drupal.
- `depends_on`: Asegura que el servicio `mysql` esté iniciado antes de iniciar `drupal`.

## Configuración de la base de datos MySQL

- `image: mysql:latest`: Utiliza la última versión de MySQL disponible en Docker Hub.
- `environment`: Define las variables de entorno utilizadas por MySQL para configurar la base de datos inicial, incluyendo el nombre de la base de datos (`drupal`), el usuario (`drupal`) y la contraseña (`drupal`).
- `volumes`: Monta un volumen llamado `mysqldata` en `/var/lib/mysql` dentro del contenedor para persistir los datos de MySQL.

## Definición de Volúmenes

- Aquí definimos los volúmenes que serán utilizados por nuestros servicios. `drupaldata` para el almacenamiento persistente de datos de Drupal y `mysqldata` para los datos de MySQL. Esto asegura que los datos permanezcan intactos incluso después de detener o reiniciar los contenedores.

## Manual de uso

1. `git clone https://github.com/PabloAC04/IISSDOCKER.git`: clonamos el repositorio en nuestra máquina para usarlo.
2. `cd IISSDOCKER/Parte1/`: nos movemos a la carpeta donde se encuentra nuestro `docker-compose.yml`.
3. `docker compose up -d`: ejecutamos el docker-compose en background.
4. En nuestro navegador vamos la dirección localhost:81 y accederemos a drupal.
5. Por último para terminar la ejecución, `docker compose down`.


# Parte 2: Docker compose para Wordpress y MariaDB

## Contenido de docker-compose.yml

```yaml
version: '3'

services:
  wordpress:
    image: wordpress
    ports:
      - "82:80"
    environment:
      WORDPRESS_DB_HOST: mariadb:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - mariadb
    networks: 
      - redDocker
  mariadb:
    image: mariadb
    environment:
      MARIADB_DATABASE: wordpress
      MARIADB_USER: wordpress
      MARIADB_PASSWORD: wordpress
    networks:
      - redDocker

networks:
  redDocker:
```

## Configuración del Servicio WordPress

- `image: wordpress`: Denota el uso de la imagen oficial de WordPress disponible en Docker Hub.
- `ports: - "82:80"`: Mapea el puerto 82 del host al puerto 80 del contenedor, lo que permite el acceso a WordPress a través de `localhost:82`.
- `environment`: Configura WordPress para conectarse a la base de datos de MariaDB mediante variables de entorno, incluyendo el host de la base de datos (`mariadb:3306`), el usuario (`wordpress`), la contraseña (`wordpress`) y el nombre de la base de datos (`wordpress`).
- `depends_on: - mariadb`: Garantiza que el servicio `mariadb` se inicie antes que `wordpress`.
- `networks: - redDocker`: Conecta el servicio `wordpress` a una red llamada `redDocker`, que es compartida con el servicio `mariadb`, facilitando la comunicación entre ambos.

## Configuración del Servicio MariaDB

- `image: mariadb`: Utiliza la imagen oficial de MariaDB de Docker Hub.
- `environment`: Establece la base de datos inicial, el usuario y la contraseña que WordPress utilizará para conectarse a MariaDB, promoviendo una configuración coherente y segura.
- `networks: - redDocker`: Asegura que `mariadb` se comunique en la misma red `redDocker` que `wordpress`, permitiendo una conexión directa y privada entre el servicio de la aplicación y la base de datos.

## Definición de la Red Docker

- `networks: redDocker:`: Define una red personalizada llamada `redDocker` que es utilizada por los servicios `wordpress` y `mariadb` para facilitar su comunicación. Esta red asegura que el tráfico entre estos servicios se mantenga aislado de otros contenedores y servicios en el host.

## Manual de uso

En la terminal de linux:

1. `git clone https://github.com/PabloAC04/IISSDOCKER.git`: clonamos el repositorio en nuestra máquina para usarlo.
2. `cd IISSDOCKER/Parte2/`: nos movemos a la carpeta donde se encuentra nuestro `docker-compose.yml`.
3. `docker compose up -d`: ejecutamos el docker-compose en background.
4. En nuestro navegador vamos la dirección localhost:82 y accederemos a wordpress.
5. Por último para terminar la ejecución, `docker compose down`.
