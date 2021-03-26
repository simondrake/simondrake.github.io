---
layout: post
title: "Setup SOCKS5 Proxy Server"
subtitle: "How to set-up a SOCKS5 Proxy Server"
---

Learn how to set-up a SOCKS5 Proxy server.

# Pre-requisites

- An Ubuntu server running Docker/docker-comose.

# Set-up

1. Create a `docker-compose.yaml` file, or edit your existing one
2. Add the following

```yaml
---
version: "2.1"
services:
    socks5:
    image: serjs/go-socks5-proxy
    container_name: socks5
    environment:
      - PROXY_USER=<your-username> # Comment out for no user/password
      - PROXY_PASSWORD=<your-password> # Comment out for no userpassword
    ports:
      - 1080:1080
```

3. Run `docker-compose up -d`
4. From a different machine run `curl --socks5 <docker host ip>:1080 http://icanhazip.com` and make sure it has the same IP of your server.

# References

- [go-socks5-proxy](https://hub.docker.com/r/serjs/go-socks5-proxy/) docker image.
