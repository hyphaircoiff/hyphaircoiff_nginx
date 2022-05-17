# What for?
- [jwilder/nginx-proxy](https://hub.docker.com/r/jwilder/nginx-proxy) is used to automatically create a nginx reverse proxy for all containers with a `VIRTUAL_HOST` environment variable defined.
- [jrcs/letsencrypt-nginx-proxy-companion](https://hub.docker.com/r/jrcs/letsencrypt-nginx-proxy-companion) is used to automatically generate ssl certificate for all containers with a `LETSENCRYPT_HOST` defined.

# Usage
## Prerequise
Assuming that have :
- VPS with an ip address like `10.110.120.130`
- One or multiple domain and/or subdomain like `example.com`
- A configured domain redirection with `A` type like `A - 10.110.120.130 -> example.com`
- A webapp or service that we want to use `example.com` domain like `wordpress`

## Configuration
On VPS, create related network, all services with a `VIRTUAL_HOST` must be connected to this network

```shell
docker network create nginx-proxy
```

If we take for example that we have a wordpress in a docker-compose like that:
(example from official samples https://docs.docker.com/samples/wordpress)
```diff
version: "3.9"
    
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
+      VIRTUAL_HOST: ${DOMAIN}
+      LETSENCRYPT_HOST: ${DOMAIN}
volumes:
  db_data: {}
  wordpress_data: {}

+networks:
+  default:
+    external:
+      name: nginx-proxy
```

***note: if your service doesn't work on port 80, you can define a `VIRTUAL_PORT` variable with right service port***

After updated `docker-compose.yml` run

```shell
docker-compose stop wordpress
docker-compose build wordpress --no-cache
docker-compose up -d
```

## Setup
On VPS, in apps or webservice or another folder
```shell
git clone git@github.com:Eth3rnit3/configured-compose-nginx-proxy.git
cd configured-compose-nginx-proxy
docker-compose up -d
```

Et voilà! You can log `nginx-proxy` and `nginx-proxy-letsencrypt` for debug if need. On the first time, `letsencrypt` generate ssl certificate, after a quick initialization for that, you can access to your webservice on https://example.com