---
description: Homepage Item Information
---

# Plex Homepage Item

![](<../../.gitbook/assets/image (16).png>)

## Summary

The Plex homepage item allows you to have the following Plex items displayed on the homepage:

* Current playing streams (Active Streams)
* Recently Added items
* Media search functionality
* Playlists

## Setting up

### Enable Tab

&#x20;When setting up the Plex homepage item, the first option is to enable it, do that by toggling the `Enable` Switch.  After that you need to set the `Minimum Authentication` group that will be able to use this item.

{% hint style="info" %}
&#x20;Please make sure not to set the `Minimum Authentication` to at least the same or lower than the homepage's `Tab Group`
{% endhint %}

| **Field**                          | **Value**                                 |
| ---------------------------------- | ----------------------------------------- |
| Enable                             | Enable the Plex Module                    |
| Main Module Minimum Authentication | Minimum Group needed to see Entire Module |

![](<../../.gitbook/assets/image (8).png>)

### Connection

If you already went through the [Plex SSO](https://docs.organizr.app/books/setup-features/page/sso) or [Plex Authentication](https://docs.organizr.app/books/setup-features/page/plex-authentication) setup you will have these next fields already saved, if not, you can hit set those values using the provided buttons.&#x20;

| **Field**                 | **Value**                                                                   |
| ------------------------- | --------------------------------------------------------------------------- |
| URL                       | URL to your Plex Server - LAN preferred                                     |
| Disable Certificate Check | Disables the Certificate of the URL provided if https                       |
| Use Custom Certificate    | Uses a Custom Certificate for the verification of the URL provided if https |
| Token                     | Plex Token (Use button to retrieve)                                         |
| Plex Machine              | Plex Machine ID (Use button to retrieve)                                    |

![](<../../.gitbook/assets/image (9).png>)

### Active Streams

With this module, you will be able to show the current streams that are active on your Plex server live.

| **Field**                               | **Value**                                          |
| --------------------------------------- | -------------------------------------------------- |
| Enable                                  | Enable the Plex Active Stream Module               |
| Module Minimum Authentication           | Minimum Group needed to see Module                 |
| User Information                        | Show Plex User name                                |
| User Information Minimum Authentication | Minimum Group needed to see Plex User name         |
| Refresh Seconds                         | How many seconds to refresh the Module             |
| Libraries to Exclude                    | Drop down of libraries to exclude from this Module |

{% tabs %}
{% tab title="Settings" %}
![](<../../.gitbook/assets/image (10).png>)
{% endtab %}

{% tab title="Preview" %}
![](<../../.gitbook/assets/image (17).png>)
{% endtab %}
{% endtabs %}

### Recent Items

With this module, you will be able to display all recent items that were added to your Plex server.

| **Field**                     | **Value**                                          |
| ----------------------------- | -------------------------------------------------- |
| Enable                        | Enable the Plex Recent Items Module                |
| Module Minimum Authentication | Minimum Group needed to see Module                 |
| Libraries to Exclude          | Drop down of libraries to exclude from this Module |
| Item Limit                    | Limit the total # of items to display              |
| Refresh Seconds               | How many seconds to refresh the Module             |

{% tabs %}
{% tab title="Settings" %}
![](<../../.gitbook/assets/image (11).png>)
{% endtab %}

{% tab title="Preview" %}
![](<../../.gitbook/assets/image (18).png>)
{% endtab %}
{% endtabs %}

### Media Search

With this module, you will be able to show a search button on your Organizr so users may search your library.

| **Field**                     | **Value**                                          |
| ----------------------------- | -------------------------------------------------- |
| Enable                        | Enable the Plex Media Search Module                |
| Module Minimum Authentication | Minimum Group needed to see Module                 |
| Libraries to Exclude          | Drop down of libraries to exclude from this Module |
| Media Server                  | Choose Plex                                        |

{% tabs %}
{% tab title="Settings" %}
![](<../../.gitbook/assets/image (12).png>)
{% endtab %}

{% tab title="Preview" %}
![](<../../.gitbook/assets/image (19).png>)
{% endtab %}
{% endtabs %}

### Playlists

With this module, you will be able to show your curated playlists from your Plex server.

| **Field**                     | **Value**                          |
| ----------------------------- | ---------------------------------- |
| Enable                        | Enable the Plex Playlist Module    |
| Module Minimum Authentication | Minimum Group needed to see Module |

{% tabs %}
{% tab title="Settings" %}
![](<../../.gitbook/assets/image (13).png>)
{% endtab %}

{% tab title="Preview" %}
![](<../../.gitbook/assets/image (20).png>)
{% endtab %}
{% endtabs %}

### Misc Options

| **Field**                           | **Value**                                        |
| ----------------------------------- | ------------------------------------------------ |
| Plex Tab Name                       | The Plex Tab name if you have one configured     |
| Plex Tab WAN URL                    | The WAN facing URL i.e. https://domains.com/plex |
| Image Cache Size                    | Cache Image quality                              |
| Use Tautulli custom names for users | Grab custom names from Tautulli                  |

{% hint style="info" %}
&#x20;The `Plex Tab Name` and `Plex Tab WAN URL` are used to configure the homepage items to open up inside the Plex Tab you have setup.

To enable`Use Tautulli custom names for users` You must setup the Tautulli Homepage item.
{% endhint %}

![](<../../.gitbook/assets/image (14).png>)

### Test Connection

{% hint style="info" %}
&#x20;Make sure to save before hitting the `Test Connection` button
{% endhint %}

![](<../../.gitbook/assets/image (15).png>)
