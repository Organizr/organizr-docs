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

![](../../.gitbook/assets/image%20%2837%29.png)

## 

## LDAP Backend

{% hint style="info" %}
Currently there are some limitations to using LDAP
{% endhint %}

![](../../.gitbook/assets/image%20%2839%29.png)

| **Type** | **Purpose** |
| :--- | :--- |
| Host Address | URL for the LDAP Server |
| Host Base DN | Hose base distinguished name |
| Account Prefix | The prefix for the account to build the distinguished name for the login |
| Account Suffix | The suffix for the account to build the distinguished name for the login |
| Bind Username | Username to bind the authentication to LDAP Server |
| Bind Password | Password to bind the authentication to LDAP Server |
| LDAP Backend Type | Type of LDAP Server |
| Account DN | Preview of the distinguished name for the login |
| Enable LDAP SSL | Option to enable the use of SSL for LDAP connections |
| Enable LDAP TLS | Option to enable the use of TLS for LDAP connections |

