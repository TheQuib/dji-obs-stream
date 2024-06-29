# DJI-OBS-Stream - Docker

Set up information for NGINX in Docker for RTMP streaming from a DJI drone or other devices that use the RTMP protocol.

It's basically the same as the [Linux version](../Linux/README.md), but in already precooked Docker container.

There are two tags for this image: `latest` and `alpine`.
The `latest` tag is based on the `ubuntu` image, while the `alpine` tag is based on the `alpine` image.
Their size at the time of writing is `581MB` and `149MB` respectively.

Both are build for two different architectures: `amd64` and `arm64`.

Available on [Docker Hub](https://hub.docker.com/r/prochac/dji-obs-stream/tags).
