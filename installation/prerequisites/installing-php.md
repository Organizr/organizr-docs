# Installing PHP

## Summary

The second component needed for Organizr to run is PHP.

## Windows

### Download and Install

1. Download PHP for Windows from here: [http://windows.php.net/download](https://windows.php.net/download) \(Non Thread Safe version used in this guide\)
2. Create a folder called PHP under your Nginx directory e.g. `C:\nginx\php` and copy the downloaded files to this folder

### Running PHP as a service

1. Install NSSM - Skip to Step 2 if already installed
   1. Download NSSM from: [https://nssm.cc/download](https://nssm.cc/download)
   2. Copy the nssm.exe from the win32 or win64 folder depending on your system to `C:\Windows\System32`
2. If you’ve got `nssm` already setup, open command prompt as admin.
3. Type in the following `cmd nssm install php`
   1. Path = `C:\nginx\php\php-cgi.exe`
   2. Startup directory = `C:\nginx\php`
   3. Arguments = `-b 127.0.0.1:900`
   4. See image below for Example
4. Install Service
5. On the opened cmd prompt type in `nssm start php` to start the PHP service.
6. If the installed PHP service doesn’t start, then try manually running the `php-cgi.exe` file in `C:\nginx\php\` 
   1.  If you get a missing ‘VCRUNTIME’ related error then follow the solution on this link: [http://stackoverflow.com/questions/30811668/php-7-missing-vcruntime140-dll](https://stackoverflow.com/questions/30811668/php-7-missing-vcruntime140-dll)
7. Make a copy of one of the `php.ini-development` or `php.ini-production` files and rename it to `php.ini`
8. Open the php.ini file and search for the following and uncomment each:
   1. extension\_dir = "ext"
   2. extension=php\_openssl.dll
   3. extension=php\_pdo\_sqlite.dll
   4. extension=php\_curl.dll
   5. extension=php\_sqlite3.dll
9. Please note that if you are running PHP 7.2 or higher, look for the below lines and uncomment them instead:
   1. extension\_dir = "ext"
   2. extension=openssl
   3. extension=pdo\_sqlite
   4. extension=curl
   5. extension=sqlite3
10. Also, uncomment the following line and add `ext` to the end of it:
    1. sqlite3.extension\_dir =
       1. So that is becomes: `sqlite3.extension_dir = ext`
11. On the opened `cmd`prompt type in `nssm restart php` to restart the PHP service to apply the changes in `php.ini`.

![](../../.gitbook/assets/image%20%2854%29.png)

## Ubuntu & Debian

### Download and Install

#### Add the repository

```text
apt-get install software-properties-common
add-apt-repository ppa:ondrej/php
apt-get update
```

#### Install

```text
apt-get install php7.1-fpm
```

Then, to be sure all of the PHP packages are installed, run the following command with your package manager. Some of these may also require other dependencies, so select "Yes" to install those as well.

```text
apt-get install php7.1-mysql php7.1-sqlite3 sqlite3 php7.1-xml php7.1-zip openssl php7.1-curl
```

