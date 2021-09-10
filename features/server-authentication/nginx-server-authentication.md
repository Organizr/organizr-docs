---
description: Utilizing Nginx's server_auth
---

# Nginx Server Authentication

After reading about how Server Authentication works, next we will need to set up the rewriting directive.

## Using the Organizr authorization API

### Native Nginx

```text
location ~ /auth-(.*) {
	internal;
	rewrite ^/auth-(.*) /api/v2/auth/$1;
}
```

### Docker Container

```text
location ~ /auth-(.*) {
        internal;
        proxy_pass http://[docker/hostIP]:[port]/api/v2/auth/$1;
        proxy_set_header Content-Length "";
}
```

### **SWAG/Letsencrypt Docker**

There is already a preconfigured file for this. Find the `organizr-auth.subfolder.conf.sample` and edit it the same way you did for your main Organizr file and remove the .sample.

For subfolders, just add one of the `auth_request` lines into the subfolder config with the groups as explained above.

For subdomains, add the `auth_request` same as you would for a subfolder and add an include for the file such as:

```text
include /config/nginx/proxy-confs/organizr-auth.subfolder.conf;
auth_request /auth-0;
```

{% hint style="info" %}
Note: If you are using a reverse proxy, this should be added on the reverse proxy layer
{% endhint %}

### **Subdomains**

For subdomains, you need to call back to the domain organizr is on, this can be done differently depending on your installation method.

Native, with local DNS setup \(This can also apply for containers\): `http://web.home.lab/api/v2/auth/$1`

Docker, using ip and port \(This is assuming the container is running in bridge\): `http://192.168.9.5:8080/api/v2/auth/$1`

```text
location ~ ^/auth-(.*) {
    ## Has to be local ip or local DNS name
    proxy_pass https://web.home.lab/api/v2/auth/$1;
    proxy_pass_request_body off;
    proxy_set_header Content-Length "";
    
}
```

### **How to include the authorization block in a reverse proxy**

All you need to do is include one line per reverse proxy block as the very first line: `auth_request /auth-0;` Where **/auth-0** is the access level for admin. 

Here is a sample of a reverse proxy with admin access:

```text
location /[SERVICE] {
    auth_request /auth-0;
    proxy_pass http://[IP]:[PORT];
    add_header X-Frame-Options "SAMEORIGIN";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

### **Excluding a location from authentication**

Most of our [examples](https://github.com/organizrTools/Config-Collections-for-Nginx/blob/master/Apps/sonarr.conf) already has this, but here is an explanation, using one of our examples\(with headers removed\)

```text
location /sonarr {
    proxy_pass http://127.0.0.1:8989/sonarr;
    auth_request /auth-0;
    location /sonarr/api { # We know that sonarr's api-endpoint is /api, so we are gonna open that up.
        auth_request off; # The line that actually opens it up
        proxy_pass http://127.0.0.1:8989/sonarr/api; # We need to tell nginx where to send the request
    }
}
```

### NPM

Please read the red bubbles in the screenshots carefully. Modify your Organizr proxy host configuration to include a custom location. Example where **`ip-address`** is local IP and **`8000`** is the port where Organizr is hosted:

```text
location: ~ /organizr-auth/(.*)
Forward Hostname/IP: ip-address/api/v2/auth/$1
```

![](../../.gitbook/assets/image%20%2873%29.png)

Modify the proxy host configuration for the service you want ServerAuth for. Modifications are needed in the Advanced section AND the Custom locations section.  Example is a ServerAuth setup for Sonarr \(as a subdomain\):

Advanced Custom Nginx Configuration section:

{% hint style="info" %}
**`organizr-auth`** can be any string you like - Just make sure to make it match the Custom Location **`location`** field on the next step.
{% endhint %}

```text
auth_request /organizr-auth/4;
```

![](../../.gitbook/assets/image%20%2865%29.png)

Custom Locations Section:

#### Location:

{% hint style="info" %}
**`organizr-auth`** can be any string you like - Just make sure to make it match the Advanced Tab
{% endhint %}

```text
~ /organizr-auth/(.*)
```

#### Forward Hostname / IP

{% hint style="warning" %}
Only change the IP Address in this URL & Don't forget to change the PORT to match yours
{% endhint %}

```text
organizr-ip-address/api/v2/auth/$1
```

![](../../.gitbook/assets/image%20%2866%29.png)

