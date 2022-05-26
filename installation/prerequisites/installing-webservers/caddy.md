# Caddy



{% embed url="https://caddyserver.com/docs/install" %}
Caddy Installation Instructions
{% endembed %}

{% embed url="https://caddyserver.com/docs/running" %}
Installing Caddy as a Service
{% endembed %}

An example Caddy V2 Caddyfile with reverse proxy

```
mydomain.com {
    root * C:\Caddy\www\organizr\html
    php_fastcgi localhost:9000
    rewrite /api/v2/* /api/v2/index.php?{query}
    file_server

    # Subdirectory
    route /calibre/* {
        uri strip_prefix /calibre
        reverse_proxy localhost:9900
    }
}

# Subdomain
tautulli.mydomain.com {
    route {
        reverse_proxy localhost:8181
    } 
}
```
