# Reverse Proxies

## Summary

A reverse proxy is a server that sits in front of web servers and forwards client \(e.g. web browser\) requests to those web servers. Reverse proxies are typically implemented to help increase security, performance, and reliability.

## OWI <a id="bkmrk-page-title"></a>

The NGINX configs that come with the automated Organizr installer for Windows are set up with a couple files to make settings up your reverse proxies easier. This guide assumes you used the default `C:\nginx`, If you are not using the default path, adjust what this says to the path you used. Inside that folder, in the `conf` folder, there are two files: `rp-subfolder.conf` and `rp-subdomain.conf`. These are the files we are going to be working with. We're also going to use the example configs that can be found [here](https://github.com/organizrTools/Config-Collections-for-Nginx) \(this link is also found in those files\)

### **Subfolders**

 For configs that are just a location block, you are going to put them inside the `rp-subfolder.conf` file. We're going to use Sonarr as an example. Using these will create a reverse proxy that looks like `http://domain.com/app`

```text
location /sonarr {
        proxy_pass http://127.0.0.1:8989/sonarr;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_no_cache $cookie_session;
        # Allow the Sonarr API through if you enable Auth on the block above
        location /sonarr/api { 
                auth_request off;
                proxy_pass http://127.0.0.1:8989/sonarr/api;
        }
}
```

Things that may need changed in this:

```text
proxy_pass http://127.0.0.1:8989/sonarr/api;
```

If your Sonarr isn't running on the same machine, you will need to change out the 127.0.0.1 to the IP of the machine where it is running. If you are running it on a non-standard port this is also where to change it. Make sure to change it in both places. 

* If you are changing the location to something else, make sure it too is changed in both places.

### Subdomains

 For configs that are a server block, you are going to put them inside the `rp-subdomain.conf` file. We're going to use Sonarr as an example as well. Using these will create a reverse proxy that looks like `http://app.domain.com`

```text
server {
        listen 443 ssl;
        server_name sonarr.DOMAIN.TLD;

        ssl_certificate /etc/letsencrypt/live/DOMAIN.TLD/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/DOMAIN.TLD/privkey.pem;

        ssl_session_cache builtin:1000 shared:SSL:10m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        access_log /var/log/nginx/sonarr.access.log;

        location / {
                proxy_pass http://127.0.0.1:8989;
                proxy_set_header X-Forwarded-Host $server_name;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-Proto $scheme;
        }
}
```



Things that may need changed in this:

* Same as the subfolder, `proxy_pass` [`http://127.0.0.1:8989;`](http://127.0.0.1:8989/sonarr;) will need to be if your Sonarr isn't running on the same machine, you will need to change out the 127.0.0.1 to the IP of the machine where it is running. If you are running it on a non-standard port this is also where to change it.
* This example is an SSL server block, if you are just running on HTTP, the `listen 443 ssl;` will need to be changed and the lines that begin with ssl will need to be removed.
* The `ssl_certificate` and `ssl_certificate_key` need to be changed to where your certs are. I recommend copying them out of the `nginx.conf` file.
* If you get:`[emerg] 17144#3272: the size 10485760 of shared memory zone "SSL" conflicts with already declared size 52428800` remove or comment out the `ssl_session_cache builtin:1000 shared:SSL:10m;` line

### **Updating SSL Certificates**

If you are adding a new subdomain and are using SSL certificates, and didn't already plan ahead when running the installer or use a wildcard certificate, you will need to update the certificate to have the new subdomains. To make this easier, in `C:\nginx\winacme` there is a batch script called `owi_sslupdater.bat`, like the installer, that just does the certificates and won't make you reinstall the whole thing. Run through the prompts to update your certificates. You shouldn't need to do the next step if you're doing this because it already reloads NGINX.

### **Reloading the NGINX config**

Once you've made all your changes, you need to tell NGINX to reload the config. You can also test the config before reloading it. Open up a command prompt window as admin and do the following:

```text
cd c:\nginx
nginx -t
nginx -s reload
```

The first command after changing into the nginx directory tests the config. This should come back with `syntax is ok`. If it doesn't, don't run the next command. After running those commands, the reverse proxies should be accessible.

### **Miscellaneous Errors**

If you do run into an error when testing the config, it will usually tell you the line on which the syntax error is. If it is related to a curly brace, that line is not always accurate because it is giving the line where it found something it shouldn't and not necessarily where your mistake actually was if you missed a closing curly brace. 

The other most common error that we see is `nginx: [emerg] could not build server_names_hash, you should increase server_names_hash_bucket_size: 32` If you see this error, add `server_names_hash_bucket_size 64;` to the top of your `rp-subdomain.conf` file. Depending on how long your domain/subdomains are, the 64 may need to be increased. 

