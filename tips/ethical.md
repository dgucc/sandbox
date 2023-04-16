# Windows

## Add local admin

Add user and password :  
`> net user /add testuser super_password`  

Display localgroups names :   
`> net localgroup`  

Add new user as member of localadmins : 
`> net localgroup administrators testuser /add`  


## Wi-Fi

Show stored Wi-Fi passwords  

`> netsh wlan show profile`  
`> netsh wlan show profile <profileName> key-clear`  

## Registry

[HKLM]\Software\Microsoft\Windows\CurrentVersion\Run  
[HKLM]\Software\Microsoft\Windows\CurrentVersion\RunOnce  
[HKLM]\SYSTEM\CurrentControlSet\Services\  

---

# Linux

## [VirtualBox](https://linuxhint.com/install-virtualbox-linux-mint/)  

`$ sudo apt-get install virtualbox virtualbox-ext-pack`  

## kali - config post installation  

```shell
sudo dpkg-reconfigure keyboard-configuration
sudo dpkg-reconfigure locales
sudo dpkg-reconfigure tzdata
```

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

## Unbound - housekeeping

```bash
sudo service unbound stop
sudo wget ftp://FTP.INTERNIC.NET/domain/named.cache -O /var/lib/unbound/root.hints
sudo service unbound start
```

