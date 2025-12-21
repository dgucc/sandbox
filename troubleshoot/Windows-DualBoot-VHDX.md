[A SMARTER WAY TO DUAL BOOT WINDOWS...](https://www.youtube.com/watch?v=7JBFJuA5QsM)
# Windows Multi-Boot with virtual disks

## Check Drivers 
Device Manager
	NVIDIA GeForce MX250
	Realtek RTL8822CE 
	Realtek Audio

## Create virtual disk
Disk Management : Action > Create Virtual Disk
	C:\VHDX\Windows11.vhdx
	Size : 50 GB
	Format : VHDX
	Type : Dynamic
	
## Restart with installation Media
Windows Setup > Advanced
cmd as admin : Shift+F10
> c:
> cd VHDX
> diskpart
	select vdisk file:c:\VHDX\Windows11.vhdx
	list volume
	attach vdisk

## After installation Restart and choose to boot into Windows 11

## Rename C: as Windows11 and D: as Windows10 (File Explorer)

## Define default Windows
  sysdm.cpl : System Properties > Advanced
	Startup - Recovery to select default Windows version
	
## To remove virtual Windows disk
	Delete c:\vhdx\Windows10.vhdx 
	msconfig > Boot : delete Windows10 Entry

## Adapt Boot Menu description	
	bcdedit /enum
	bcdedit /set {current} description "Windows for Gaming"
	bcdedit /set {guid} description "Windows for Work"

## Enlarge Virtual disk

> diskpart
> select vdisk file=c:\VHDX\Windows11.vhdx
> expand vdisk maximum=80000
> attach vdisk
> select volume 3
> extend
