# Diskpart

## Delete protected partition with diskpart

delete partition **override**  

```cmd
(as admin) > diskpart
> list disk
> select disk X 
> list partition
> select partition Y
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
