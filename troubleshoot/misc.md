[Moving Windows Recovery Partition Correctly](https://thedxt.ca/2023/06/moving-windows-recovery-partition-correctly/)

- Disable Recovery Partition  
´´´
(as admin) > reagentc /disable  
´´´  
- C:\Windows\System32\Recovery\Winre.wim  
- Delete Recorery Partition with diskpart  
´´´
(as admin) > diskpart
list disk
select disk #
list partition
select partition #
delete partition override
´´´
