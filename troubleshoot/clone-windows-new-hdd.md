# Clone Windows on new disk

[How to clone Windows 11/10 on new SSD/HDD with no software](https://www.youtube.com/watch?v=CDLyNJGzkWE)

## Create Windows backup with dism

`(as admin) > dism /capture-image /imagefile:D:\win10backup.wim /capturedir:C:\ /name:"Win10Backup" /compress:max /checkintegrity`


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

## Restaure Windows on new disk
Restaure the backup image :
`(as admin) > dism /apply-image /imagefile:win10backup.wim /index:1 /applydir:W:\`


## Activate the boot partition 
`(ad admin) > bcdboot w:\Windows`
