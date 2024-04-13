# SSL

## Postman

If you are using postman remember to use https protocol after you installed the certificate.
When postman sends a request using http protocol and gets a 301 response (auto redirect to https)
it will send the request to the new url but with GET method always,
so if you send a POST request it will turn into GET.

## Free Let's Encrypt SSL

https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-22-04#step-1-installing-certbot
