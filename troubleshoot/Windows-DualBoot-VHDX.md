[A SMARTER WAY TO DUAL BOOT WINDOWS...](https://www.youtube.com/watch?v=7JBFJuA5QsM)  
[How to Natively Boot a Windows 11 Virtual Hard Disk (VHDX)](https://www.ninjaone.com/blog/native-boot-vhdx-virtual-hard-disk/)  


# Windows Multi-Boot with virtual disks

## Check Drivers 
Device Manager
	NVIDIA GeForce MX250
	Realtek RTL8822CE 
	Realtek Audio

## Create virtual disk
With Disk Management  
Action > Create Virtual Disk  

	- C:\VHDX\Windows11.vhdx  
	- Size : 50 GB  
	- Format : VHDX  
	- Type : Dynamic  
	
## Restart with installation Media
During Windows Installation    
Shift+F10 to open cmd  

> c:  
> cd VHDX  
> diskpart  
	select vdisk file:c:\VHDX\Windows11.vhdx  
	list volume  
	attach vdisk  

## Setup after installation restart and choose to boot into Windows 11

	- Rename C: as Windows11 and D: as Windows10 (File Explorer)
	- Define default Windows  
  	sysdm.cpl : System Properties > Advanced  
  	Startup - Recovery to select default Windows version  

Adapt Boot Menu description  	

	bcdedit /enum  
	bcdedit /set {current} description "Windows 11 Gaming"  
	bcdedit /set {guid} description "Windows 11 Work"  
	
## To remove virtual Windows disk
	
	- Simply delete c:\vhdx\Windows11.vhdx  
	- msconfig > Boot : delete Windows11 Entry  


## To enlarge Virtual disk

> diskpart  
> select vdisk file=c:\VHDX\Windows11.vhdx  
> expand vdisk maximum=81920  
> attach vdisk  
> select volume 3  
> extend  
