---
description: Supported backends that work with Organizr
---

# Authentication \(Backends\)

## Summary

Organizr ships with a few options to use as the user management backend.  Backends allow Organizr to authenticate user logins to gain access to Organizr.

## Default Backend

The default backend is Organizr's built in user management.  It allows all user functions such as:

* Add Users
* Delete Users
* Edit Users

### Organizr + Backend

While using the default backend from Organizr, you may also add a secondary backend server to utilize as another option to authenticate.  Using this option still allows you the same user functions from Organizr's backend listed above.

{% hint style="info" %}
Settings / System Settings / Main
{% endhint %}

To get to the other backends head to the above path and change the `Authentication Type` option to `Organizr DB + Backend` and change the `Authentication Backend`option.

![](../.gitbook/assets/image%20%2837%29.png)

## Plex Backend

![](../.gitbook/assets/image%20%2832%29.png)

Change the Authentication type to `Organizr DB + Backend`. Choose `Plex` as the Authentication Backend. Use the Retrieve button to fill in the Plex Token and Plex Machine.‌

| **Type** | **Purpose** |
| :--- | :--- |
| Plex Token | Token to authenticate with Plex Servers |
| Plex Machine | Plex Machine ID for your specific server |
| Admin Username | Username or Email for Organizr and Plex Admin account |
| Enable Plex oAuth | This will bring up a Plex login screen that will flow credentials through plex.tv |
| Strict Plex Friends | Enabling this option will only allow people from your friends list that have access to the server that you selected for `Plex Machine` |

‌





