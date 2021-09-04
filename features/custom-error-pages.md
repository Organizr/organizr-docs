# Custom Error Pages

## Summary

Organizr comes with integrated error pages, they have to be configured in the webserver. 

It accepts most error codes, and can do a redirect when the user has acknowledged the error.

![](../.gitbook/assets/image%20%2850%29.png)

## Breakdown

The full Syntax for the error page is:

{% tabs %}
{% tab title="Nginx" %}
```text
$scheme://$server_name/?error=$status&return=$request_uri
```
{% endtab %}

{% tab title="Not used ATM" %}

{% endtab %}
{% endtabs %}

### URL Breakdown

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Type</b>
      </th>
      <th style="text-align:left"><b>Breakdown</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">$scheme://$server_name</td>
      <td style="text-align:left">
        <p>This will translate to the URL to the domain that the servers gets the
          request from. i.e.</p>
        <p><em>https://organizr.app</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">?error=$status</td>
      <td style="text-align:left">
        <p>This will set the correct Status code for the error page</p>
        <p><a href="https://www.restapitutorial.com/httpstatuscodes.html">HTTP Error Code (Status Codes)</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&amp;return=$request_uri</td>
      <td style="text-align:left"><em><b>OPTIONAL: </b></em>This will let Organizr know to redirect the
        user once they have logged in</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
You may set custom ones with predefined URLs
{% endhint %}

```text
https://organizr.app/?error=401&return=https://organizr.app
```

To get error pages to work with a reverse proxies, you may need to tell the webserver to intercept the errors from the service.  

  
In NGINX the way to do this is with `proxy_intercept_errors on;` This _can_ break some services \(like plex\), add `proxy_intercept_errors off;` to the location if that is the case.

## NGINX Example for Proxies

```text
# This is using nginx's built-in variables, should be copy-paste for most setups.
# for subdomain, replace $server_name with the domain organizr is on
error_page 401 $scheme://$server_name/?error=$status&return=$request_uri; # We only want the Unauthorized code to redirect back to the loginpage

error_page 400 402 403 404 405 408 500 502 503 504  $scheme://$server_name/?error=$status; # Responds with the errorpage for the errorcodes listed
```

{% hint style="warning" %}
Please see note about Subdomains from above example
{% endhint %}

 For Subdomain's, replace `$server_name` with the domain organizr is on, i.e. `organizr.app`

```text
error_page 401 $scheme://organizr.app/?error=$status&return=$scheme://$host$request_uri; # We only want the Unauthorized code to redirect back to the loginpage

error_page 400 402 403 404 405 408 500 502 503 504  $scheme://organizr.app/?error=$status; # Responds with the errorpage for the errorcodes listed
```



