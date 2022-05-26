---
description: Supported backends that work with Organizr
---

# Authentication Backend

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

![](<../../.gitbook/assets/image (48).png>)
