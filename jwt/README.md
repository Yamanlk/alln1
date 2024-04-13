# JWT

## Secret Key

https://datatracker.ietf.org/doc/html/rfc7518#section-3.2

```sh
# generate 256 bit length key, you can increase the key length as you like.
node -e "require('crypto').randomBytes(32, (err, buffer) => { console.log(buffer.toString('hex')) });"
```
