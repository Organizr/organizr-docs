
**Information & Background**

\*Warning\* FOR USE with Windows 10. So the process may vary for other OS.

**Implementation**

1. First You need to stop and delete the NGINX Service by Opening Services.msc there a multitude of ways to do this. You can look that up somewhere else.
2. Search for service named `NGINX` and select Stop
3. Right Click on service `NGINX`
4. Now im assuming you have NSSM installed in the Windows 10 Path if Google is your friend.
5. Open up a powershell as administrator
6. once loaded you can be in any directory if NSSM is installed in your Windows Path
7. Type command `nssm remove NGINX`
8. Now Click on the refresh in the Services.msc
9. You Should not be able to find the Service NGINX anymore. Congrats
10. Now Locate your Organizr Main directory. IF YOU USED THE DEFAULT DIRECTORY: `C:\nginx\
11. Inside your C:\Nginx directory locate your www directory. Move this to wherever you want your Caddyfile to read. IE. `C:\Tools\Organizr`**(THIS IS NOT NECCESARY BUT IF YOU LIKE YOUR DIRECTORIES TIDY You CAN DO THIS)**
12. Now edit the below caddy script to add your domain or subdomain and file locations.

Here is my Caddyfile Setup

    reverseproxy.website.com {
    
    root * C:\Your\Organizr\www\html
    
    php_fastcgi localhost:9000
    
    rewrite /api/v2/* /api/v2/index.php?{query}
    
    file_server
    
        tls {
            dns cloudflare (CLOUDFLARE-API-KEY)
        }
    }

**NOTE**

If you Do not Run Cloudflare with your reverse proxy you can remove the

&#x200B;

    tls {
            dns cloudflare (CLOUDFLARE-API-KEY)
        }
    }

Now load up your Powershell/CMD as administrator restart caddy however it is installed whether its a service or running manually.

Wam! Bam! Thank You Mam! You just natively are running Organizr on Caddy.
