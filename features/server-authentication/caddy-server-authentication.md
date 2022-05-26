# Caddy Server Authentication

{% tabs %}
{% tab title="Caddy V1" %}
### Using the Organizr authorization API

Using Caddy and the reauth plugin you can accomplish the same using the following block:

```
reauth {
    path /sonarr   # location that requires reauth
    # path /glances   # other directories can be listed
    #
    # if someone is not authorized for a page, send them here instead
    failure redirect target=https://<your_domain>/
    
    upstream url=https://<your_domain>/api/v2/auth/<group_id>,cookies=true
}
```

### Using OAuth / JWT tokens

Here is a sample Caddy directive using caddy-jwt to protect a path using the Organizr token:

```
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

&#x20;The secret to use to validate the token needs to be passed to Caddy either as an environment variable named `JWT_SECRET` or in a file, specified with the `secret` configuration option.

{% hint style="info" %}
&#x20;Note that the http.jwt plugin is not installed in default Caddy builds.
{% endhint %}

{% hint style="info" %}
&#x20;You should not protect the `/` Organizr root path. Organizr handles it on its ow
{% endhint %}

{% embed url="https://github.com/BTBurke/caddy-jwt" %}
caddy-jwt Github
{% endembed %}
{% endtab %}

{% tab title="Caddy V2" %}
### Using JWT tokens

For Caddy v2, caddy-security authorize offers all the required functionality for server authentication

{% hint style="info" %}
Note that caddy-security plugin is not installed in default Caddy builds
{% endhint %}

{% embed url="https://github.com/greenpau/caddy-security" %}
caddy-security Github
{% endembed %}

{% embed url="https://authp.github.io/docs/authorize/intro" %}
Caddy Security Authorize Docs
{% endembed %}

An example Caddy V2 Caddyfile using caddy-security for authentication

```
security { 
    authorization policy admin {
        
        set auth url https://mydomain.com/auth
        crypto key token name organizr_token_uuid
        crypto key verify organizrHash
        set token sources cookie
        validate bearer header

        # Log any admin
        acl rule {
            match iss Organizr
            match role Admin
            allow stop log info
        }

        # Log any denied 
        acl rule {
            match iss any
            deny log warn
        }
    }

    authorization policy user {
        
        set auth url https://mydomain.com/auth
        crypto key token name organizr_token_uuid
        crypto key verify organizrHash
        set token sources cookie
        validate bearer header

        # Log any admin/user
        acl rule {
            match iss Organizr
            match role Admin User
            allow stop log info
        }

        # Log any denied 
        acl rule {
            match iss any
            deny log warn
        }
    }
}

mydomain.com {
    root * C:\Caddy\www\organizr\html
    php_fastcgi localhost:9000
    rewrite /api/v2/* /api/v2/index.php?{query}
    file_server

    # Subdirectory authentication
    route /calibre/* {
        uri strip_prefix /calibre
        authorize with user
        reverse_proxy localhost:9900
    }
}

# Subdomain authentication
tautulli.mydomain.com {
    route {
        authorize with admin
        reverse_proxy localhost:8181
    } 
}
```
{% endtab %}
{% endtabs %}
