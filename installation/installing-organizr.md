# Installing Organizr

## Summary

blah

## Docker

### Installing via CLI

```text
docker create \
  --name=organizr \
  -v <path to data>:/config \
  -e PGID=<gid> -e PUID=<uid>  \
  -p 80:80 \
  -e fpm="false" \ # optional
  -e branch="v2-master" \ # optional
  organizr/organizr
```

### Installing via Compose File

```text
version: "3.6"
services:
    organizr:
        container_name: organizr
        hostname: organizr
        image: organizr/organizr:latest
        restart: unless-stopped
        ports:
            - 80:80
        volumes:
            - <path to data>:/config
        environment:
            - PUID=<uid>
            - PGID=<gid>
            - TZ=<timezone>
```

### More Information

 Head over to [https://github.com/Organizr/docker-organizr](https://github.com/Organizr/docker-organizr) to see more information.

## Windows

### Pre-Check

{% hint style="info" %}
Make sure you have setup Nginx and PHP
{% endhint %}

{% page-ref page="prerequisites/installing-webservers/nginx.md" %}

{% page-ref page="prerequisites/installing-php.md" %}

{% hint style="warning" %}
Make sure you have enabled php\_pdo\_sqlite.dll & php\_openssl.dll PHP extensions.
{% endhint %}

### Download Organizr

1. [Download](https://github.com/causefx/Organizr/) the latest release of Organizr.
2. Open the downloaded organizr zip file and copy all files and paste them in the web root folder `c:\nginx\html\`
   1. OR If you prefer you can create sub-directory called organizr under `c:\nginx\html` and paste the copied organizr files in that folder.
3. Go to `http(s)://localhost/index.php`

{% hint style="info" %}
You may use this Nginx config file if you would like
{% endhint %}

```text
#user  nobody;
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        #CHANGE THESE LINES##########
        server_name  localhost;
        root   html/Organizr;
        #############################
        index  index.php index.html index.htm;
        error_page 400 401 403 404 405 408 500 502 503 504  /?error=$status;
        location / { }
        location ~ \.php$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
        location /api/v2 {
	        try_files $uri /api/v2/index.php$is_args$args;
        }
    }
}
```

## Ubuntu & Debian

### Navigate to Webserver Directory <a id="bkmrk-installing-organizr"></a>

1. Navigate to your website path with `cd /var/www/websites/roxinsocks.com`
   1. Replace the domain path in the webserver path with the correct path
2. Using **one** of the following two methods, grab the most recent Organizr build from github:

{% tabs %}
{% tab title="Git" %}
Copy this command and paste into your terminal

```text
git clone https://github.com/causefx/Organizr /var/www/websites/roxinsocks.com
```

{% hint style="info" %}
You may need to install `git` if you don't have it installed: `apt-get install git`
{% endhint %}
{% endtab %}

{% tab title="Zip" %}
Copy this command and paste into your terminal

```
wget https://github.com/causefx/Organizr/archive/v2-master.zip
```

{% hint style="info" %}
You may need to install `wget` if you don't have it installed: `apt-get install wget`
{% endhint %}

Unzip the file with the following command while replacing the file path with the location to your servers domain files

```text
unzip v2-master.zip -d /var/www/websites/roxinsocks.com
```
{% endtab %}
{% endtabs %}

All your Organizr files are now installed at `/var/www/websites/roxinsocks.com/`

### Permissions & Access <a id="bkmrk-permissions-%26-access"></a>

1. Set the permission to your path, so that Organizr can write to it by running `chown -R www-data:www-data /var/www/websites/roxinsocks.com/`
2. For external access and functionality, edit your nginx sites-enabled config file for your domain \(`nano /etc/nginx/sites-enabled/roxinsocks.com`\), and be sure the `root` is set correctly in the server block. This will tell nginx where to look for organizr, when you navigate to your domain:

```text
server{
    root /var/www/websites/roxinsocks.com;
    index index.php index.html index.htm index.nginx-debian.html;
    server_name roxinsocks.com;
    location / { try_files $uri $uri/ =404; }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7-fpm.sock;
    }
    location /api/v2 {
	    try_files $uri /api/v2/index.php$is_args$args;
    }
}
```

{% hint style="warning" %}
You may need to change the path to the socket depending on what version of PHP you installed
{% endhint %}

    3. Navigate to that path locally using your web browser and the host's local ip address. `http://localhost` or `http://192.168.1.###` You should be able to login and establish your admin account.

## Helm

 Our helm chart is maintained by the guys over at [k8s@home.](https://github.com/k8s-at-home/charts/) This uses the official docker container.

### **Links**

| Repo | Link |
| :--- | :--- |
| Chart Github Repository | [k8s-at-home/charts/organizr](https://github.com/k8s-at-home/charts/tree/master/charts/organizr) |
| Chart Helm Repository | [k8s-at-home](https://k8s-at-home.com/charts/) |
| Artifacthub | [k8s-at-home/organizr](https://artifacthub.io/packages/helm/k8s-at-home/organizr) |

### TL;DR

```text
helm repo add k8s-at-home https://k8s-at-home.com/charts
helm install organizr k8s-at-home/organizr --values values.yaml # User supplied
```

### **Installing**

1.  Add the helm repository for k8s-at-home
2. Read through the values.yaml file either in the github repository or via helm commands
3. Deploy a named release with your override values.yaml file

### **Example Commands**

```text
helm repo add k8s-at-home https://k8s-at-home.com/charts
# these next 2 lines are convenience lines to build a full values file for modification. 
# You can construct your own overrides as you see fit.
helm show values k8s-at-home/organizr | \
    sed '1,2d;/service/,+1d' > values.yaml
helm show values k8s-at-home/media-common | \
    sed '1d;/image:/,+5d;s/port: ""/port: 80/;s/^/  /' >> values.yaml
vi values.yaml # modify as needed
helm install organizr k8s-at-home/organizr --values values.yaml
```

### Example values.yaml override

```text
organizr:
  imagePullSecrets: []
  fullnameOverride: organizr
  
  env:
    TZ: UTC
  
  ingress:
    enabled: true
    annotations:
      kubernetes.io/ingress.class: traefik
      traefik.ingress.kubernetes.io/router.entrypoints: websecure
      traefik.ingress.kubernetes.io/router.priority: "10"
      cert-manager.io/cluster-issuer: letsencrypt-prod
    hosts:
      - host: organizr.domain.tld
        paths:
          - /
    tls:
      - secretName: organizr-domain-tld
        hosts:
          - organizr.domain.tld
  
  persistence:
    # type: options are statefulset or deployment
    type: statefulset
    config:
      enabled: true
  
  resources:
    requests:
      cpu: 100m
      memory: 128Mi
```

