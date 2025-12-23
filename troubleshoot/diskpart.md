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

## Virtual disks


```cmd
(as admin) > diskpart
select vdisk file:c:\VHDX\Windows11.vhdx
	list volume
	attach vdisk
```

