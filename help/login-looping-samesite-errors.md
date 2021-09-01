# Login Looping - SameSite Errors

## Summary

Are you getting stuck in a redirect loop?  Are you seeing some console errors in your browser about SameSite Cookies?

Browsers are starting to enforce strict rules on Cookies set by web apps. The issue here is when an application is not hosted on the same host as Organizr. You have three options...

## Options

{% tabs %}
{% tab title="Host file location" %}
We will use windows as an example.

The Hosts file in Windows is located at the following location:

```text
C:\Windows\System32\drivers\etc
```

Here you will see the Hosts file. Right-click on it and select Notepad. Make the changes and Save.

But sometimes, even when you are logged on with administrative credentials, you may receive one of the following error message:

{% hint style="danger" %}
Access to C:\Windows\System32\drivers\etc\ hosts was denied
{% endhint %}

{% hint style="danger" %}
Cannot create the C:\Windows\System32\drivers\etc\hosts file. Make sure that the path and file name are correct.
{% endhint %}

In this case, type Notepad in Start search and right-click on the Notepad result. Select _Run as administrator_. Open the Hosts file, make the necessary changes, and then click Save.

The changes you need to make are like below:

```text
127.0.0.1       hostname
```

The left value is the IP address and the right value is the hostname or text you want to tie to that IP address. For this fix everything needs to be on the same domain \(basically like how subdomains work when reverse proxying\).

Note: They must be on the same subdomain for this to work. You can't just do:

 `<service>.tld`, they have to be `<service>.something.tld`

**Router/DNS**

Depending on your Router you will need to lookup how to achieve this.  Routers usually utilize using Dnsmasq.
{% endtab %}

{% tab title="Reverse Proxy" %}
Use specific webserver to achieve this.  Tutorials soon!
{% endtab %}

{% tab title="Browser Settings" %}
{% hint style="info" %}
This only works if the cookie is not being set with the SameSite property
{% endhint %}

## Chrome

Access this page from your browser  `chrome://flags`

Search for SameSite and disable it.

Note: This was only supposed to be a temporary setting and it seems like Chrome is starting to ignore what this is set to so you'll have to use one of the other options if you try and it doesn't work.

## **Edge**

Access this page from your browser [`edge://flags/`](edge://flags/) 

Search for SameSite and disable it.

![Edge settings](../.gitbook/assets/image%20%286%29.png)

## **FireFox**

Access this page from your browser [`about:config`](edge://flags/)

Search for SameSite and disable it.

![Firefox settings](../.gitbook/assets/image%20%283%29.png)

### **Application Errors**

Jackett and Tautulli seem to hardcode the SameSite cookie as Lax

Sonarr/Radarr/Lidarr are starting to hardcode the SameSite cookie as Strict

The only way to bypass that is to use Option \#1 or Option \#2

 [Read more about SameSite](https://web.dev/samesite-cookies-explained/)
{% endtab %}
{% endtabs %}

