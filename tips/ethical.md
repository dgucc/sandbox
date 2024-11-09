<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Windows](#windows)
   * [Add local admin](#add-local-admin)
   * [diskpart](#diskpart)
   * [Bitlocker](#bitlocker)
   * [How to Identify Username and UserSid](#how-to-identify-username-and-usersid)
   * [Wi-Fi](#wi-fi)
   * [Registry](#registry)
   * [Windows Defender Firewall with Advanced Security](#windows-defender-firewall-with-advanced-security)
- [Linux](#linux)
   * [VirtualBox  ](#virtualbox)
   * [kali - config post installation  ](#kali-config-post-installation)
   * [Proxy Squid](#proxy-squid)
   * [Unbound - housekeeping](#unbound-housekeeping)
   * [Protect USB Drive ](#protect-usb-drive)
   * [Unquoted Service Path](#unquoted-service-path)
   * [How To get rid of UAC pop-up](#how-to-get-rid-of-uac-pop-up)
-  [Misc](#misc)
  * [Google Dorks](#google-dorks)
  
<!-- TOC end -->

<!-- TOC --><a name="windows"></a>
# Windows

ALT+F10 : open cmd from recovery login screen

<!-- TOC --><a name="add-local-admin"></a>
## Add local admin

Add user and password :  
`> net user /add testuser super_password`  

Display localgroups names :   
`> net localgroup`  

Add new user as member of localadmins : 
`> net localgroup administrators testuser /add`  

Reboot :  
`> wpeutil reboot`  

<!-- TOC --><a name="diskpart"></a>
## diskpart

Display volumes :  
`DISKPART> list volume`  

Mount volume and assign letter :  
`DISKPART> select volume <VolumeNumber>`  

<!-- TOC --><a name="bitlocker"></a>
## Bitlocker

`> manage-bde status`  

Unlock with Recovery Code :  
`> manage-bde -Unlock -RecoveryPassword <XXXX>`    


<!-- TOC --><a name="how-to-identify-username-and-usersid"></a>
## How to Identify Username and UserSid
Get All Names and SID
`> wmic useraccount get name,sid`  

Get Name with SID  
`> wmic useraccount where sid="S-1-5-21-992878714-4041223874-2616370337-1001" get name`  

Get SID with Name  
`> wmic useraccount where name="Alice" get sid`

<!-- TOC --><a name="wi-fi"></a>
## Wi-Fi

Show stored Wi-Fi passwords  

`> netsh wlan show profile`  
`> netsh wlan show profile <profileName> key-clear`  

<!-- TOC --><a name="registry"></a>
## Registry

[HKLM]\Software\Microsoft\Windows\CurrentVersion\Run  
[HKLM]\Software\Microsoft\Windows\CurrentVersion\RunOnce  
[HKLM]\SYSTEM\CurrentControlSet\Services\  

<!-- TOC --><a name="windows-defender-firewall-with-advanced-security"></a>
## Windows Defender Firewall with Advanced Security

`control firewall.cpl` or `wf.msc`  

Deactivate/Activate command :  

(as admin)
`netsh advfirewall set domainprofile state off`    

(as admin)
`netsh advfirewall set domainprofile state on`  

---

<!-- TOC --><a name="linux"></a>
# Linux

<!-- TOC --><a name="virtualbox"></a>
## [VirtualBox](https://linuxhint.com/install-virtualbox-linux-mint/)  

`$ sudo apt-get install virtualbox virtualbox-ext-pack`  

<!-- TOC --><a name="kali-config-post-installation"></a>
## kali - config post installation  

```shell
sudo dpkg-reconfigure keyboard-configuration
sudo dpkg-reconfigure locales
sudo dpkg-reconfigure tzdata
```

<!-- TOC --><a name="proxy-squid"></a>
## Proxy Squid

[Installer un proxy Squid et un filtrage avec SquidGuard sous Debian](https://memo-linux.com/installer-un-proxy-squid-et-un-filtrage-avec-squidguard-sous-debian/)  

Blacklist : http://dsi.ut-capitole.fr/blacklists/   
Mettre en place une tache planifiée pour la mise à jour de la blacklist

Créer un script :

`nano updateblacklist`  

```bash
#!/bin/bash
cd /tmp
wget http://dsi.ut-capitole.fr/blacklists/download/blacklists.tar.gz
tar -xzf blacklists.tar.gz
cp -rf blacklists/* /var/lib/squidguard/db/
rm -Rf blacklists*
squidGuard -C all
service squid3 restart
```

Rendre le script éxécutable :  

`chmod +x updateblacklist`  

Ajouter le script dans cron.weekly :  

`mv updateblacklist /etc/cron.weekly/`  

<!-- TOC --><a name="unbound-housekeeping"></a>
## Unbound - housekeeping

```bash
sudo service unbound stop
sudo wget ftp://FTP.INTERNIC.NET/domain/named.cache -O /var/lib/unbound/root.hints
sudo service unbound start
```

<!-- TOC --><a name="protect-usb-drive"></a>
## Protect USB Drive 

[cryptsetup](https://www.linuxtricks.fr/wiki/cryptsetup-creer-une-cle-ou-un-disque-usb-chiffree) TO TEST  

[Encrypt your Flash Drive on Ubuntu/Linux Mint](https://www.noobslab.com/2012/03/encrypt-your-flash-drive-on-ubuntulinux.html) TO TEST  

<!-- TOC --><a name="unquoted-service-path"></a>
## Unquoted Service Path

`wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """`  

[Windows Privilege Escalation](https://medium.com/@SumitVerma101/windows-privilege-escalation-part-1-unquoted-service-path-c7a011a8d8ae)

<!-- TOC --><a name="how-to-install-any-software-without-admin-rights"></a>
## How To get rid of UAC pop-up

via a batch file :  
```cmd
set __COMPAT_LAYER=RunAsInvoker
start SteamSetup.exe
```
or
`cmd /min /c set __COMPAT_LAYER=RunAsInvoker && start "" %1`  

# Misc

## Google Dorks

[Google Dorks / Google Hacking : Google au service des hackers](https://www.securiteinfo.com/attaques/hacking/google-dorks-et-google-hacking.shtml)  

- intitle
> intitle:"Pi-hole Admin Console"   
> intitle:"Netgear™ - NETGEAR Configuration Manager Login" : cible les configuration de routeurs Netgear qui sont en ligne.  
> intitle:"index of" .env : trouve des mots de passe de base de données Mysql  
> intitle:"Yawcam" inurl:8081 : rechercher des caméras en ligne  

- inurl
> inurl:/login.rsp : cible les webcams en ligne   
> inurl:/phpMyAdmin/setup/index.php?phpMyAdmin= : cible des zones d'administration de PHPMyAdmin  
> inurl:pipermail filetype:txt : permet d'accéder à des mails stockés en ligne  

- filetype
> filetype:txt "Registration Code" : rechercher des numéros d'enregistrements de logiciels payants.
> filetype:txt "License Key" : la même chose que précédemment
> filetype:png | "proportal" : cible des pages d'authentification du logiciel proportal  
> "database_password" filetype:yml "config/parameters.yml" : la même chose que précédemment
- ...
