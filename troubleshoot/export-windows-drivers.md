# Export Windows Drivers

Export with dism :   
`(as admin) > dism /online /export-driver /destination:"E:\DriversWin11"`

Export from existing image :
`(as admin) > dism /image:E:\backup\Win11Backup /export-driver /destination:E:\DriversWin11`

---

Re-install the drivers :  
`(as admin) > pnputil /add-driver "E:\DriversWin11\*.inf" /subdirs /install` 

Or  

Re-install with Device Manager  
Right-Click on the root node (Computer name) > Update Drivers etc.  

