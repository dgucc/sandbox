# Multi Windows with virtual disk

## Main Windows installation from bootable usb

Windows 11 - 24H2  

Prepare a clean disk  
```cmd
(as admin) > diskpart
list disk
select disk 0
clean
convert gpt
```

Create efi partition  
```cmd
create partition efi size=200
format fs=fat32 quick label "efi"
```

Create Microsoft Reserved (MSR) partition  
```cmd
create partition msr size=128
```

Create Windows partition  
```cmd
create partition primary
format fs=ntfs quick label "host-disk"
assign letter c
list vol
```

Create and attach virtual disk
```cmd
create vdisk file=c:\Win11Work.vhdx maximum=81920
select vdisk file=c:\Win11Work.vhdx
attach vdisk
```

Back to Windows installation   
 - Choose "Install Windows 11"  
 - Previous Version of Setup  
 - Install Now  
 - Customised installation (advanced)  
 - Choose 80GB partition  
 - Follow installation procedure until reboot  


## Reboot to install another Windows for Gaming  

Create and attach another virtual disk
```cmd
(as admin) > diskpart
list vol
create vdisk file=c:\Win11Gaming.vhdx maximum=81920
select vdisk file=c:\Win11Gaming.vhdx
attach vdisk
exit
```

Back to Windows installation   
 - Choose "Install Windows 11"  
 - Previous Version of Setup  
 - Install Now  
 - Customised installation (advanced)  
 - Choose 80GB partition  
 - Follow installation procedure until reboot  

Define default OS  
	Run sysdm.cpl > Advanced

Rename OS in command line
	`bcdedit /set {current} "Windows 11 Gaming"`  
or by using EasyBCD


Enlarge virtual disk
```cmd
(as admin) > diskpart
select vdisk file=D:\Win11Gaming.vhdx
expand vdisk maximum 102400
attach vdisk
list volume
select volume 3
extend
exit
```


