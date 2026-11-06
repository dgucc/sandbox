# Repair EFI

```cmd
(as admin) > diskpart
list disk
select disk 0
list vol           # Identify Windows and EFI partitions
select vol 1       # Select EFI partition (100MB FAT32) 
assign letter s    # Assign letter to format it later on
list vol           # check v:
exit
```
Format corrupted EFI partition  

```cmd
s:
cd EFI\Microsoft\Boot   # verify it well the EFI partition
format s: /fs:FAT32
```

Recreate EFI

```cmd
c:
bcdboot c:\windows /s s: /f UEFI
```

Reboot ;-)

Notes : EFI recreated but loosing Recovery Partition

---

## Delete Windows Recovery Partition

[Moving Windows Recovery Partition Correctly](https://thedxt.ca/2023/06/moving-windows-recovery-partition-correctly/)

- Disable Recovery Partition  
> (as admin) > reagentc /disable  
- C:\Windows\System32\Recovery\Winre.wim  
- Delete Recorery Partition with diskpart  
> (as admin) > diskpart  
> list disk  
> select disk #  
> list partition  
> select partition #  
> delete partition override  

## Recreate Windows recovery partition

For GPT disk :  
- reagentc /info : Affiche l'état actuel de Windows RE (activé ou désactivé) ainsi que l'emplacement de l'image winre.wim.
- reagentc /disable : Désactive l'Environnement de récupération Windows.  

```cmd
diskpart
create partition primary size=500
set id=de94bba4-06d1-4d40-a16a-bfd50179d6ac
gpt attributes=0x8000000000000001
format quick fs=ntfs label="Recovery"
```

- reagentc /enable : Active l'Environnement de récupération Windows.  
- reagentc /setreimage /path <chemin> : Définit le chemin d'accès au fichier d'image de récupération Windows RE (winre.wim).  
