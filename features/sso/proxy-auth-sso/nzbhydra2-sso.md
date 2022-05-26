---
description: NZBHydra2 Single Sign On with Proxy Auth
---

# NZBHydra2 SSO

## Summary

NZBHydra has as of version 2.13.13 the ability to authorize using a similar method as Grafana.&#x20;

In NZBHydra's settings we need to set a few values. The `Auth header` has to have the same as the one in the NGINX reverse proxy (example to follow), while the `Secure ip ranges` should be set to the nginx ip.

![](<../../../.gitbook/assets/image (46).png>)
