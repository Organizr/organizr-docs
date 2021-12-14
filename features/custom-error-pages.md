# Custom Error Pages

## Summary

Organizr comes with integrated error pages, they have to be configured in the webserver.&#x20;

It accepts most error codes, and can do a redirect when the user has acknowledged the error.

![](<../.gitbook/assets/image (50).png>)

## Breakdown

The full Syntax for the error page is:

{% tabs %}
{% tab title="Nginx" %}
```
$scheme://$server_name/api/v2/organizr/error/$status?return=$request_uri;
```
{% endtab %}

{% tab title="Not used ATM" %}

{% endtab %}
{% endtabs %}

### URL Breakdown

| **Type**                | **Breakdown**                                                                                                                                                           |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $scheme://$server\_name | <p>This will translate to the URL to the domain that the servers gets the request from.  i.e.</p><p><em>https://organizr.app</em></p>                                   |
| /api/v2/organizr/error/ | Path to the error page                                                                                                                                                  |
| $status                 | <p>This will set the correct Status code for the error page</p><p><a href="https://www.restapitutorial.com/httpstatuscodes.html">HTTP Error Code (Status Codes)</a></p> |
| ?return=$request\_uri   | _**OPTIONAL:**_ This will let Organizr know to redirect the user once they have logged in                                                                               |

{% hint style="info" %}
You may set custom ones with predefined URLs
{% endhint %}

```
https://organizr.app/api/v2/organizr/error/401?return=https://demo.organizr.app
```

To get error pages to work with a reverse proxies, you may need to tell the webserver to intercept the errors from the service. &#x20;

\
In NGINX the way to do this is with `proxy_intercept_errors on;`&#x20;

This _can_ break some services (like plex), add `proxy_intercept_errors off;` to the location if that is the case.

## NGINX Example for Proxies

{% tabs %}
{% tab title="Regular Proxy" %}
```
# This is using nginx's built-in variables, should be copy-paste for most setups.
error_page 401 $scheme://$server_name/api/v2/organizr/error/$status?return=$request_uri; # We only want the Unauthorized code to redirect back to the loginpage

error_page 400 402 403 404 405 408 500 502 503 504  $scheme://$server_name/api/v2/organizr/error/$status; # Responds with the errorpage for the errorcodes listed
```


{% endtab %}

{% tab title="Subdomain Proxy" %}
{% hint style="warning" %}
For Subdomain's, we replaced `$server_name` with the domain organizr is on.
{% endhint %}

```
error_page 401 $scheme://organizr.app/api/v2/organizr/error/$status?return=$scheme://$host$request_uri; # We only want the Unauthorized code to redirect back to the loginpage

error_page 400 402 403 404 405 408 500 502 503 504  $scheme://organizr.app/api/v2/organizr/error/$status; # Responds with the errorpage for the errorcodes listed
```
{% endtab %}
{% endtabs %}

## Organizr Reverse Proxy

If you have Organizr Reversed Proxied, which we are sure you do.  You need to add an additional block for the API so it doesn't overwrite the errors for it.

```
location /api {
    include /config/nginx/proxy.conf; # Replace with any proxy config options
    proxy_pass http://organizr-ip:organizr-port;
    proxy_intercept_errors off; # This is the important part
}
```
