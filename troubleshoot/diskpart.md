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
