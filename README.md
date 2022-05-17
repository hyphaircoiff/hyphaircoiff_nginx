# What for?
- [jwilder/nginx-proxy](https://hub.docker.com/r/jwilder/nginx-proxy) is used to automatically create a nginx reverse proxy for all containers with a `VIRTUAL_HOST` environment variable defined.
- [jrcs/letsencrypt-nginx-proxy-companion](https://hub.docker.com/r/jrcs/letsencrypt-nginx-proxy-companion) is used to automatically generate ssl certificate for all containers with a `LETSENCRYPT_HOST` defined.

# Usage
## Prerequise
Assuming that have :
- VPS with an ip address like `10.110.120.130`
- One or multiple domain and/or subdomain like `example.com`
- A configured domain redirection with `A` type like `A - 10.110.120.130 -> example.com`

## Setup
On VPS, in apps or webservice or another folder
```shell
```