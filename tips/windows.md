## Keyboard layout
powershell as admin  
`PS > Set-WinUserLanguageList -LanguageList fr-be -Force`    

## Add "Show Desktop" shortcut

Right Click > New > Shortcut  
`C:\Windows\explorer.exe shell:::{3080F90D-D7AD-11D9-BD98-0000947B0257}`  

## Open CMD or PowerShell from any folder  

`Shift + Right-Click` on folder in File Explorer...  

## ALT+TAB behaviour

regedit `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer`  
New Key "DWORD (32-bit)" > `AltTabSettings = 1`  
Restart computer  

