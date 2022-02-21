---
description: Komga Single Sign On
---

# Komga SSO

## Summary

Using SSO with Komga allows you to access the Komga UI using only one sign in.

{% hint style="info" %}
Settings / System Settings / Single Sign-On / Komga
{% endhint %}

![](<../../.gitbook/assets/image (86).png>)

| **Type**               | **Purpose**                          |
| ---------------------- | ------------------------------------ |
| URL                    | URL for your Server's Komga Instance |
| Minimum Authentication | Group Needed to use SSO              |
| Enable                 | Enable Komga SSO                     |

## Tab Setup

{% hint style="warning" %}
Komga needs a specific Tab URL in order to function
{% endhint %}

![](<../../.gitbook/assets/image (87).png>)

| Type    | Value                                   |
| ------- | --------------------------------------- |
| Tab URL | http://komga-domain/?xAuthToken={komga} |

Replace <mark style="color:orange;">komga-domain</mark> with the actual domain of your Komga instance.
