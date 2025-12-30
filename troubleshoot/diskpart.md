# Diskpart

**diskpart** : manage computer’s drives (discs, partitions, volumes, or virtual hard drives).

## Create primary partition
```cmd
create part primary size=102400
format fs=ntfs quick label Data 
assign letter D
list vol
```

## Delete all partition
```cmd
list disk
select disk 0 
clean # Clearing all partitions
```

## Convert disk from MBR to GPT 
```cmd
list disk
select disk 0 # number for main disk
convert gpt
```

## Delete protected partition with diskpart

delete partition **override**  

```cmd
(as admin) > diskpart
> list disk
> select disk 0 
> list partition
> select partition 2
> delete partition override 
```

## Create EFI partition on new disk

```cmd
(as admin) > diskpart 
list disk
select disk 0
create partition efi size=512
format fs=fat32 quick label="System Boot"
assign letter S
```

## Attach a virtual disk

```cmd
(as admin) > diskpart
select vdisk file:c:\VHDX\Windows11.vhdx
	list volume
	attach vdisk
```

## Enlarge Virtual disk

```cmd
(as admin) > diskpart  
> select vdisk file=c:\VHDX\Windows11.vhdx  
> expand vdisk maximum=80000  
> attach vdisk  
> select volume 3  
> extend
```
