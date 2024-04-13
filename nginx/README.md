# Nginx

## Install

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04

## Reverse Proxy

> remember to use FQDN for your configuration file, then .conf extension, for example mydomain.com.conf.

```nginx
server {
    listen 80;

    server_name mydomain.com;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

then

```sh
ln -s /etc/nginx/sites-available/mydomain.com.conf /etc/nginx/sites-enabled/
service nginx reload
```
