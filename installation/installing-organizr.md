# Installing Organizr

## Summary

blah

## Docker



## Windows



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

You may need to change the path to the socket depending on what version of PHP you installed

    3. Navigate to that path locally using your web browser and the host's local ip address. `http://localhost` or `http://192.168.1.###` You should be able to login and establish your admin account.

