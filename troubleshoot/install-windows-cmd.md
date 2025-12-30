# Installing Windows using CMD. (GPT Disk)
[Reference](https://gist.githubusercontent.com/Alee14/e8ce6306a038902df6e7a6d667544ac9/raw/00fc5ad29c5d1595d266b0bbd043f143fa58f50c/win-dism.md)

## Disable secured boot in BIOS

Reboot windows installation media (Ventoy + ISO)  

## Open CMD
During setup screen : open cmd
`Shift + F10` 

## Creating Partition

### UEFI
```cmd
diskpart
list disk
select disk 0 # number for main disk
clean # Clearing all partitions
convert gpt
create partition efi size=512
list partition
format fs=fat32 quick
assign letter S
list vol
-----------------------
create part primary size=102400
format fs=ntfs quick label Windows 
assign letter W
list vol
exit
```
Tip : Use notepad to identify drive of installation media  

## Go to install.wim directory
```
G: # letter of installation disk
cd Sources
```

## List indexes of SKUs
Listing SKUs like Home, Pro, Education, Ultimate, etc.  

`dism /get-wiminfo /wimfile:install.wim`  

## Deploying WIM file
Copies the content from the install.wim file to the main disk.  

`dism /apply-image /imagefile:install.wim /index:1 /applydir:W:`  

## Creating boot files

Tip : Use notepad to verify drive letters  

### MBR + UEFI
`bcdboot W:\Windows /s S: /f uefi`  

## Reboot
`wpeutil reboot`

## Bypassing the OOBE to create local account 
Shift + F10 to open cmd  
cd Sources folder et type   
`oobe\bypassnro` or `oobe\bypassnroCMD`

Option "no internet connection" visible  
Able to create local user  

## Follow installation process

---
## Post installation

### Activation  

(powershell as admin) : `irm https://get.activated.win | iex`    
irm : invoke request method  
iex : invoke expression  

### Disable password expiration
`(as admin) > wmic UserAccount Set PasswordExpires=False`

### Disable hibernation
`(as admin) > powercfg /h off`    

### Disable bitlocker

---
## Add user (When the boot status says "Getting Ready")
```cmd
net user /add john password1234
net localgroup users /add john # in case Windows didn't add the users group
net localgroup administrators /add john
```

