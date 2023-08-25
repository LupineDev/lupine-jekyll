---
layout: post
title: "Docker: Error response from daemon volume is in use"
categories:
  - software development
tags:
  - docker
  - error
  - docker compoes
---
It's been almost 10 years since my last post... Here is a short one I've been meaning to write for a while.

Have you ever encountered an error like this when running `docker compose down -v`
<br/>
(formerly `docker-compose down -v`)?


```
Docker: Error response from daemon volume is in use
```
<br/>

It can happen when you have a container still running 
This is something I have seen often enough to prompt writing this blog post.  Then next thing I usually try is `docker compose ps`
to see if any containers are still running.

If you have stopped all containters and it is still complaining the next thing to try is to remove all stopped containers
with this command thanks to ([LinPy on stackoverflow](https://stackoverflow.com/a/56865672))

```bash
docker rm -f $(docker ps -a -q)
```
<br/>

You can also use the [docker system prune](https://docs.docker.com/engine/reference/commandline/system_prune/) command. It will remove all unused:
- Containers
- Networkds
- Images
- Volumes

This command is also useful to run periodically, especially after tinkering with Dockerfiles as there are bound to be a few vestigial image layers
hanging around.