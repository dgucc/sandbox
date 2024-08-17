# IntelliJ

## Keyboard Shortcuts

Go to Declaration  
	`F4`

Go to Implementation  
	`<Ctrl> + <Alt> + B`  

Find All Usages  
	`<Ctrl> + <Alt> + F7`

Find in Files  
	`<Ctrl> + <Shift> + F`

---
Copy Absolute Path  
	`<Ctrl> + <Shift> + C`

---  
Add Bookmark  
	`F11`

Show Bookmarks  
	`<Shift> + F11`

---

Go to Line  
	`<Ctrl> + G`  
 
---
Move line  
	`<Alt> + <Shift> + <Up>`   
	or  
	`<Alt> + <Shift> + <Down>`  

---
Comment In/Out  
	`<Ctrl> + / numpad`  

---
Format Code  
	`<Ctrl> + <Alt> + L`  

---
Build Project  
	`<Ctrl> + <F9>`  

---
Navigate backward | Forward  
	`<Ctrl> + <Alt> + <Left>`  
 	or  
  	`<Ctrl> + <Alt> + <Right>`  

---

## PlantUML

`$ sudo apt-get install graphviz`  

Install Plugins :  
- PlantUML Diagram Generator  
- PlantUML Integration  

Contextual Menu > Generate Diagram...   

## Custom configuration file (overwrite default settings)  

%AppData%\Roaming\JetBrains\IdeaIC2023.3\idea64.exe.vmoptions  


## Add IntelliJ in contextual menu wit reg.exe  
	/f : force no prompting  
	/ve : set empty value name "(Default)"  

```cmd
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ" /f /ve /d "Open with &IntelliJ"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ" /v "Icon" /t REG_SZ /f /v "C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea.ico"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ\command" /f /ve /d "C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea64.exe %V"

reg.exe add "HKCU\Software\Classes\Directory\Shell\IntelliJ" /f /ve /d "Open with &IntelliJ"
reg.exe add "Software\Classes\Directory\Shell\IntelliJ" /v "Icon" /t REG_SZ /f /d "C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea.ico"
reg.exe add "Software\Classes\Directory\Shell\IntelliJ\command" /d	Set (Default)="C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea64.exe" "%V"
```
