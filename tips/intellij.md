<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [IntelliJ](#intellij)
   * [Keyboard Shortcuts](#keyboard-shortcuts)
   * [PlantUML](#plantuml)
   * [Custom configuration file (overwrite default settings)  ](#custom-configuration-file-overwrite-default-settings)
   * [Add IntelliJ in contextual menu wit reg.exe  ](#add-intellij-in-contextual-menu-wit-regexe)

<!-- TOC end -->

<!-- TOC --><a name="intellij"></a>
# IntelliJ

<!-- TOC --><a name="keyboard-shortcuts"></a>
## Keyboard Shortcuts

<details>
	<summary>Shortcuts</summary>
	
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
	
</details>

---
<!-- TOC --><a name="plantuml"></a>
## PlantUML

`$ sudo apt-get install graphviz`  

Install Plugins :  
- PlantUML Diagram Generator  
- PlantUML Integration  

Contextual Menu > Generate Diagram...   

<!-- TOC --><a name="custom-configuration-file-overwrite-default-settings"></a>
## Custom configuration file (overwrite default settings)  

%AppData%\Roaming\JetBrains\IdeaIC2023.3\idea64.exe.vmoptions  


<!-- TOC --><a name="add-intellij-in-contextual-menu-wit-regexe"></a>
## Add IntelliJ in contextual menu wit reg.exe  
	/f : force no prompting and update if the element already exists  
	/ve : set empty value name "(Default)"  

```cmd
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ" /f /ve /d "Open with &IntelliJ"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ" /v "Icon" /t REG_SZ /f /v "C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea.ico"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ\command" /f /ve /d "C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea64.exe %V"

reg.exe add "HKCU\Software\Classes\Directory\Shell\IntelliJ" /f /ve /d "Open with &IntelliJ"
reg.exe add "Software\Classes\Directory\Shell\IntelliJ" /v "Icon" /t REG_SZ /f /d "C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea.ico"
reg.exe add "Software\Classes\Directory\Shell\IntelliJ\command" /d	Set (Default)="C:\Program Files (x86)\JetBrains\IntelliJ IDEA Community Edition 2023.3.2\bin\idea64.exe" "%V"
```

for linux mint  

~/.local/share/nemo/actions/intellij.nemo_action :  

```
[Nemo Action]
Name=Open in Intellij
Comment=Open with Intellij
Exec="/opt/idea/bin/idea.sh" %F
Icon-Name=intellij
Selection=Any
Extensions=dir;
```

