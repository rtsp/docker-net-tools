# RTSP Network Tools Docker Image

Debian with basic network tools included.

## Usage


### Run Once

```ShellSession
docker run --rm -it rtsp/net-tools bash
```


### Run as Daemon

```ShellSession
docker run -d --name rtsp-net-tools rtsp/net-tools
docker exec -it rtsp-net-tools bash
```



## Links

- [Docker Hub: rtsp/net-tools](https://hub.docker.com/r/rtsp/net-tools/)
- [GitHub: rtsp/docker-net-tools](https://github.com/rtsp/docker-net-tools)
