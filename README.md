# Nginx Gateway

nginx-gateway is a ready-to-use docker-compose project which relies on
[`nginx-proxy`](https://github.com/nginx-proxy/nginx-proxy) to act
an HTTP/HTTPs gateway for you server, routing the requests to specific docker
containers based on the domain.

It also generate/renew your Let's Encrypt certificates using
[`docker-letsencrypt-nginx-proxy-companion`](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion). 

Read the full documention on: https://github.com/nginx-proxy/nginx-proxy

Usage example:

Adds to your app `docker-compose.yaml`:
```
version: "3"

services:
  my-app:
    image: awesome-app-image
    environment:
      VIRTUAL_HOST: example.com
      VIRTUAL_HOST_ALIAS: alternative.example.com,www.example.com
      LETSENCRYPT_HOST: example.com,alternative.example.com,www.example.com
      LETSENCRYPT_EMAIL: email#example.com

networks:
  default:
    external:
      name: nginx-gateway
```


### Virtual Host Aliases

Based on https://github.com/nginx-proxy/nginx-proxy/pull/1369 this project
supports creating domain alias which redirect users to the main domain.

You can add aliases that will redirect (301) to the first entry in `VIRTUAL_HOST`
by adding the `VIRTUAL_HOST_ALIAS` env var:
    $ docker run -e VIRTUAL_HOST=example.com -e VIRTUAL_HOST_ALIAS=www.example.com,old.example.com ...
This will setup the following redirects:
- `http://www.example.com` &#8594; `http://example.com`
- `http://old.example.com` &#8594; `http://example.com`

Remeber to add all the aliases to the `LETSENCRYPT_HOST` to generate their certificates.
