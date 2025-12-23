# Clone Windows on new disk

[How to clone Windows 11/10 on new SSD/HDD with no software](https://www.youtube.com/watch?v=CDLyNJGzkWE)


Windows restart in rescue mode  
Shift + Restart -> Open Command prompt  

## Create Windows backup with dism

`(as admin) > dism /capture-image /imagefile:D:\win10backup.wim /capturedir:C:\ /name:"Win10Backup" /compress:max /checkintegrity`

## Prepare new disk

Reboot Windows  
Open Disk Manager  
Choose GPT as partition style in Diaglog (for new HDD)  


## Create EFI partition on new disk
```
(as admin) > diskpart 
list disk
select disk 0
create partition efi size=512
format fs=fat32 quick label="System Boot"
assign letter S
```

## Create new partition for windows
Use Disk Manager to create new partition for windows  
assign letter W:  

## Restore Windows on new disk
Restore the backup image :
`(as admin) > dism /apply-image /imagefile:win10backup.wim /index:1 /applydir:W:\`


## Activate the boot partition 
`(ad admin) > bcdboot w:\Windows`

Restart > BIOS : select new HDD  
