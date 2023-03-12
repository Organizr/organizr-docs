---
description: Proxy Auth Single Sign On
---

# Proxy Auth SSO

## Summary

Using SSO with Proxy Auth allows you to access an apps UI using only one sign in.

All of these type of apps work the same way. For the reverse proxy you need to add the following:

```
auth_request /organizr-auth/X; #Change the X to whatever group you want to allow access
auth_request_set $auth_user $upstream_http_x_organizr_user;
proxy_set_header X-WEBAUTH-USER $auth_user;
```
