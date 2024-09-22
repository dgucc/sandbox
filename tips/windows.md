## Windows 11 installation with local user
When invited to connect to internet : SHIFT + F10  
`(as admin)> OOBE\BYPASSNRO`  -> Restart installation procedure...  

## Usefull commands 

<details>
<summary>Les commandes de Windows par invité de commande</summary>

## General
```
    ACCESS.CPL : ouvre les options d’accessibilité (Pour XP uniquement)
    APPWIZ.CPL : ouvre l’outil Ajouter/Supprimer un programme
    AZMAN.MSC : ouvre le gestionnaire d’autorisations (Pour Vista uniquement)
    CERTMGR.MSC : ouvre les certificats pour l’utilisateur actuel
    CLICONFG : ouvre la configuration des clients SQL
    COLLAB.CPL : ouvre le voisinage immédiat (Pour Vista uniquement)
    COMEXP.MSC ou bien DCOMCNFG : ouvre l’outil services et composants (Pour Vista uniquement)
    COMPMGMT.MSC : ouvre l’outil de gestion de l’ordinateur
    COMPUTERDEFAULTS : ouvrir l’outil des programmes par défaut (Pour Vista uniquement)
    CONTROL /NAME MICROSOFT.BACKUPANDRESTORECENTER : ouvre le centre de sauvegarde et de restauration (Pour Vista uniquement).
    CONTROL ADMINTOOLS : ouvre les outils d’administrations
    CONTROL COLOR : ouvre les paramètres de l’apparence
    CONTROL FOLDERS : ouvre les options de dossiers
    CONTROL FONTS : ouvre le gestionnaire de polices
    CONTROL INTERNATIONAL ou bien INTL.CPL : ouvre les options régionales et linguistiques
    CONTROL KEYBOARD : ouvre les propriétés du clavier
    CONTROL MOUSE ou bien MAIN.CPL : ouvre les propriétés de la souris
    CONTROL PRINTERS : ouvre les imprimantes et les fax disponibles
    CONTROL USERPASSWORDS : ouvre l’éditeur des comptes utilisateurs
    CONTROL USERPASSWORDS2 ou bien NETPLWIZ : contrôle les utilisateurs et leurs accès
    CONTROL : ouvre le panneau de configuration
    CREDWIZ : ouvre l’outil de sauvegarde et restauration des mots de passe des utilisateurs (Pour Vista uniquement)
    DESK.CPL : ouvre les paramètres d’affichage
    DEVMGMT.MSC : ouvre les gestionnaire de périphériques.
    DRWTSN32 : ouvre Dr. Watson (Pour XP uniquement)
    DXDIAG : ouvre l’outil de diagnostic DirectX
    EVENTVWR ou bien EVENTVWR.MSC : ouvre l’observateur d’évènements
    FSMGMT.MSC : ouvre les dossiers partagés
    GPEDIT.MSC : ouvre l’éditeur des stratégies de groupe (Pour les éditions professionnelles et plus de Windows)
    HDWWIZ.CPL : ouvre l’assistant ajout de matériels
    INFOCARDCPL.CPL : ouvre l’assistant compatibilité des programmes
    IRPROPS.CPL : ouvre le gestionnaire d’infrarouge
    ISCSICPL : ouvre l’outil de configuration de l’initiateur ISCI Microsoft (Pour Vista uniquement)
    JOY.CPL : ouvre l’outil de contrôleur de jeu
    LPKSETUP : ouvre l’assistant d’installation et désinstallation des langues d’affichage (Pour Vista uniquement)
    LUSRMGR.MSC : ouvre l’éditeur des utilisateurs et groupes locaux
    MDSCHED : ouvre l’outil de diagnostics de la mémoire Windows (Pour Vista uniquement)
    MMC : ouvre une nouvelle console vide
    MMSYS.CPL : ouvre les paramètres de sons
    MOBSYNC : ouvre le centre de synchronisation
    MSCONFIG : ouvre l’outil de configuration du système
    NAPCLCFG.MSC : ouvre l’outil de configuration du client NAP (Pour Vista uniquement)
    NTMSMGR.MSC : ouvre le gestionnaire des supports de stockage amovibles
    NTMSOPRQ.MSC : ouvre les demandes de l’opérateur de stockage amovible
    ODBCAD32 : ouvre l’administrateur de sources de données ODBC
    OPTIONALFEATURES : ouvre l’outil Ajouter/Supprimer des composants Windows (Pour Vista uniquement)
    PERFMON ou bien PERFMON.MSC : ouvre le moniteur de fiabilité et de performances Windows.
    POWERCFG.CPL : ouvre le gestionnaire des modes d’alimentation (Pour Vista uniquement)
    REGEDIT ou bien REGEDT32 (Pour Vista uniquement) : ouvre l’éditeur de registre
    REKEYWIZ : ouvre le gestionnaire des certificats de chiffrement de fichiers (Pour Vista uniquement)
    RSOP.MSC : ouvre le jeu de stratégie résultant
    SECPOL.MSC : ouvre les paramètres de sécurités locales
    SERVICES.MSC : ouvre le gestionnaire de services
    SLUI : ouvre l’assistant d’activation de Windows (Pour Vista uniquement)
    SYSDM.CPL : ouvre les propriétés système
    SYSEDIT : ouvre l’éditeur de configuration système (Attention, à manipuler avec prudence)
    SYSKEY : ouvre l’utilitaire de protection de la base de données des comptes Windows (Attention, à manipuler avec extrême prudence !)
    SYSPREP: ouvre le dossier contenant l’outil de préparation du système (Pour Vista uniquement)
    TABLETPC.CPL : ouvre les paramètres pour Tablet pc (Pour Vista uniquement)
    TASKSCHD.MSC ou bien CONTROL SCHEDTASKS : ouvre le planificateur de tâches (Pour Vista uniquement)
    TELEPHON.CPL : ouvre l’outil de connexion téléphonique
    TIMEDATE.CPL : ouvre les paramètres de l’heure et de la date
    TPM.MSC : ouvre l’outil gestion de module de plateforme sécurisée sur l’ordinateur local (Pour Vista uniquement)
    UTILMAN : ouvre les options d’ergonomie (Pour Vista uniquement)
    VERIFIER : ouvre le gestionnaire de vérification des pilotes
    WMIMGMT.MSC : ouvre Windows Management Infrastructure
    WSCUI.CPL : ouvre le centre de sécurité Windows
    WUAUCPL.CPL : ouvre le service de mise à jour Windows (Pour XP uniquement)
```
## Programmes et outils Windows
```
    %WINDIR%\SYSTEM32\RESTORE\RSTRUI.EXE : ouvre l’outil de restauration de système (Pour XP uniquement).
    CALC : ouvre la calculatrice
    CHARMAP : ouvre la table des caractères
    CLIPBRD : ouvre le presse papier (Pour XP uniquement)
    CMD : ouvre l’invite de commandes
    DIALER : ouvre le numérateur téléphonique de Windows
    DVDPLAY : ouvre votre lecteur DVD
    EUDCEDIT : ouvre l’éditeur de caractères privés
    EXPLORER : ouvre l’explorateur Windows
    FSQUIRT : Assistant transfert Bluetooth
    IEXPLORE : ouvre Internet Explorer
    IEXPRESS : ouvre l’assistant de création des archives auto-extractibles.
    JOURNAL : ouvre un nouveau journal (Pour Vista uniquement)
    MAGNIFY : ouvre la loupe
    MBLCTR : ouvre le centre de mobilité de Windows (Pour Vista uniquement)
    MIGWIZ : ouvre l’outil de transfert de fichiers et de paramètres Windows (Pour Vista uniquement)
    MIGWIZ.EXE : ouvre l’outil de transfert de fichiers et de paramètres Windows (pour XP uniquement)
    MOVIEMK : ouvre Windows Movie Maker
    MRT : lance l’utilitaire de suppression de logiciel malveillant.
    MSDT : ouvre l’outil de diagnostics et support Microsoft
    MSINFO32 : ouvre les informations système
    MSPAINT : ouvre Paint
    MSRA : ouvre l’assistance à distance Windows
    MSTSC : ouvre l’outil de connexion du bureau a distance
    NOTEPAD : ouvre le bloc-notes
    OSK : ouvre le clavier visuel.
    PRINTBRMUI : ouvre l’assistant de migration d’imprimante (Vista uniquement)
    RSTRUI : ouvre l’outil de restauration du système (Pour Vista uniquement)
    SIDEBAR : ouvre le volet Windows (Pour Vista uniquement)
    SIGVERIF : ouvre l’outil de vérification des signatures de fichiers
    SNDVOL : ouvre le mélangeur de volume
    SNIPPINGTOOL : ouvre l’outil capture d’écran (Pour Vista uniquement).
    SOUNDRECORDER : ouvre le magnétophone
    STIKYNOT : ouvre le pense-bête (Pour Vista uniquement)
    TABTIP : ouvre le panneau de saisie Tablet PC (Pour Vista uniquement)
    TASKMGR : ouvre le gestionnaire des tâches Windows
    WAB : ouvre les contacts (Pour Vista uniquement)
    WERCON : ouvre l’outil de rapports et de solutions aux problèmes (Pour Vista uniquement)
    WINCAL : ouvre le calendrier Windows (Pour Vista uniquement)
    WINCHAT : ouvre le logiciel Microsoft de chat en réseau (Pour Windows XP uniquement)
    WINDOWSANYTIMEUPGRADE : permet la mise à niveau de Windows Vista
    WINVER : ouvre la fenêtre pour connaître votre version Windows
    WINWORD: ouvre Word (si il est installé)
    WMPLAYER : ouvre le lecteur Windows Media
    WRITE ou bien Wordpad : ouvre Wordpad
```

## Gestion des disques
```
    CHKDSK : effectue une analyse de la partition précisée dans les paramètres de la commande (Pour plus d’informations, tapez CHKDSK /? dans l’invite de commande CMD)
    CLEANMGR : ouvre l’outil de nettoyage de disque
    DEFRAG: Défragmente le disque dur
    DFRG.MSC : ouvre l’outil de défragmentation de disque
    DISKMGMT.MSC : ouvre le gestionnaire de disques
    DISKPART : ouvre l’outil de partitionnement (un peu lourd à manipuler)
```
## Gestion des réseaux et Internet
```
    CONTROL NETCONNECTIONS ou bien NCPA.CPL : ouvre les connexions réseau
    FIREWALL.CPL : ouvre le pare-feu Windows
    INETCPL.CPL : ouvre les propriétés internet
    IPCONFIG : affiche les configurations des adresses IP sur l’ordinateur (Pour plus d’informations, tapezIPCONFIG /? dans l’invite de commande CMD)
    NETSETUP.CPL : ouvre l’assistant configuration réseau (Pour XP uniquement)
    WF.MSC : ouvre les fonctions avancées du pare-feu Windows (Pour Vista uniquement).
```
## Autres commandes
```
    %HOMEDRIVE% : ouvre l’explorateur sur la partition ou le système d’exploitation est installé
    %HOMEPATH% : ouvre le dossier d’utilisateur connecté actuellement C:\Documents and settings\[nom d’utilisateur]
    %PROGRAMFILES% : ouvre le dossier d’installation d’autres programmes (Program Files)
    %TEMP% ou bien %TMP% : ouvre le dossier temporaire
    %USERPROFILE% : ouvre le dossier du profil de l’utilisateur connecté actuellement
    %WINDIR% ou bien %SYSTEMROOT% : ouvre le dossier d’installation de Windows
    %WINDIR%\system32\rundll32.exe shell32.dll,Control_RunDLL hotplug.dll : affiche la fenêtre “Supprimer le périphérique en toute sécurité”
    AC3FILTER.CPL : ouvre les propriétés du filtre AC3 (Si installé)
    FIREFOX : lance Mozilla FireFox (Si installé)
    JAVAWS : Visualise le cache du logiciel JAVA (Si installé)
    LOGOFF : ferme la session actuelle
    NETPROJ : autorise ou pas la connexion à un projecteur réseau (Pour Vista uniquement)
    Vérificateur des fichiers système (Nécessite un CD de Windows si le cache n’est pas disponible)
        SFC /SCANNOW : scanne immédiatement tous les fichiers système et répare les fichiers endommagés
        SFC /VERIFYONLY : scanne seulement les fichiers système
        SFC /SCANFILE=”nom et chemin de fichier” : scanne le fichier précisé, et le répare s’il est endommagé
        SFC /VERIFYFILE=”nom et chemin de fichier” : scanne seulement le fichier précisé
        SFC /SCANONCE : scanne les fichiers système au prochain redémarrage
        SFC /REVERT : remet la configuration initiale (Pour plus d’informations, tapez SFC /? dans l’invite de commande CMD.
    SHUTDOWN : éteint Windows
    SHUTDOWN –A : interrompe l’arrêt de Windows
    VSP1CLN : supprime le cache d’installation du service pack 1 de Vista

```  
</details>

## Run commnad by skipping UAC pop-up

Environment Variables  
`cmd /min /c start "" rundll32 sysdm.cpl,EditEnvironmentVariables`  

Task Manager no UAC  
`cmd /min /c "set __COMPAT_LAYER=RunAsInvoker && TaskMgr.exe"`  

Regedit no UAC  
`cmd /min /c "set __COMPAT_LAYER=RunAsInvoker && regedit.exe"`  


## Free up disk space
[12 best ways to free up hard drive space on Windows 10](https://www.windowscentral.com/best-ways-to-free-hard-drive-space-windows-10)  

## Keyboard layout
powershell as admin  
`PS > Set-WinUserLanguageList -LanguageList fr-be -Force`    

## Add "Show Desktop" shortcut
Right Click > New > Shortcut  
`C:\Windows\explorer.exe shell:::{3080F90D-D7AD-11D9-BD98-0000947B0257}`  

## God Mode
Create special folder : `GodMode.{ED7BA470-8E54-465E-825C-99712043E01C}`  

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

Fix Activation Issue :  
```
(as admin) > CD C:\Program Files\Microsoft Office\Office16
(as admin) > cscript ospp.vbs /sethst:kms.03k.org
(as admin) > cscript ospp.vbs /act
```

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

## Move or Rezize Window with Keyboard Shortcut

`Alt`+`Space` : open title bar menu (Move, Size, ...)  


## Access to local network while using VPN 

Enable split-tunneling :  
Run ncpa.cpl to open Network Adapter properties  
VPN connection > Properties > Internet Protocol IPv4 > Properties > Advanced...  
Uncheck "Use Default Gateway on remote network" 

or 

`PS> Set-VPNConnection -Name "Connection Name" -SplitTunneling $True`

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

## Windows Subsystem Linux (WSL)

### Prerequis
- Virtual Machine Platform   
- Microsoft Windows Subsystem Linux   

```
PS> dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
PS> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
### Installation
```
PS> wsl --list --online
PS> wsl --install -d Ubuntu
```
