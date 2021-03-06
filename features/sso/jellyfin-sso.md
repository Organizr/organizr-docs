---
description: Jellyfin Single Sign On
---

# Jellyfin SSO

## Summary

Using SSO with Jellyfin allows you to access the Jellyfin UI using only one sign in.

{% hint style="info" %}
Settings / System Settings / Single Sign-On / Jellyfin
{% endhint %}

![](../../.gitbook/assets/image%20%2845%29.png)

| **Type** | **Purpose** |
| :--- | :--- |
| Jellyfin API URL | URL for your Server's Jellyfin API Instance |
| Jellyfin SSO URL | URL for to pass to Jellyfin for SSO |
| Enable | Enable Jellyfin SSO |

## Tips

{% hint style="warning" %}
SSO doesn't work if Reverse Proxy is a subdomain - It must be on the same domain as Organizr
{% endhint %}



