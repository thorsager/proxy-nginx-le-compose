NGINX Proxy & LetsEncrypt
=========================
This simple compose file is just an example of how you could put some of
great stuff people have made to some use.

Please note I have *NOT* made any of this, I have just created the compose
file.

The compose-file starts up a couple of containers, and interact with 
docker itself, to create automated revers-proxy for any docker container
that wants it, and if so wanted creates a TLS cert and smack that in the
hat as well.

Starting out
------------
Create a network which will allow you proxy to communicate with all your
other containers that need proxying. (the below name `proxy-net` is what is
used in the `docker-compose.yml`, you could choose your own!)
```
$ docker network create proxy-net
```
When this has been created, your start up the stack.
```
$ docker-compose up -d
```
You are now ready to create you container that needs proxying..

### Using a compose file:
```
version: '2'

networks:
  nginx-proxy:
    external:
      name: proxy-net

services:
  whoami-ssl:
    image: jwilder/whoami
    networks:
      - nginx-proxy
    environment:
      - VIRTUAL_HOST=whoami.mydomain.tld
      - LETSENCRYPT_HOST=whoami.domain.tld
```
### Or simply running it:
```
docker run -it --network proxy-net \
 -e VIRTUAL_HOST=w.mydomain.tld \
 -e LETSENCRYPT_HOST=w.mydomain.tld \
 jwilder/whoami
```

Original work by:
-----
 - https://github.com/jwilder
   - https://github.com/jwilder/nginx-proxy
   - https://github.com/jwilder/docker-gen

 - https://github.com/JrCs
   - https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion

 - https://www.nginx.com/
 - https://letsencrypt.org/  

