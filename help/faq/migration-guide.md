---
description: Migrating from certain versions requires changes on your part
---

# Migration Guide

## Summary

Sometimes Organizr needs to be updated on the backend which causes user interaction that needs to be completed prior to the upgrade.

## Version 2.0 -&gt; Version 2.1

### Prerequisites <a id="bkmrk-prerequisites"></a>

#### DOCKER USERS

First of all, if you are using an Organizr docker container, please make sure you are using the new Organizr Image:

{% embed url="https://github.com/Organizr/docker-organizr\#migration" %}

```text
organizr:
    container_name: organizr
    hostname: organizr
    image: organizr/organizr
    restart: unless-stopped
    ports:
        - 80:80
    volumes:
        - /home/organizr:/config
    environment:
        - fpm=true #true or false | using true will provide better performance
        - branch=v2-master #v2-master or #v2-develop
        - PUID=1000
        - PGID=1000
        - TZ=${TZ}
```

If you are using or you have now switched over to this container, you will not need to add the new api location block as that has been added for you in the container image.  If you are using Nginx's Auth\_Request module, you will need to update that address by following these instructions: [HERE](https://docs.organizr.app/link/39#bkmrk-nginx-auth_request)

Note: the branch variable will take short hand such as `master` instead of `v2-master` and `dev` or `develop` instead of `v2-develop` as may be seen in the tag migration examples.

#### NON-DOCKER USERS

We will need to update your webserver to include the new API location block.

### API Location Block

{% hint style="info" %}
This is only needed on the actual webserver that is running Organizr - This also assumes Organizr is proxied to root directory - You will need to add directory to example if it is not running at root directory.
{% endhint %}

{% hint style="danger" %}
If you reverse proxy Organizr to another Webserver - do not add to that server. Only add to the webserver that hosts Organizr.
{% endhint %}

#### Nginx

Please include this location block within the server block that houses Organizr.

```text
location /api/v2 {
	try_files $uri /api/v2/index.php$is_args$args;
}
```

 If you are using Nginx's Auth\_Request module, you will need to update that address by following these instructions:

{% page-ref page="../../features/server-authentication/nginx-server-authentication.md" %}

#### Apache

```text
RewriteEngine On
RewriteBase /api/v2
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ /api/v2/index.php [QSA,L]
```

#### Caddy v2 <a id="bkmrk-%C2%A0-2"></a>

```text
rewrite /api/v2/* /api/v2/index.php?{query}
```

#### Caddy v1 <a id="bkmrk-%C2%A0"></a>

```text
rewrite / {
   regexp ^/api/v2/*
   to /api/v2/index.php?{query}
}
```

#### IIS <a id="bkmrk-iss"></a>

1. Head to IIS webpage Settings
2. Import Rules
3. Paste code below and Apply

```text
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ /api/v2/index.php [QSA,L]
```

Also make sure you allow the following HTTP Methods:

1. Put
2. Delete
3. Options

### Nginx Auth\_Request <a id="bkmrk-nginx-auth_request"></a>

You will need to update your url for the auth\_request module.

{% hint style="warning" %}
Notice the position of the ? and & signs
{% endhint %}

#### **Subdirectories**

```text
Change from this line:
rewrite ^/auth-(.*) /api/?v1/auth&group=$1;

Change to this line:
rewrite ^/auth-(.*) /api/v2/auth/$1;
```

The complete config block will look like this:

```text
location ~ /auth-(.*) {
	rewrite ^/auth-(.*) /api/v2/auth/$1;
}
```

#### **Subdomains**

{% hint style="danger" %}
 Do not just copy/paste this. This is just an example of how it should look. `organizr-upstream` is probably not the same as your setup.
{% endhint %}

```text
Change from this line:
proxy_pass http://organizr-upstream/api/?v1/auth&group=$1;

Change to this line:
proxy_pass http://organizr-upstream/api/v2/auth/$1;
```

The complete config block will look like this:

```text
location ~ ^/auth-(.*) {
	## Has to be local ip or local DNS name or if Proxied use proxy address
	proxy_pass http://organizr-upstream/api/v2/auth/$1;
	proxy_pass_request_body off;
	proxy_set_header Content-Length "";
}
```

### Jellyfin & Emby <a id="bkmrk-jellyfin-%26-emby"></a>

We have separated Jellyfin from Emby. Please head to the homepage item section and fix accordingly. If you are using Jellyfin for authentication, please edit the Authentication settings and input your URL and Token respectively.

### Troubleshooting <a id="bkmrk-troubleshooting"></a>

#### Nothing loading at all/Blank Organizr <a id="bkmrk-nothing-loading-at-a"></a>

If your Organizr is totally blank or you see 404's in the Browser Console \(F12\) then more than likely you did not perform the API location block addition at all or correctly.  Please refer to the changes above for the appropriate webserver.

#### Settings/Homepage won't load <a id="bkmrk-settings%2Fhomepage-wo"></a>

If you cannot access the settings/homepage page in Organizr - more than likely the tab url updater didn't run successfully.

Head over to your browser and type in the following:

{% api-method method="get" host="http://organizr-location" path="/api/v2/update/migrate/2.1.0" %}
{% api-method-summary %}
Migration API Endpoint
{% endapi-method-summary %}

{% api-method-description %}
Migrates some settings from 2.0 to 2.1
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Now you can go back to Organizr and reload if you haven't and check the settings/homepage page.

#### Custom Homepage Items Missing <a id="bkmrk-custom-homepage-item"></a>

There was a type on the there config item keys.  There are a total of 4 of them.  Go to the file `/api/config/config.php` and change these four keys from the left side of the image to the right side of the image.  All that was missing was the letter `e` 

[![image-1598799936499.png](https://docs.organizr.app/uploads/images/gallery/2020-08/scaled-1680-/b9KhmSe9oVXa0dfG-image-1598799936499.png)](https://docs.organizr.app/uploads/images/gallery/2020-08/b9KhmSe9oVXa0dfG-image-1598799936499.png)

