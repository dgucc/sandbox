# Repair EFI

```cmd
(as admin) > diskpart
list disk
select disk 0
list vol           # Identify Windows and EFI partitions
select vol 1       # Select EFI partition (100MB FAT32) 
assign letter=v    # Assign letter to format it later on
list vol           # check v:
exit
```
Format corrupted EFI partition  

```cmd
v:
cd EFI\Microsoft\Boot   # verify it well the EFI partition
format v: /fs:FAT32
```

Recreate EFI
```cmd
c:
bcdboot c:\windows /s v: /f UEFI
```

Notes : EFI recreated but loosing Recovery Partition

