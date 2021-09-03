---
description: Plex Authentication Backend
---

# Plex Backend

## Summary

Using Plex as your Organizr backend allows you to authenticate using both your Local Plex Server as well as the servers at Plex.tv

![](../../.gitbook/assets/image%20%2832%29.png)

Change the Authentication type to `Organizr DB + Backend`. Choose `Plex` as the Authentication Backend. Use the Retrieve button to fill in the Plex Token and Plex Machine.‌

| **Type** | **Purpose** |
| :--- | :--- |
| Plex Token | Token to authenticate with Plex Servers |
| Plex Machine | Plex Machine ID for your specific server |
| Admin Username | Username or Email for Organizr and Plex Admin account |
| Enable Plex oAuth | This will bring up a Plex login screen that will flow credentials through plex.tv |
| Strict Plex Friends | Enabling this option will only allow people from your friends list that have access to the server that you selected for `Plex Machine` |

