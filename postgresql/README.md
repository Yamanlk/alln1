# PostgreSQL

## PSQL Only Docker Container

```sh
docker run --rm -it --add-host db:host-gateway alpine:3.19 sh
```

then inside the container

```sh
apk --no-cache add postgresql15-client

psql -h db -U user -d database
```

## Prisma

```sh
sudo -i -u postgres
```

once you logged in postgres user run

```sh
createuser myuser --pwprompt
createdb mydb --owner=viralme
```

## Connecting From Docker Container To Host

Add the following line to `/etc/postgresql/15/main/postgresql.conf`

> docker uses 172.17.0.0/16 range by default, and host-gateway is assigned 172.17.0.1, unless changed in docker deamon settings.

```text
listen_addresses = 'localhost,172.17.0.1'
```

Add the following line to `/etc/postgresql/15/main/pg_hba.conf`

> This tells postgres to accept connections from any docker containers IP address range, only for `myuser` role and `mydb` database.

```text
host    myuser    mydb    172.17.0.0/16    scram-sha-256
```

> This only works with `docker run` since it uses the default network, to get this to work with docker compose you need to set `network_mode` to `bridge` to tell docker compose not to create its own network, or you can create a different network and set ipam rules correctly to match the configuration in `pg_hba.conf` file, remeber 172.17.0.0/16 is taken so use something else in that case.

Now restart the database service

```sh
sudo service postgresql restart
```

If you have UFW enabled you might need to add the following rule

```sh
ufw allow proto tcp from 172.17.0.0/16
```

> remember to use the correct subnet, in the example 172.17.0.0 is used which is the default subnet used by docker, but in case you're using a custom network with ipam settings, you need to set the correct subnet.
