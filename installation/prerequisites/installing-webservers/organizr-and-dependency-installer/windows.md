---
description: Organizr & Dependency Installer for Windows
---

# Windows

## Summary

OWI \(Organizr Windows Installer\)

## Requirements

{% hint style="info" %}
Currently x64 bit OS only.
{% endhint %}

* Latest version of PowerShell, if you're on Windows 7/Win Server 2008 [download](https://social.technet.microsoft.com/wiki/contents/articles/21016.how-to-install-windows-powershell-4-0.aspx)
* Windows 10 recommended but it should work on Windows 7 if you have the latest version of PowerShell
* The user account running the installer should have admin privileges and a password set.

### OWI Tested on the following OS Versions:

* Windows 10 Pro \(Fall creators update\)
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

