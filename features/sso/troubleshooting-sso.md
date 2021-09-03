---
description: Troubleshooting Single Sign On
---

# Troubleshooting SSO

## Summary

In the event that something isn't working as expected, here we will short where to start looking to troubleshoot.

## Debug Area

In the drop down under your username in the top right there is an option for the Debug Area. From here use the drop down at the top and choose the SSO option you are trying to troubleshoot.

![](../../.gitbook/assets/image%20%2843%29.png)

![](../../.gitbook/assets/image%20%2841%29.png)

```text
misc:

oAuthLogin: true

rememberMe: true

rememberMeDays: 7

plex:

enabled: true

cookie: true

machineID: true

token: true

plexAdmin: email

strict: true

oAuthEnabled: true

backend: true

ombi:

enabled: true

cookie: true

url: http://docker.home.lab:3579/

api: true

tautulli:

enabled: true

cookie: false

url: http://docker.home.lab:8181
```

| SSO Values |
| :--- |
| misc.oAuthLogin = Current Login used oAuth |
| misc.rememberMe = Remember me button was toggle on login |
| misc.rememberMeDays = Cookie Length in days |
| %SSO\_TYPE%.enabled: false = SSO module enable status |
| %SSO\_TYPE%.cookie: false = Cookie status |
| %SSO\_TYPE%.url: false = URL of SSO module |
| %SSO\_TYPE%.api: false = The API key status if set |
| %SSO\_TYPE%.backend: false = Plex Backend is not enabled |
| %SSO\_TYPE%.machineID = Used for Plex - machineID status |
| %SSO\_TYPE%.token = The API key status if set |
| %SSO\_TYPE%.plexAdmin = Either username or email |
| %SSO\_TYPE%.strict = Status of Plex Friends status |
| %SSO\_TYPE%.oAuthEnabled = oAuth enable status |

