---
description: Ombi Single Sign On
---

# Ombi SSO

## Summary

Using SSO with Ombi allows you to access the Ombi UI using only one sign in.

{% hint style="info" %}
Settings / System Settings / Single Sign-On / Ombi
{% endhint %}

![](../../.gitbook/assets/image%20%2838%29.png)

| **Type** | **Purpose** |
| :--- | :--- |
| Ombi URL | URL for your Server's Ombi Instance |
| Token | API Key for Ombi |
| Ombi Fallback User | If your user doesn't have an Ombi account, Organizr will use this account |
| Ombi Fallback Password | Password for the above account |
| Enable | Enable Ombi SSO |

Fill out the URL for your Ombi install \(it can be the local IP or local DNS and port\) and copy your API key from Ombi's settings to the Token box. Toggle the enabled switch to turn it on.

If you are doing a subdomain for Ombi, go to your tabs and set the Tab URL to: 

| **Proxy Type** | **URL** |
| :--- | :--- |
| Subdomain \(ombi.domain.com\) | [https://ombi.domain.com/auth/cookie](https://ombi.domain.com/auth/cookie) |
| Directory  \(domain.com/ombi\) | https://domain.com/ombi |

## Tips

Please make sure that you have the following options enabled in Ombi.

![](../../.gitbook/assets/image%20%2849%29.png)

 By enabling those options, your users under User Management should have the `User Type` as `Plex User` now.

![](../../.gitbook/assets/image%20%2840%29.png)

