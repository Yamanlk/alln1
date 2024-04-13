# GitLab Container Registry

## Authenticate With Container Registry

https://docs.gitlab.com/ee/user/packages/container_registry/authenticate_with_container_registry.html

## Container => Host Connection

```yaml
# add the following to allow the container to connect connect to the local machine
extra_hosts:
  - "host:host-gateway"
```

Now you can use `host` hostname to connect to the host machine, usually this resolves to 172.11.0.1, but this may differ depending on your docker configuration.

> You can also choose hostnames other that `host` like `db`, `my.docker.host` or any FQDN.s