---
description: Homepage Item Information
---

# Sonarr Homepage Item

![](../../.gitbook/assets/image%20%2880%29.png)

{% embed url="https://sonarr.tv/" %}

{% embed url="https://github.com/Sonarr/Sonarr" %}

## Summary

This homepage item allows you to have the following items displayed on the homepage:

* Calendar \(Upcoming and Past TV Shows\)
* Queue \(Current Downloading Items\)
* API Socks \(Access to API without needing to Reverse Proxy Sonarr\)

## Setting up

### Enable Tab

When setting up the homepage item, the first option is to enable it, do that by toggling the `Enable` Switch.  After that you need to set the `Minimum Authentication` group that will be able to use this item.

{% hint style="info" %}
 Please make sure not to set the `Minimum Authentication` to at least the same or lower than the homepage's `Tab Group`
{% endhint %}

![](../../.gitbook/assets/image%20%2878%29.png)

| **Field** | **Value** |
| :--- | :--- |
| Enable | Enable the Module |
| Main Module Minimum Authentication | Minimum Group needed to see Entire Module |

### Connection

Enter the connection details of your Sonarr server.  This homepage item allows multiple Sonarr server connections.

![](../../.gitbook/assets/image%20%2875%29.png)

| **Field** | **Value** |
| :--- | :--- |
| Multiple URL's | URL to your Sonarr Servers - LAN preferred  |
| Multiple API Key/Token's | API Keys to your Sonarr Servers |
| Disable Certificate Check | Disables the Certificate of the URL provided if https |
| Use Custom Certificate | Uses a Custom Certificate for the verification of the URL provided if https |

### API Socks

Organizr's API Socks allows you to access Sonarr's API without needing to Reverse Proxy it.

#### API Socks URL

{% tabs %}
{% tab title="Settings" %}
![](../../.gitbook/assets/image%20%2879%29.png)

| **Field** | **Value** |
| :--- | :--- |
| Enable | Enable the Module |
| Module Minimum Authentication | Minimum Group needed to use Module |
| Disable Certificate Check | Disables the Certificate of the URL provided if https |
{% endtab %}

{% tab title="Preview" %}
![](../../.gitbook/assets/image%20%2876%29.png)
{% endtab %}
{% endtabs %}

Depending on if you entered either one Sonarr Connection or multiple will determine which URL you will use.

{% tabs %}
{% tab title="One Sonarr Connection" %}
URL for Socks Connection:

{% hint style="info" %}
Replace `docker:8000` with the address to your actual Organizr Server
{% endhint %}

```text
http://docker:8000/api/v2/socks/sonarr/
```

I.E.

```text
http://demo.organizr.app/api/v2/socks/sonarr/
```
{% endtab %}

{% tab title="Multiple Sonarr Connections" %}
Depending on how many connections you setup, replace {\#} with which server you are connecting to.

URL for Socks Connection:

{% hint style="info" %}
Replace `docker:8000` with the address to your actual Organizr Server and `{#}` with the number of the connection
{% endhint %}

```text
http://docker:8000/api/v2/multiple/socks/sonarr/{#}
```

I.E.

```text
http://demo.organizr.app/api/v2/multiple/socks/sonarr/1
http://demo.organizr.app/api/v2/multiple/socks/sonarr/2
http://demo.organizr.app/api/v2/multiple/socks/sonarr/3
```
{% endtab %}
{% endtabs %}

### Queue

Enabling this module will output Sonarr's Download Queue on the homepage.

{% tabs %}
{% tab title="Settings" %}
![](../../.gitbook/assets/image%20%2882%29.png)

| **Field** | **Value** |
| :--- | :--- |
| Enable | Enable the Module |
| Module Minimum Authentication | Minimum Group needed to use Module |
| Add to Combined Downloader | Adds this downloader to a combined downloader module |
| Refresh Seconds | Sets the time in between data being refreshed |
{% endtab %}

{% tab title="Homepage Preview" %}
Better preview coming soon...

![](../../.gitbook/assets/image%20%2874%29.png)
{% endtab %}
{% endtabs %}

### Calendar

This module adds past and future TV Shows on the homepage Calendar

{% tabs %}
{% tab title="Settings" %}
![](../../.gitbook/assets/image%20%2883%29.png)

| **Field** | **Value** |
| :--- | :--- |
| Days Before | Amount of days to grab before todays date |
| Days After | Amount of days to grab after todays date |
| Start Day | First Day of Calendar for Week View |
| Default View | Which view to display on page load |
| Time Format | Format for the time of TV Showing |
| Locale | Region format for date |
| Items Per Day | Amount of items to show pre day before cut-off |
| Refresh Seconds | Sets the time in between data being refreshed |
| Show Unmonitored | Display shows that are unmonitored |
{% endtab %}

{% tab title="Homepage Preview" %}
![](../../.gitbook/assets/image%20%2877%29.png)
{% endtab %}
{% endtabs %}

### Test Connection

{% hint style="info" %}
 Make sure to save before hitting the `Test Connection` button
{% endhint %}

![](../../.gitbook/assets/image%20%2881%29.png)

