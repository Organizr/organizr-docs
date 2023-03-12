---
description: Calibre Single Sign On with Auth Proxy
---

# Calibre SSO

## Summary

Calibre added support to Auth via header in v0.6.5. You need to set the Calibre settings in the Admin Configuration.

The `Reverse Proxy Header Name` should be the header you set in your reverse proxy config.

![](<../../../.gitbook/assets/image (47).png>)

{% hint style="info" %}
You will need to update your `calibre.conf` file as documented on the [Proxy Auth SSO](./) page.
{% endhint %}

Here is an example `calibre.conf` file for a subdomain of https://domain.com/calibre

```bash
location /calibre {
        proxy_bind              $server_addr;
        proxy_pass              http://127.0.0.1:8083;
        proxy_set_header        Host            $http_host;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header        X-Scheme        $scheme;
        proxy_set_header        X-Script-Name   /calibre;  # IMPORTANT: path has NO trailing slash
        auth_request /organizr-auth/4; #Change the X to whatever group you want to allow access
        auth_request_set $auth_user $upstream_http_x_organizr_user;
        proxy_set_header X-WEBAUTH-USER $auth_user;
}
```
