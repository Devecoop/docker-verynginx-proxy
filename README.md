# docker-verynginx-proxy

docker-verynginx-proxy sets up a docker compose with nginx, verynginx dashboard and docker-gen. docker-gen generates reverse proxy configs for nginx and reloads nginx when containers are started and stopped.

This repository is a merge with two tools:
- [verynginx](https://github.com/alexazhou/VeryNginx) from [camil/verynginx](https://hub.docker.com/r/camil/verynginx/) docker image.
- [docker-gen](https://github.com/jwilder/docker-gen) from [jwilder/nginx-proxy](https://hub.docker.com/r/jwilder/docker-gen/) docker image.

## Information

#### Two containers:
- [camil/verynginx](https://hub.docker.com/r/camil/verynginx/)
    - name: nginx
    - port: 80
- [jwilder/docker-gen](https://hub.docker.com/r/jwilder/docker-gen/)
    - name: dockergen

## Usage

#### Pull images
```bash
docker-compose pull
```

#### Run containers
```bash
docker-compose up -d
```

#### View verynginx dashboard
```url
http://verynginx/verynginx/index.html

user: verynginx
pass: verynginx
```

#### Multiple Hosts

If you need to support multiple virtual hosts for a container, you can separate each entry with commas.  For example, `foo.bar.com,baz.bar.com,bar.com` and each host will be setup the same.

#### Wildcard Hosts

You can also use wildcards at the beginning and the end of host name, like `*.bar.com` or `foo.bar.*`. Or even a regular expression, which can be very useful in conjunction with a wildcard DNS service like [xip.io](http://xip.io), using `~^foo\.bar\..*\.xip\.io` will match `foo.bar.127.0.0.1.xip.io`, `foo.bar.10.0.2.2.xip.io` and all other given IPs. More information about this topic can be found in the nginx documentation about [`server_names`](http://nginx.org/en/docs/http/server_names.html).

#### Multiple Networks

With the addition of overlay networking in Docker 1.9, your nginx-proxy container may need to connect to backend containers on multiple networks. By default, if you don't pass the --net flag when your nginx-proxy container is created, it will only be attached to the default bridge network. This means that it will not be able to connect to containers on networks other than bridge.

If you want your nginx-proxy container to be attached to a different network, you must pass the --net=my-network option in your docker create or docker run command. At the time of this writing, only a single network can be specified at container creation time. To attach to other networks, you can use the docker network connect command after your container is created:

###### Add new network
```bash
$ docker network connect my-other-network dockergen
$ docker network connect my-other-network nginx
```

#### SSL Backends

If you would like to connect to your backend using HTTPS instead of HTTP, set `VIRTUAL_PROTO=https` on the backend container.

#### uWSGI Backends

If you would like to connect to uWSGI backend, set `VIRTUAL_PROTO=uwsgi` on the
backend container. Your backend container should than listen on a port rather
than a socket and expose that port.

#### Default Host

To set the default host for nginx use the env var `DEFAULT_HOST=foo.bar.com` for example

#### More settings
Check:
- [nginx-proxy](https://github.com/jwilder/nginx-proxy)
- [docker-gen](https://github.com/jwilder/docker-gen)
