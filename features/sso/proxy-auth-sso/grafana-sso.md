---
description: Grafana Single Sign On with Proxy Auth
---

# Grafana SSO

## Summary

SSO with Grafana is a combination of reverse proxy configuration and some settings in grafana.ini or with environment variables.

## Grafana.ini

```text
[auth.proxy]
enabled = true             
header_name = X-WEBAUTH-USER
header_property = username
auto_sign_up = true
;ldap_sync_ttl = 60
whitelist = 172.27.1.131
;headers = Email:X-User-Email, Name:X-User-Name
```

## Environment variables

```text
-e GF_AUTH_PROXY_ENABLED=true \             
-e GF_AUTH_PROXY_HEADER_NAME="X-WEBAUTH-USER" \
-e GF_AUTH_PROXY_HEADER_PROPERTY="username" \
-e GF_AUTH_PROXY_AUTO_SIGN_UP=true \
-e GF_AUTH_PROXY_LDAP_SYNC_TTL=60 \
-e GF_AUTH_PROXY_WHITELIST="172.27.1.131" \
-e GF_AUTH_PROXY_HEADERS="Email:X-User-Email, Name:X-User-Name"
```

## Notes

 You need to flip the enabled to true \(it's disabled by default\) and you should set the whitelist to the IP of your Organizr install so the header can only come from it. To read more about this, see [Grafana's docs](https://grafana.com/docs/auth/auth-proxy/).

