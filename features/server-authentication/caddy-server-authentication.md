---
description: Utilizing Caddy's reauth
---

# Caddy Server Authentication

## Using the Organizr authorization API

Using Caddy and the reauth plugin you can accomplish the same using the following block:

```text
reauth {
    path /sonarr   # location that requires reauth
    # path /glances   # other directories can be listed
    #
    # if someone is not authorized for a page, send them here instead
    failure redirect target=https://<your_domain>/
    
    upstream url=https://<your_domain>/api/v2/auth?group=<group_id>,cookies=true
}
```

## Using OAuth / JWT tokens

Here is a sample Caddy directive to protect a path using the Organizr token:

```text
jwt {
    # Name of the path to protect
    path /protected
    
    # Allow / deny based on JWT claims
    allow group Admin
    allow group User
    
    # Where to redirect in case the token is invalid or the claims are denied	
    redirect /
    
    # Where to read the token from
    token_source cookie organizr_token_62d9e46e-cdad-4726-9db7-e25b85397f57
    
    # Path the the secret to validate the token
    secret /etc/myprecious.txt
}
```

 The secret to use to validate the token needs to be passed to Caddy either as an environment variable named `JWT_SECRET` or in a file, specified with the `secret` configuration option.

{% hint style="info" %}
 Note that the http.jwt plugin is not installed in default Caddy builds. See [https://caddyserver.com/docs/http.jwt](https://caddyserver.com/docs/http.jwt) for instructions on how to install it.
{% endhint %}

 See [https://github.com/BTBurke/caddy-jwt](https://github.com/BTBurke/caddy-jwt) for more information on the jwt plugin and its configuration options.

{% hint style="info" %}
 You should not protect the `/` Organizr root path. Organizr handles it on its own.
{% endhint %}

