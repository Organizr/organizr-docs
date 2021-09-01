---
description: Login Error - API Connection Failed
---

# Organizr Login Error

## Summary

This seems to be coming up more frequently lately. The Organizr logs will look like there was a successful login. If you are reverse proxying Organizr, you may see the following error in your NGINX logs:

```text
2020/05/12 15:52:05 [error] 439#439: *1502 upstream sent too big header while reading response header from upstream, client: 10.0.10.50, server: org.*, request: "POST /api/v2/login HTTP/2.0", upstream: "http://172.18.0.5:80/api/v2/login", host: "domain.com", referrer: "https://domain.com/"
```

## Solution

With a reverse proxy, this seems to be a two step process to fix. If you're not using a reverse proxy skip to the next one. First adding \(or increasing the limits if you have this set already, check your `proxy.conf`\) like the following:

```text
proxy_buffer_size          128k;
proxy_buffers              4 256k;
proxy_busy_buffers_size    256k;
```

The second part and the part if you're not reverse proxying and just running it native, you may see this in the logs:

```text
2020/05/20 11:58:31 [error] 130009#130009: *87200 upstream sent too big header while reading response header from upstream, client: 172.xx.xx.xx, server: Domain.org, request: "POST /api/v2/login HTTP/1.1", upstream: "fastcgi://unix:/run/php/php7.4-fpm.sock:", host: "www.domain.org", referrer: "https://domain.org/"
```

And this needs to go in the NGINX config where your Organizr is in the PHP block \(if you are on the new `organizr/organizr` container, we have added this now\):

```text
fastcgi_buffers 32 32k;
fastcgi_buffer_size 32k;
```

