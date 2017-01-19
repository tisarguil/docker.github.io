---
datafolder: engine-cli
datafile: docker_service_scale
title: docker service scale
---

<!--
Sorry, but the contents of this page are automatically generated from
Docker's source code. If you want to suggest a change to the text that appears
here, you'll need to find the string by searching this repo:

https://www.github.com/docker/docker
-->

{% include cli.md %}

## Examples

### Scale a service

The scale command enables you to scale one or more replicated services either up
or down to the desired number of replicas. This command cannot be applied on
services which are global mode. The command will return immediately, but the
actual scaling of the service may take some time. To stop all replicas of a
service while keeping the service active in the swarm you can set the scale to 0.

For example, the following command scales the "frontend" service to 50 tasks.

```bash
$ docker service scale frontend=50
frontend scaled to 50
```

The following command tries to scale a global service to 10 tasks and returns an error.

```
$ docker service create --mode global --name backend backend:latest
b4g08uwuairexjub6ome6usqh
$ docker service scale backend=10
backend: scale can only be used with replicated mode
```

Directly afterwards, run `docker service ls`, to see the actual number of
replicas.

```bash
$ docker service ls --filter name=frontend

ID            NAME      MODE        REPLICAS  IMAGE
3pr5mlvu3fh9  frontend  replicated  15/50     nginx:alpine
```

You can also scale a service using the [`docker service update`](service_update.md)
command. The following commands are equivalent:

```bash
$ docker service scale frontend=50
$ docker service update --replicas=50 frontend
```

### Scale multiple services

The `docker service scale` command allows you to set the desired number of
tasks for multiple services at once. The following example scales both the
backend and frontend services:

```bash
$ docker service scale backend=3 frontend=5
backend scaled to 3
frontend scaled to 5

$ docker service ls
ID            NAME      MODE        REPLICAS  IMAGE
3pr5mlvu3fh9  frontend  replicated  5/5       nginx:alpine
74nzcxxjv6fq  backend   replicated  3/3       redis:3.0.6
```