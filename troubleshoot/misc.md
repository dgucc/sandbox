# Misc 

## ufw : allow ssh in subnet

```bash
sudo ufw allow from 192.168.1.0/24 to any port 22
sudo ufw enable
sudo ufw status
```  

## Keyboard problem in linux mint 22  

Add pnpacpi=off in grub config :   
`sudo nano /etc/default/grub`  

> GRUB_CMDLINE_LINUX_DEFAULT="quiet splash pnpacpi=off"  
Plug and Play ACPI (Advanced Configuration and Power Interface)  

or  

> GRUB_CMDLINE_LINUX_DEFAULT="quiet splash pnpacpi=off i8042.notimeout"  
No time out to detect keyboard/mouse...  

`sudo update-grub`  

Reboot  


