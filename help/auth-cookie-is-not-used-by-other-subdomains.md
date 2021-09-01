# Auth cookie is not used by other subdomains

## Caddy

This is caused by Caddy not adding the necessary headers when forwarding the login request to Organizr.

Using the "transparent" preset or manually adding the necessary headers solves the issue.

For Example:

```text
organizr.example.com {
  proxy / http://organizr.internal {
    transparent
  }
}
```

See more: [Here](https://caddyserver.com/docs/proxy)

