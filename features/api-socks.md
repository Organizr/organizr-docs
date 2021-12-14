---
description: API Connections without needing to Reverse Proxy Services
---

# API Socks

## Summary

Do you need access to a services API through WAN but don't want to reverse proxy it?  Organizr can help with that...  All you need to do is, enable that option under that Services homepage item.  After that, you are off to the races.

## Supported Apps

| Application | Supports Multiple Servers |
| ----------- | ------------------------- |
| Sonarr      | Yes                       |
| Radarr      | Yes                       |
| Lidarr      | Yes                       |
| Tautulli    | Yes                       |
| SabNZBd     | No                        |
| NZBGet      | No                        |
| qBittorrent | No                        |

## URL Endpoints

{% tabs %}
{% tab title="One Connection" %}
{% hint style="info" %}
Make sure to replace the corresponding fields
{% endhint %}

| Field              | Value                            |
| ------------------ | -------------------------------- |
| {ORGANIZR\_DOMAIN} | Domain of your Organizr instance |
| {SERVICE}          | Supported Application            |

```
http://{ORGANIZR_DOMAIN}/api/v2/socks/{SERVICE}/
```

I.E.

```
http://demo.organizr.app/api/v2/socks/sonarr/
```
{% endtab %}

{% tab title="Multiple Connections" %}


{% hint style="info" %}
Make sure to replace the corresponding fields
{% endhint %}

| Field              | Value                                           |
| ------------------ | ----------------------------------------------- |
| {ORGANIZR\_DOMAIN} | Domain of your Organizr instance                |
| {SERVICE}          | Supported Application                           |
| {#}                | Id of Supported Application (Order in Organizr) |

```
http://{ORGANIZR_DOMAIN}/api/v2/multiple/socks/{SERVICE}/{#}
```

I.E.

```
http://demo.organizr.app/api/v2/multiple/socks/sonarr/1
http://demo.organizr.app/api/v2/multiple/socks/sonarr/2
http://demo.organizr.app/api/v2/multiple/socks/sonarr/3
```
{% endtab %}
{% endtabs %}

## Example API Call

{% swagger baseUrl="https://demo.organizr.app/api" path="/v2/socks/sonarr/api/system/status?apikey=sonarrAPIkey" method="get" summary="API Socks" %}
{% swagger-description %}
Calls Sonarr's API
{% endswagger-description %}

{% swagger-parameter in="path" name="apikey" type="string" %}
Sonarr's API Key
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Token" type="string" %}
Organizr's API Key
{% endswagger-parameter %}

{% swagger-response status="200" description="Success" %}
```
{
  "version": "3.0.6.1265",
  "buildTime": "2021-06-17T12:24:54Z",
  "isDebug": false,
  "isProduction": true,
  "isAdmin": false,
  "isUserInteractive": false,
  "startupPath": "/app/sonarr/bin",
  "appData": "/config",
  "osName": "ubuntu",
  "osVersion": "18.04",
  "isMonoRuntime": true,
  "isMono": true,
  "isLinux": true,
  "isOsx": false,
  "isWindows": false,
  "branch": "main",
  "authentication": "none",
  "sqliteVersion": "3.22.0",
  "urlBase": "",
  "runtimeVersion": "5.20.1.34",
  "runtimeName": "mono"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="User Unauthorized" %}
```
{
    "response": {
        "result": "error",
        "message": "Not Authorized",
        "data": null
    }
}
```
{% endswagger-response %}
{% endswagger %}
