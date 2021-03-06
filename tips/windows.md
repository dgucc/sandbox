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


