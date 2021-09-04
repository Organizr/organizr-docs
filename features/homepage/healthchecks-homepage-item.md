# HealthChecks Homepage Item

![](../../.gitbook/assets/image%20%2819%29.png)

## Summary

The HealthChecks Homepage item allows you to see all your health checks on your page at a quick glance.

## Setting up

### Enable

 When setting up the homepage item, the first option is to enable it, do that by toggling the `Enable` Switch.  After that you need to set the `Minimum Authentication` group that will be able to use this item.

{% hint style="info" %}
 Please make sure not to set the `Minimum Authentication` to at least the same or lower than the homepage's `Tab Group`
{% endhint %}

| **Field** | **Value** |
| :--- | :--- |
| Enable | Enable the Plex Module |
| Main Module Minimum Authentication | Minimum Group needed to see Entire Module |

![](../../.gitbook/assets/image%20%2829%29.png)

### Connection

If you already went through the [Plex SSO](https://docs.organizr.app/books/setup-features/page/sso) or [Plex Authentication](https://docs.organizr.app/books/setup-features/page/plex-authentication) setup you will have these next fields already saved, if not, you can hit set those values using the provided buttons. 

| **Field** | **Value** |
| :--- | :--- |
| URL | URL to the HealthCheck API \(Local or Main API Site\)  |
| Disable Certificate Check | Disables the Certificate of the URL provided if https |
| Use Custom Certificate | Uses a Custom Certificate for the verification of the URL provided if https |

![](../../.gitbook/assets/image%20%2826%29.png)

### Misc Options

| **Field** | **Value** |
| :--- | :--- |
| Tags | Pull only checks with this tag |
| Refresh Seconds | How many seconds to update the healthchecks on the homepage |
| Show Description | Show description on the healthchecks item |
| Show Tags | Show tags on the healthchecks item |

![](../../.gitbook/assets/image%20%2818%29.png)

## Signing up at HealthChecks.io

Head over to [https://healthchecks.io](https://healthchecks.io/) and create an account.  Once created head over to User Menu and select Project Settings to copy your API Key.

![](../../.gitbook/assets/image%20%2823%29.png)



Enable the API and once enabled click on Show API Keys

![](../../.gitbook/assets/image%20%2856%29.png)

Copy the API key \(Top one\) and paste into Organizr

![](../../.gitbook/assets/image%20%2861%29.png)

### **Tips**

You can setup logo/images for the checks if you add an images URL to the tags section for that check

![](../../.gitbook/assets/image%20%2863%29.png)

