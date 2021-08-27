# Ubuntu & Debian

## OUI \(Organizr Ubuntu Installer\) <a id="bkmrk-windows-here"></a>

#### Requirements

* Git \(`sudo apt-get install git`\)

#### Tested on

* Ubuntu 16.04
* Debian 9.5+

#### What does it do

* Installs Unzip, NGINX, and PHP
* Installs the required PHP modules
* Adds in a working NGINX conf with PHP block and sample app configs

### Installation Steps

1. Clone/Download the OUI folder from [https://github.com/elmerfdz/OrganizrInstaller](https://github.com/elmerfdz/OrganizrInstaller) - `git clone https://github.com/elmerfdz/OrganizrInstaller /opt/OrganizrInstaller`
2. Navigate to the OUI folder - `cd /opt/OrganizrInstaller/ubuntu/oui`
3. Run the installer script - `sudo bash ou_installer.sh`

{% hint style="info" %}
If you want to set it up without SSL, use the Let's Encrypt/Standard option
{% endhint %}

## Manual Install <a id="bkmrk-manual-install"></a>

{% hint style="info" %}
Important note for beginners
{% endhint %}

Organizr is a php based web front-end to help organize your services. Organizr itself is not a service, so do not think of this as another usenet application. It's a collection of files that live on your webserver to serve up existing content/services in a streamlined, organized way.

### This guide makes the following assumptions: <a id="bkmrk-this-guide-makes-the"></a>

* You are installing Organizr on the same host machine as your webserver
* You are using a debian or ubuntu based linux distro for an OS, and nginx as a webserver
* You are installing as the root user, or a user with \`sudo\` privelages \(use \`sudo\` where appropriate if you're not root\)

If you don't already have nginx installed as a webserver, run `apt-get install nginx` or [consult this guide](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04) for detailed setup.

### Dependencies <a id="bkmrk-dependencies%3A"></a>

There are a few packages that Organizr depends on to function. Some of them may already be installed with your OS, some may be missing, or some may require an upgraded version. This guide shows you how to use and install php7.1 and its packages, but Organizr also lets you use php7.0 or php7.2, if you prefer.

{% hint style="info" %}
If you plan on using LDAP for authenticating into Organizr, php7.1 or later is required
{% endhint %}

* [ ] **P**HP**D**ata **O**bjects \(PDO\) can be acquired through `php7.1-mysql`
* [ ] PDO SQLite, for which we'll use `php7.1-sqlite3`
* [ ] SQLite3, which is required for the in-site chat feature. \(`sqlite3`\)
* [ ] **F**ile **Open** \(fopen\) should already be installed with your version of php
* [ ] SimpleXML via `php7.1-xml`
* [ ] `php7.1-zip`
* [ ] OpenSSL via `openssl`, if it's not already installed with your OS
* [ ] `mail` for resetting passwords \(this is likely already be installed with your OS\)
* [ ] cURL for Plex and Emby logins \(`php7.1-curl`\)
* [ ] 
#### PHP

{% hint style="info" %}
If you do not have `php` already installed on your system, you'll need to add the repository and install the package
{% endhint %}

```text
apt-get install software-properties-common
add-apt-repository ppa:ondrej/php
apt-get update
apt-get install php7.1-fpm
```

Then, to be sure all of the Organizr's prerequisites are installed, run the following command with your package manager. Some of these may also require other dependencies, so select "Yes" to install those as well.

```text
apt-get install php7.1-mysql php7.1-sqlite3 sqlite3 php7.1-xml php7.1-zip openssl php7.1-curl
```

You now have the necessary prerequisites to install and use Organizr. Next you need to decide where you're going to install Organizr within your web directory. Most default nginx installations give you a path at `/var/www/html` to insert your website files. You _can_ install Organizr there, but to keep things organized \(in the event that you want more than one website\), we recommend `/var/www/websites/website_name.com/`. You can also install Organizr as a subdirectory, if you do not want Orgniazr being the default root of your domain. In that case, you would install it to `/var/www/websites/website_name.com/organizr`. For this guide, we'll assume your domain is named `roxinsocks.com` and that Organizr is running at the root level of the site.

### Installing Organizr <a id="bkmrk-installing-organizr"></a>

1. Navigate to your website path with `cd /var/www/websites/roxinsocks.com`
2. Using **one** of these two methods, grab the most recent Organizr build from github:

### Using Git <a id="bkmrk-using-git"></a>

`git clone https://github.com/causefx/Organizr /var/www/websites/roxinsocks.com`

You may need to install `git` if you don't have it installed: `apt-get install git`

### Using Zip <a id="bkmrk-using-zip"></a>

1. `wget` [`https://github.com/causefx/Organizr/archive/v2-master.zip`](https://github.com/causefx/Organizr/archive/v2-master.zip) You may need to install `wget` if you don't have it installed: `apt-get install wget`
2. Unzip the file with `unzip v2-master.zip -d /var/www/websites/roxinsocks.com`

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

