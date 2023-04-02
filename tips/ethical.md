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

# Linux

## [VirtualBox](https://linuxhint.com/install-virtualbox-linux-mint/)  

`$ sudo apt-get install virtualbox virtualbox-ext-pack`  

## kali - config post installation  

```shell
sudo dpkg-reconfigure keyboard-configuration
sudo dpkg-reconfigure locales
sudo dpkg-reconfigure tzdata
```
