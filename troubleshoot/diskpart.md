# Diskpart

## Delete protected partition with diskpart


```cmd
(as admin) > diskpart
> list disk
> select disk X 
> list partition
> select partition Y
> delete partition override 
```
