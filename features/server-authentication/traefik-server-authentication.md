---
description: Utilizing Traefik's auth-forward
---

# Traefik Server Authentication

## Using the Organizr authorization API

### **Træfik v1**

You can use Traefik's auth-forward feature to do the same.

Example docker-compose.yml block for Organizr:

```text
services:
  organizr:
    image: organizr/organizr
    environment:
      - fpm=true
      - branch=master
      - TZ
      - PUID=${USER_UID}
      - PGID=${USER_GID}
    labels:
      - "traefik.enable=true"
      - "traefik.organizr.frontend.rule=Host: www.your_domain.com"
      - "traefik.organizr.port=80"
    depends_on:
      - traefik
```

Example service that depends on user being authenticated to Organizr:

```text
services:
  nzbget:
    image: linuxserver/nzbget
    environment:
      - TZ
      - PUID=${USER_UID}
      - PGID=${USER_GID}
    labels:
      - "traefik.enable=true"
      - "traefik.frontend.rule=Host: nzbget.your_domain.com"
      - "traefik.frontend.auth.forward.address=http://organizr/api/v2/auth?group=1"
      - "traefik.port=6789"
    depends_on:
      - traefik
      - organizr
```

### **Træfik v2**

Træfik changed how the tags work in v2.

Example docker-compose.yml block for Organizr:

```text
services:
  organizr:
    image: organizr/organizr
    environment:
      - TZ
      - PUID=${USER_UID}
      - PGID=${USER_GID}
    labels:
      - "traefik.http.routers.organizr.rule=Host(`www.your_domain.com`)"
      - "traefik.http.services.organizr.loadbalancer.server.port=80"
      - "traefik.http.services.organizr.loadbalancer.server.scheme=http"
    depends_on:
      - traefik
```

Example service that depends on user being authenticated to Organizr:

```text
services:
  nzbget:
    image: linuxserver/nzbget
    environment:
      - TZ
      - PUID=${USER_UID}
      - PGID=${USER_GID}
    labels:
      - "traefik.http.routers.nzbget.service"
      - "traefik.http.routers.nzbget.rule=Host(`nzbget.your_domain.com`)'
      - "traefik.http.services.nzbget.loadbalancer.server.port=6789"
      - "traefik.http.routers.nzbget.middlewares=auth"
      - "traefik.http.middlewares.auth.forwardauth.address=http://organizr/api/v2/auth?group=1"
    depends_on:
      - traefik
      - organizr
```

