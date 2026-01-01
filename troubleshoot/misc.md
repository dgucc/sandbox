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



## Windows Recovery Partition

[Moving Windows Recovery Partition Correctly](https://thedxt.ca/2023/06/moving-windows-recovery-partition-correctly/)

- Disable Recovery Partition  
> (as admin) > reagentc /disable  
- C:\Windows\System32\Recovery\Winre.wim  
- Delete Recorery Partition with diskpart  
> (as admin) > diskpart  
> list disk  
> select disk #  
> list partition  
> select partition #  
> delete partition override  

[TODO : How to recreate a recovery partition...]
