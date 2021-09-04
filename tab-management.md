# Tab Management

![](.gitbook/assets/image%20%2851%29.png)

## Summary

Organizr Tabs is the main focus of Organizr.  You can have as many tabs as you like and even put them into Categories.  Each tab can be configured to be opened in an iframe or in a new window.  In your Settings in Tab Editor, it will list all your current tabs. From this screen you can add, delete, disable, etc

{% hint style="warning" %}
Organizr **does not** do any type of proxying. The URL you are putting in the Tab URL **must** be accessible from your browser. This means if you want external access to the items you are putting in Organizr, they too must be externally accessible via a reverse proxy \(recommended\) or opened port.
{% endhint %}

## Tab Editor Settings

**Rearranging Tabs:** Hover over the three dots on the left side of the tabs, click and drag to where you want it. 

**Home Symbol \(See image below\):** This will take you to the Homepage setting based on a best guess of your tab name.

![](.gitbook/assets/image%20%2860%29.png)

{% hint style="info" %}
Homepage Items and Tabs are two separate things and one does not configure the other
{% endhint %}

**Category:** Group tabs together so you can collapse them \(set up your categories in the Categories tab below Tab Editor\) 

**Group:** Minimum Group that the tab should be shown to. Usually should match ServerAuth if you are using it.

{% page-ref page="features/server-authentication/" %}

**Type:**

* * Internal - only for Organizr things \(Homepage and Settings\)
  * iFrame - your standard tab
  * New Window - Opens a new window/tab

**Default:** Choose which tab loads when you first login.

{% hint style="info" %}
If you set the default tab to something that a user doesn't have access to, it will load the first in the list that they do.
{% endhint %}

**Active:** Enable or Disable the tab

**Splash:** Adds a link to the splash screen that shows when you log in/time out.

**Ping:** Enables the ping functionality \(must have Ping URL filled in\)

**Preload:** Loads the tab when you first load/login to Organizr

## Add/Edit Tabs

**Tab Name:** What you want the tab to show

**Tab URL:** URL you want to load. Examples: `/appname`, `https://app.domain.com` Note: must have scheme if you are using a full URL.

**Tab Local URL:** URL you want to load when you are local to Organizr \(RFC1918 space by default: 192.168.x.x, 10.x.x.x, or 172.16-31.x.x, You can add more address space in System Settings -&gt; Main -&gt; Login\) This is optional and can be left blank.

{% hint style="danger" %}
If you are using HTTPS and have this filled in, you will a Mixed Content Error when you are local unless you are also switching to an HTTP page for Organizr when local
{% endhint %}

**Ping URL:** URL and port to check if a service is up. Examples: `192.168.1.4:8181`, `tautulli:8181` These should not have scheme. This is optional and can be left blank.

**Tab Auto Action:** Auto Close/Reload after a specified amount of minutes. Note: this is timed of when the tab is opened, not when last interacted with.

**Tab Image:** Will fill in if you choose from the Image/Icon drop downs 

**Test Tab:** Will test to see if the tab can be loaded in an iFrame or not.

## I deleted my Homepage Tab, how do I get it back?

**Tab URL:** `api/v2/page/homepage`

**Original Icon:** `fontawesome::home` 

**Type \(after saving\):** Organizr

