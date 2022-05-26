---
description: Plex Single Sign On
---

# Plex SSO

## Summary

If you are using Plex as the main driving factor for your Organizr instance, you will want to enable Plex as backend choice to login via Plex credentials.

{% hint style="info" %}
Settings / System Settings / Main / Authentication
{% endhint %}

![](<../../.gitbook/assets/image (32).png>)

&#x20;Change the Authentication type to `Organizr DB + Backend`. Choose `Plex` as the Authentication Backend. Use the Retrieve button to fill in the Plex Token and Plex Machine.

The other two toggles are optional:

| **Type**            | **Purpose**                                                                                                                            |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Enable Plex oAuth   | This will bring up a Plex login screen that will flow credentials through plex.tv                                                      |
| Strict Plex Friends | Enabling this option will only allow people from your friends list that have access to the server that you selected for `Plex Machine` |

Now that Plex is setup to be the backend for Organizr, you can head over to the SSO section for Plex

{% hint style="info" %}
Settings / System Settings / Single Sign-On / Plex
{% endhint %}

{% hint style="danger" %}
If Plex account was made using Facebook/Google - YOU HAVE TO USE OAUTH to sign in
{% endhint %}

&#x20;Plex SSO will only work with Plex reverse proxied as a subdirectory and not as a subdomain. Fill out the `Plex Token` and `Plex Machine` (They should already be filled in if you did the above step). You can use the retrieve buttons to fill these out. Toggle the enabled switch to turn it on.

![](<../../.gitbook/assets/image (33).png>)

| **Type**       | **Purpose**                                           |
| -------------- | ----------------------------------------------------- |
| Plex Token     | Token to authenticate with Plex Servers               |
| Plex Machine   | Plex Machine ID for your specific server              |
| Admin Username | Username or Email for Organizr and Plex Admin account |
| Enable         | Enable Plex SSO                                       |

{% hint style="warning" %}
If not using Plex OAuth - For Admin Account - Make sure passwords match in Organizr and Plex
{% endhint %}

{% hint style="warning" %}
Plex SSO doesn't work if Plex Reverse Proxy is a subdomain - It must be on the same domain as Organizr
{% endhint %}

### Plex Reverse Proxy (Sub-Directory)

```
location /plex/ {
  proxy_pass http://ip-of-plex:32400/;
  include /path/to/proxy.conf;
}
if ($http_referer ~ /plex/) {
  rewrite ^/web/(.*) /plex/web/$1? redirect;
}
```

#### Contents of Proxy.conf

```
client_max_body_size 10m;
client_body_buffer_size 128k;
proxy_bind $server_addr;
proxy_buffers 32 4k;
#Timeout if the real server is dead
proxy_next_upstream error timeout invalid_header http_500 http_502 http_503;
# Advanced Proxy Config
send_timeout 5m;
proxy_read_timeout 240;
proxy_send_timeout 240;
proxy_connect_timeout 240;
proxy_hide_header X-Frame-Options;
# Basic Proxy Config
proxy_set_header Host $host:$server_port;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto https;
proxy_redirect  http://  $scheme://;
proxy_http_version 1.1;
proxy_set_header Connection "";
proxy_no_cache $cookie_session;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```
