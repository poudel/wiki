+++
title = "Docker"
weight = 30
+++

# Notes

## Port mapping syntax

Keep forgetting this.

```
${HOST_PORT}:${CONTAINER_PORT}
```

i.e.

```
8080:8000
```

here, 8080 is the port on the host machine and 8000 is on the docker container

## Pycharm cannot find Docker on MacOS

Trying to setup docker-compose based interpreter but getting this

```text
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

I found this [solution](https://stackoverflow.com/a/76003595) on SO. It didn't work the first time around.
I restarted Docker Desktop again and also restarted Pycharm to finally get it working.