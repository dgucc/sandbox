## Free up disk space
[12 best ways to free up hard drive space on Windows 10](https://www.windowscentral.com/best-ways-to-free-hard-drive-space-windows-10)  

## Keyboard layout
powershell as admin  
`PS > Set-WinUserLanguageList -LanguageList fr-be -Force`    

## Add "Show Desktop" shortcut
Right Click > New > Shortcut  
`C:\Windows\explorer.exe shell:::{3080F90D-D7AD-11D9-BD98-0000947B0257}`  

## Open CMD or PowerShell from any folder  
`Shift + Right-Click` on folder in File Explorer...  

## Retake ownership  
(cmd as admin) : `> TAKEOWN /F . /R`  

## ALT+TAB behaviour

regedit `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer`  
New Key "DWORD (32-bit)" > `AltTabSettings = 1`  
Restart computer  

## File locked by another process  
Run resmon.exe "Resource Monitor" > tab "CPU" > "Search" :  filename  

## Install Office 2021 LTSC
[Office Customization Tool](https://config.office.com/deploymentsettings)  
Configure and export : Configuration.xml  
[Office Deployment Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49117)
Download and unzip : officedeploymenttool_16731-20398.exe  

`(as admin)> Setup.exe /configure Configuration.xml`  

## Passwords  

(as admin) `rundll32.exe keymgr.dll,KRShowKeyMgr`  

## USB

USBSTOR allow|disallow  
```
3 : ENABLED
4 : DISABLED
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\USBSTOR]
"Start"=dword:00000003
```

Remove RegKey : 
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PolicyManager\default\ADMX_DeviceInstallation\DeviceInstall_Removable_Deny`  

## Command to open explorer

`explorer.exe /e,/root,"C:\TEMP"`

## Access to local network while using VPN 

Enable split-tunneling :  
Run ncpa.cpl to open Network Adapter properties  
VPN connection > Properties > Internet Protocol IPv4 > Properties > Advanced...  
Uncheck "Use Default Gateway on remote network"

## Windows Defender Firewall with Advanced Security

Run control firewall.cpl  
or   
Run wf.msc  

Deactivate/Activate via netsh :  

```cmd
(as admin)
> netsh advfirewall set domainprofile state off
```
## Windows Key Backup

(as admin)
`PS > wmic path softwarelicensingservice get OA3xOriginalProductKey`   


## VLC : screen recording  
CTRL+C  
Mode de capture : Bureau > Afficher plus d'options > Modifier les options : 
`:screen-fps=20.000000 :live-caching=300 :screen-mouse-image=Mouse_pointer_small.png`  
Lire > Convertir > Définir fichier de destination > Démarrer  

---

## Excel

Function to check belgian company number (BCE) :  
`=SI((DROITE(A1;2)-(97-((GAUCHE(A1;7)-(ENT(GAUCHE(A1;7)/97)*97))))=0);"ok";"nok")`  

---

## WINMERGE

Plugin 'DisplayXMLFiles.dll' cannot pack your changes to the left file back into $filename$. The original file will not be changed.  
Fix : Menu Plugins > Manual unpacking   

---

## Keyboard shortcut

### Windows Start > Contextual Menu
`<WIN>+X`  

### System Settings
`<WIN>+<PAUSE-BREAK>`

### Task Manager
`<CTRL>+<SHIFT>+<ESCAPE>`  

### File Explorer
Open Cmd Shell (or PowerShell) in specific folder :  
`<CTRL>+<SHIFT>+Right-Click`

### Excel
Insert Date or Time :  
`<CTRL>+<;>`  
`<CTRL>+<:>`  

### Outlook
Insert Date or Time :  
`<ALT>+<SHIFT>+D`  
`<ALT>+<SHIFT>+T`  

### Eclipse
Find next :  
`<CTRL>+K`  

### Cygwin  
Call bash script from cmd :  
```
@echo off
@%~d0
@cd %~p0
@C:\cygwin64\bin\bash -c ./my_script.sh
```

### Screen + Sounds Recording with ffmpeg 
0. Enable "Stereo Mix"  
`mmsys.cpl` 
1. Identify exact names of available devices  
`$ ffmpeg -list_devices true -f dshow -i dummy`  
2. Start recording including Speaker sound  
-crf : Constant Ratio Factor [0..51] Quality Loss  
`ffmpeg -rtbufsize 1500M  -thread_queue_size 512 -f gdigrab -s 1920x1080 -i desktop -f dshow -i audio="Stereo Mix (Realtek(R) Audio)" -crf 30  -filter:a "volume=1.5" -vcodec libx264  output.mp4`   

### Retrieve the key of a pre-installed Windows 
`(as admin) > wmic path SoftwareLicensingService get OA3xOriginalProductKey`  

### Fix Corrupted token.dat file (Activation file)
[Resolve activation issue with error code 0xC004E015 on Windows 10 Enterprise for Virtual Desktops](https://www.augmastudio.com/2021/01/08/resolve-activation-issue-with-error-code-0xc004e015-on-windows-10-enterprise-for-virtual-desktops/)  

1. check Software Protection services  
`PS (as admin)> Get-Service sppsvc`  

2. Rename presumably corrupted file  
```
PS> Stop-Service sppsvc
PS> ren C:\Windows\System32\spp\store\2.0\tokens.dat C:\Windows\System32\spp\store\2.0\tokens.bak
PS> Start-Service sppsvc
```  
3. Re-install license files
`C:\Windows\System32>slmgr -rilc`  

