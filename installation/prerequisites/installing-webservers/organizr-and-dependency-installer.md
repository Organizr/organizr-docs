# Organizr & Dependency Installer

{% tabs %}
{% tab title="Windows" %}
## OWI (Organizr Windows Installer) <a href="#bkmrk-windows-here" id="bkmrk-windows-here"></a>

![](https://camo.githubusercontent.com/4324aec22434796df57feea80536aac768306c90a28543dbd425603fa7d8eeb2/68747470733a2f2f692e696d6775722e636f6d2f4e3675395837642e706e67)

## Requirements

{% hint style="info" %}
Currently x64 bit OS only.
{% endhint %}

* Latest version of PowerShell, if you're on Windows 7/Win Server 2008 [download](https://social.technet.microsoft.com/wiki/contents/articles/21016.how-to-install-windows-powershell-4-0.aspx)
* Windows 10 recommended but it should work on Windows 7 if you have the latest version of PowerShell
* The user account running the installer should have admin privileges and a password set.

### OWI Tested on the following OS Versions:

* Windows 10 Pro (Fall creators update)
* Windows Server 2012 R2

### What does it do?

* Downloads Nginx, PHP, NSSM and Visual C++ Redistributable
* Creates services for Nginx and PHP
* Downloads Organizr
* Configures PHP as per Org requirements
* Adds in a working Nginx conf file with PHP block enabled

### Steps

1. Clone\Download the OWI folder from: [https://github.com/elmerfdz/OrganizrInstaller](https://github.com/elmerfdz/OrganizrInstaller)
2. Extract the zip file to your `desktop`
3. Navigate to `\OrganizrInstaller\windows\owi`
4. Right-click on `owi_installer.bat` and click on `Run as administrator`
5. Installer will ask you for the nginx install location, type in the full path as per the e.g. `c:\nginx`
6. The installer will ask you to provide the password of the current user during installation, the nginx service requires that you run it under a user account instead of the 'Local System' account, if you don't then you won't be able to save and reload your nginx config
{% endtab %}

{% tab title="Ubuntu/Debian" %}
## OUI (Organizr Ubuntu Installer) <a href="#bkmrk-windows-here" id="bkmrk-windows-here"></a>

![](https://camo.githubusercontent.com/710edb7402bba5237fa129dd448030b2ee75896e11fc9065af8168fe6fa3411d/68747470733a2f2f692e696d6775722e636f6d2f72396c616a7a572e706e67)

## Requirements

* Git (`sudo apt-get install git`)

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

## Manual Install <a href="#bkmrk-manual-install" id="bkmrk-manual-install"></a>

{% hint style="info" %}
Important note for beginners
{% endhint %}

Organizr is a php based web front-end to help organize your services. Organizr itself is not a service, so do not think of this as another usenet application. It's a collection of files that live on your webserver to serve up existing content/services in a streamlined, organized way.

### This guide makes the following assumptions: <a href="#bkmrk-this-guide-makes-the" id="bkmrk-this-guide-makes-the"></a>

* You are installing Organizr on the same host machine as your webserver
* You are using a debian or ubuntu based linux distro for an OS, and nginx as a webserver
* You are installing as the root user, or a user with \`sudo\` privelages (use \`sudo\` where appropriate if you're not root)

If you don't already have nginx installed as a webserver, run `apt-get install nginx` or [consult this guide](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04) for detailed setup.

### Dependencies <a href="#bkmrk-dependencies-3a" id="bkmrk-dependencies-3a"></a>

There are a few packages that Organizr depends on to function. Some of them may already be installed with your OS, some may be missing, or some may require an upgraded version. This guide shows you how to use and install php7.1 and its packages, but Organizr also lets you use php7.0 or php7.2, if you prefer.

{% hint style="info" %}
If you plan on using LDAP for authenticating into Organizr, php7.1 or later is required
{% endhint %}

* [ ] **P**HP**D**ata **O**bjects (PDO) can be acquired through `php7.1-mysql`
* [ ] PDO SQLite, for which we'll use `php7.1-sqlite3`
* [ ] SQLite3, which is required for the in-site chat feature. (`sqlite3`)
* [ ] **F**ile **Open** (fopen) should already be installed with your version of php
* [ ] SimpleXML via `php7.1-xml`
* [ ] `php7.1-zip`
* [ ] OpenSSL via `openssl`, if it's not already installed with your OS
* [ ] `mail` for resetting passwords (this is likely already be installed with your OS)
* [ ] cURL for Plex and Emby logins (`php7.1-curl`)
*

#### PHP

{% hint style="info" %}
If you do not have `php` already installed on your system, you'll need to add the repository and install the package
{% endhint %}

```
apt-get install software-properties-common
add-apt-repository ppa:ondrej/php
apt-get update
apt-get install php7.1-fpm
```

Then, to be sure all of the Organizr's prerequisites are installed, run the following command with your package manager. Some of these may also require other dependencies, so select "Yes" to install those as well.

```
apt-get install php7.1-mysql php7.1-sqlite3 sqlite3 php7.1-xml php7.1-zip openssl php7.1-curl
```

You now have the necessary prerequisites to install and use Organizr. Next you need to decide where you're going to install Organizr within your web directory. Most default nginx installations give you a path at `/var/www/html` to insert your website files. You _can_ install Organizr there, but to keep things organized (in the event that you want more than one website), we recommend `/var/www/websites/website_name.com/`. You can also install Organizr as a subdirectory, if you do not want Orgniazr being the default root of your domain. In that case, you would install it to `/var/www/websites/website_name.com/organizr`. For this guide, we'll assume your domain is named `roxinsocks.com` and that Organizr is running at the root level of the site.

### Installing Organizr <a href="#bkmrk-installing-organizr" id="bkmrk-installing-organizr"></a>

1. Navigate to your website path with `cd /var/www/websites/roxinsocks.com`
2. Using **one** of these two methods, grab the most recent Organizr build from github:

### Using Git <a href="#bkmrk-using-git" id="bkmrk-using-git"></a>

`git clone https://github.com/causefx/Organizr /var/www/websites/roxinsocks.com`

You may need to install `git` if you don't have it installed: `apt-get install git`

### Using Zip <a href="#bkmrk-using-zip" id="bkmrk-using-zip"></a>

1. `wget` [`https://github.com/causefx/Organizr/archive/v2-master.zip`](https://github.com/causefx/Organizr/archive/v2-master.zip) You may need to install `wget` if you don't have it installed: `apt-get install wget`
2. Unzip the file with `unzip v2-master.zip -d /var/www/websites/roxinsocks.com`

All your Organizr files are now installed at `/var/www/websites/roxinsocks.com/`

### Permissions & Access <a href="#bkmrk-permissions-26-access" id="bkmrk-permissions-26-access"></a>

1. Set the permission to your path, so that Organizr can write to it by running `chown -R www-data:www-data /var/www/websites/roxinsocks.com/`
2. For external access and functionality, edit your nginx sites-enabled config file for your domain (`nano /etc/nginx/sites-enabled/roxinsocks.com`), and be sure the `root` is set correctly in the server block. This will tell nginx where to look for organizr, when you navigate to your domain:

```
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

&#x20;   3\. Navigate to that path locally using your web browser and the host's local ip address. `http://localhost` or `http://192.168.1.###` You should be able to login and establish your admin account.
{% endtab %}

{% tab title="CentOS" %}
## OCI (Organizr CentOS Installer)

![](https://camo.githubusercontent.com/76652c658e3fa6fa812b9f498307ed15dafd4ad155723273f97a1377019b8fae/68747470733a2f2f692e696d6775722e636f6d2f376e536e41586c2e706e67)

## Requirements

* Git (`sudo yum install git`)

#### Tested on version?

* CentOS 7

### How do I run it?

1. `sudo yum install git`
2. `sudo git clone https://github.com/elmerfdz/OrganizrInstaller /opt/OrganizrInstaller`
3. `cd /opt/OrganizrInstaller/centos/oci`
4. `sudo bash oc_installer.sh`
{% endtab %}
{% endtabs %}

