---
description: Migrating from certain versions requires changes on your part
---

# Migration Guide

## Summary

Sometimes Organizr needs to be updated on the backend which causes user interaction that needs to be completed prior to the upgrade.

## Version 2.0 -> Version 2.1

### Prerequisites <a href="#bkmrk-prerequisites" id="bkmrk-prerequisites"></a>

#### DOCKER USERS

First of all, if you are using an Organizr docker container, please make sure you are using the new Organizr Image:

{% embed url="https://github.com/Organizr/docker-organizr#migration" %}

```
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

If you are using or you have now switched over to this container, you will not need to add the new api location block as that has been added for you in the container image.  If you are using Nginx's Auth\_Request module, you will need to update that address by following these instructions: [HERE](https://docs.organizr.app/link/39#bkmrk-nginx-auth\_request)

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

```
location /api/v2 {
	try_files $uri /api/v2/index.php$is_args$args;
}
```

&#x20;If you are using Nginx's Auth\_Request module, you will need to update that address by following these instructions:

{% content-ref url="../../features/server-authentication/nginx-server-authentication.md" %}
[nginx-server-authentication.md](../../features/server-authentication/nginx-server-authentication.md)
{% endcontent-ref %}

#### Apache

```
RewriteEngine On
RewriteBase /api/v2
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ /api/v2/index.php [QSA,L]
```

#### Caddy v2 <a href="#bkmrk-c2-a0-2" id="bkmrk-c2-a0-2"></a>

```
rewrite /api/v2/* /api/v2/index.php?{query}
```

#### Caddy v1 <a href="#bkmrk-c2-a0" id="bkmrk-c2-a0"></a>

```
rewrite / {
   regexp ^/api/v2/*
   to /api/v2/index.php?{query}
}
```

#### IIS <a href="#bkmrk-iss" id="bkmrk-iss"></a>

1. Head to IIS webpage Settings
2. Import Rules
3. Paste code below and Apply

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ /api/v2/index.php [QSA,L]
```

Also make sure you allow the following HTTP Methods:

1. Put
2. Delete
3. Options

### Nginx Auth\_Request <a href="#bkmrk-nginx-auth_request" id="bkmrk-nginx-auth_request"></a>

You will need to update your url for the auth\_request module.

{% hint style="warning" %}
Notice the position of the ? and & signs
{% endhint %}

#### **Subdirectories**

```
Change from this line:
rewrite ^/auth-(.*) /api/?v1/auth&group=$1;

Change to this line:
rewrite ^/auth-(.*) /api/v2/auth/$1;
```

The complete config block will look like this:

```
location ~ /auth-(.*) {
	rewrite ^/auth-(.*) /api/v2/auth/$1;
}
```

#### **Subdomains**

{% hint style="danger" %}
&#x20;Do not just copy/paste this. This is just an example of how it should look. `organizr-upstream` is probably not the same as your setup.
{% endhint %}

```
Change from this line:
proxy_pass http://organizr-upstream/api/?v1/auth&group=$1;

Change to this line:
proxy_pass http://organizr-upstream/api/v2/auth/$1;
```

The complete config block will look like this:

```
location ~ ^/auth-(.*) {
	## Has to be local ip or local DNS name or if Proxied use proxy address
	proxy_pass http://organizr-upstream/api/v2/auth/$1;
	proxy_pass_request_body off;
	proxy_set_header Content-Length "";
}
```

### Jellyfin & Emby <a href="#bkmrk-jellyfin-26-emby" id="bkmrk-jellyfin-26-emby"></a>

We have separated Jellyfin from Emby. Please head to the homepage item section and fix accordingly. If you are using Jellyfin for authentication, please edit the Authentication settings and input your URL and Token respectively.

### Troubleshooting <a href="#bkmrk-troubleshooting" id="bkmrk-troubleshooting"></a>

#### Nothing loading at all/Blank Organizr <a href="#bkmrk-nothing-loading-at-a" id="bkmrk-nothing-loading-at-a"></a>

If your Organizr is totally blank or you see 404's in the Browser Console (F12) then more than likely you did not perform the API location block addition at all or correctly.  Please refer to the changes above for the appropriate webserver.

#### Settings/Homepage won't load <a href="#bkmrk-settings-2fhomepage-wo" id="bkmrk-settings-2fhomepage-wo"></a>

If you cannot access the settings/homepage page in Organizr - more than likely the tab url updater didn't run successfully.

Head over to your browser and type in the following:

{% swagger baseUrl="http://organizr-location" path="/api/v2/update/migrate/2.1.0" method="get" summary="Migration API Endpoint" %}
{% swagger-description %}
Migrates some settings from 2.0 to 2.1
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```
{
    "response": {
        "result": "success",
        "message": "Ran update function for version: 2.1.0",
        "data": null
    }
}
```
{% endswagger-response %}
{% endswagger %}

Now you can go back to Organizr and reload if you haven't and check the settings/homepage page.

#### Custom Homepage Items Missing <a href="#bkmrk-custom-homepage-item" id="bkmrk-custom-homepage-item"></a>

There was a type on the there config item keys.  There are a total of 4 of them.  Go to the file `/api/config/config.php` and change these four keys from the left side of the image to the right side of the image.  All that was missing was the letter `e`

![](<../../.gitbook/assets/image (52).png>)
