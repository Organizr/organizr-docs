# Nginx

## Summary

The first component needed for Organizr to run is a webserver.

{% tabs %}
{% tab title="Windows" %}
### Download and Install

1. Download Nginx from: [http://nginx.org/en/download.html](https://nginx.org/en/download.html)
2. Install Nginx to your preferred location
3. Copy the Nginx folder to your preferred location e.g. `c:\`
4. If you want to run Nginx as a service then skip to `Running Nginx as a service` section, if not continue.
5. Go to the location you copied the Nginx folder e.g. `c:\nginx`
6. Double click on `nginx.exe` in `c:\nginx` , nginx should now be running on your system
7. To verify, open a browser and type localhost and press enter. If you get "**Welcome to nginx!**” message then Nginx has been installed successfully
   1. **Note:** you would need to open 'nginx.exe' every time you reboot your system, to avoid this, install Nginx as a service.

### Running Nginx as a service

1. Download NSSM from: [https://nssm.cc/download](https://nssm.cc/download)
2. Copy the nssm.exe from the win32 or win64 folder depending on your system to `C:\Windows\System32`
3. Open `cmd` as admin, navigate to `C:\Windows\System32`
4. Type in this command `nssm install nginx`
   1. Path = `C:\nginx\nginx.exe`
   2. Startup directory = `C:\nginx`
   3. See image below for Example
5. Install service
6. Make sure you run the service as the admin account
   1. Open run and type in `services.msc`
   2. Search for the nginx service we just installed
   3. Double-click and go to the Log On tab
   4. Select ‘This account:’ and fill in your account details and then press ok.
   5. Right click on the nginx service and restart
7. Making your Nginx install PHP ready, uncomment the following code from your nginx.conf file `c:\nginx\conf\nginx.conf`

![](<../../../.gitbook/assets/image (57).png>)

```
location ~ \.php$ {
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```

{% hint style="info" %}
To verify, open a browser and type localhost and press enter. If you get "Welcome to nginx!” message then Nginx has been installed successfully
{% endhint %}

### Sample Config File

You can copy the following if you wish and replace the content in your `nginx.conf` file

```
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

##
{% endtab %}

{% tab title="Ubuntu/Debian" %}
### Download and Install

Run `apt-get install nginx` or [consult this guide](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04) for detailed setup.
{% endtab %}
{% endtabs %}

