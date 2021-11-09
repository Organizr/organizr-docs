# Fail2Ban Integration

## Summary

_**Fail2ban**_ scans log files \(e.g. /var/log/nginx/error.log\) and bans IPs that show malicious signs -- too many password failures, seeking for exploits, etc. Generally Fail2Ban is then used to update firewall rules to reject the IP addresses for a specified amount of time, although any arbitrary other **action** \(e.g. sending an email/notification\) could also be configured.

## Prerequisites

* Fail2ban installed and configured

## **Fail2Ban filter**

Go to your `filter.d` folder in your Fail2Ban install location `/etc/fail2ban/filter.d` and create a file called `organizr-auth.conf` and add the following:

```text
[Definition]
failregex = ","username":"\S+","ip":"<HOST>","auth_type":"error"}*
ignoreregex =
```

## **Organizr Jail**

Edit the `jail.local` file in the Fail2Ban directory and add the following: 

```text
[organizr-auth]
enabled = true
port = http,https
filter = organizr-auth
logpath = /var/www/html/db/organizrLoginLog.json
ignoreip = 192.168.1.0/24
```

The ignore IP is so that fail2ban won’t ban your local IP. Check out [https://www.aelius.com/njh/subnet\_sheet.html](https://www.aelius.com/njh/subnet_sheet.html) if you are wondering what your [**CIDR notation**](https://www.digitalocean.com/community/tutorials/understanding-ip-addresses-subnets-and-cidr-notation-for-networking) is. Most often it will be **/24** \(netmask 255.255.255.0\)  
To find your netmask run **`ipconfig /all`** on windows or **`ifconfig | grep netmask`** on linux.

Restart Fail2Ban with `sudo service fail2ban restart`

## Organizr logs

Normal Install

```text
/var/www/html/db/organizrLoginLog.json
```

Docker Install

```text
/config/db/organizrLoginLog.json
```

## Docker

Because the Organizr container only logs the docker IP addresses e.g `172.17.0.2` you need to add this in the Organizr default nginx site file. Go to `\organizr\nginx\site-confs\default` and add the following inside the server block:

```text
# get real IP
real_ip_header X-Forwarded-For;
set_real_ip_from 172.17.0.0/16;
real_ip_recursive on;
```

If you're using `organizrtools/organizr-v2` it's already added and you only need to uncomment the `set_real_ip_from` line. 

Then restart the container: `docker restart organizr`

### **Using the linuxserver/letsencrypt container**

{% hint style="info" %}
 The Fail2ban filter folder is in `/<appdatafolder>/letsencrypt/fail2ban/filter.d`
{% endhint %}

For this to work you need the letsencrypt container to be able to read the `organizrLoginLog.json` file in the Organizr container. 

Mount the Organizr log like this:

```text
-v <path/to/organizr/config/db:/organizrlog:ro
```

 And set the log path in the Fail2Ban `jail.local` file to `/organizrlog/organizrLoginLog.json`

## **Banned**

 The **`fail2ban.log`** file should output something like this:

```text
2017-08-08 21:51:13,777 fail2ban.filter [262]: INFO [organizr-auth] Found 5.153.234.107 - 2017-08-08 21:51:12
2017-08-08 21:51:18,811 fail2ban.filter [262]: INFO [organizr-auth] Found 5.153.234.107 - 2017-08-08 21:51:18
2017-08-08 21:51:43,965 fail2ban.filter [262]: INFO [organizr-auth] Ignore 192.168.1.1 by ip
2017-08-08 21:51:51,008 fail2ban.filter [262]: INFO [organizr-auth] Ignore 192.168.1.1 by ip
2017-08-08 21:51:57,045 fail2ban.filter [262]: INFO [organizr-auth] Ignore 192.168.1.1 by ip
2017-08-08 21:52:03,080 fail2ban.filter [262]: INFO [organizr-auth] Ignore 192.168.1.1 by ip
2017-08-08 21:53:25,578 fail2ban.filter [262]: INFO [organizr-auth] Found 104.160.20.131 - 2017-08-08 21:53:24
2017-08-08 21:53:31,617 fail2ban.filter [262]: INFO [organizr-auth] Found 104.160.20.131 - 2017-08-08 21:53:30
2017-08-08 21:53:36,650 fail2ban.filter [262]: INFO [organizr-auth] Found 104.160.20.131 - 2017-08-08 21:53:36
2017-08-08 21:53:42,688 fail2ban.filter [262]: INFO [organizr-auth] Found 104.160.20.131 - 2017-08-08 21:53:41
2017-08-08 21:53:48,726 fail2ban.filter [262]: INFO [organizr-auth] Found 104.160.20.131 - 2017-08-08 21:53:47
2017-08-08 21:53:48,733 fail2ban.actions [262]: NOTICE [organizr-auth] Ban 104.160.20.131
```

If you managed to ban yourself or a friend banned themself you can run one of these commands:

```text
fail2ban-client unban <ip>
#OR
docker exec letsencrypt fail2ban-client unban <ip>
```

Thanks to rix1337 for the fail2ban config:

* [organizr-auth.conf](https://github.com/rix1337/docker-organizr/blob/master/root/etc/fail2ban/filter.d/organizr-auth.conf)
* [jail.local](https://github.com/rix1337/docker-organizr/blob/master/root/defaults/jail.local)

