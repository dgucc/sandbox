# vscode

## shortcut

Open Terminal : <Ctrl+J>  


## Configuration
[Configuration Visual Studio Code](https://grafikart.fr/tutoriels/vscode-settings-2096)  
<Ctrl+Shift+P> : Preferences  
Edit /home/<user>/.config/Code/User/settings.json :

```json
{
  "workbench.startupEditor": "none", // On ne veut pas une page d'accueil chargée
  "editor.minimap.enabled": false,
  "breadcrumbs.enabled": false,
  // -- Sidebar
  "workbench.tree.indent": 20, // Indente plus pour plus de clarté dans la sidebar
  "workbench.tree.renderIndentGuides": "always",
  // -- Code
  "editor.occurrencesHighlight": false, 
  "editor.renderWhitespace": "trailing", // On ne veut pas laisser d'espace en fin de ligne

  // Thème
  "editor.fontFamily": "'JetBrains Mono', 'Fira Code', 'Operator Mono Lig', monospace",
  "editor.fontLigatures": true,
  "editor.fontSize": 11,
//   "editor.lineHeight": 28,
  "workbench.colorTheme": "Tokyo Night",
  "workbench.iconTheme": "material-icon-theme",
  "workbench.colorCustomizations": {
    "[Tokyo Night]": {
      "editor.selectionBackground": "#3D59A1",
      "editor.selectionHighlightBackground": "#3D59A1"
    },
  },

  // Ergonomie
  "editor.wordWrap": "on",
  "editor.suggest.insertMode": "replace", // L'autocomplétion remplace le mot en cours
  "editor.acceptSuggestionOnCommitCharacter": false, // Evite que l'autocomplétion soit accepté lors d'un . par exemple
  "editor.formatOnSave": false,
  "editor.formatOnPaste": false,
  "editor.linkedEditing": true, // Quand on change un élément HTML, change la balise fermante
  "explorer.autoReveal": false,
  "explorer.confirmDragAndDrop": false,
  "workbench.editor.enablePreview": false, // Un clic sur un fichier l'ouvre
  "emmet.triggerExpansionOnTab": true, 
  // Fichiers
  "files.autoSave": "onFocusChange",
  "files.defaultLanguage": "markdown",
  "files.exclude": {
    "**/.idea": true
  },
  // Languages
  "javascript.preferences.importModuleSpecifierEnding": "js",
  "typescript.preferences.importModuleSpecifierEnding": "js",
  // Formatters
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  // Extensions
  "liveServer.settings.donotVerifyTags": true,
  "gitlens.codeLens.enabled": false,
  "gitlens.currentLine.enabled": false,
  "editor.unicodeHighlight.nonBasicASCII": false,
  "omnisharp.autoStart": true,
  "omnisharp.enableEditorConfigSupport": false,
  "editor.codeLens": false,
  "editor.mouseWheelZoom": true,
  "editor.tabSize": 3,
  "git.enableSmartCommit": true,
  "RainbowBrackets.depreciation-notice": false,
  "[python]": {
    "editor.formatOnType": true
  }
}
```  
---

## Installation on Linux  

[Install Visual Studio Code and .NET Core for C# coding on Linux](https://www.pragmaticlinux.com/2021/03/install-visual-studio-code-and-net-core-for-c-coding-on-linux/)  

Install the .NET SDK package repository   
`$ wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb`  
`$ sudo dpkg -i packages-microsoft-prod.deb` 

Install .NET SDK   
`$ sudo apt-get update`   
`$ sudo apt-get install dotnet-sdk-5.0`  
`$ dotnet --list-sdks`  

[Setup, Build and Run Blazor WASM App on Linux](https://www.prowaretech.com/articles/current/information-technology/linux/setup-and-configure/build-and-run-blazor-wasm-apps)

Create the Blazor WebAssembly Client and Server Projects  
`$dotnet new blazorwasm -o BlazorWasmApp -f net6.0 -ho`  

Restore Dependencies  
`$ dotnet restore`   
Build (in same folder as "sln")  
`$ dotnet build`   
Run by specifying project  
`$ dotnet run --project Server`   

[3 Ways to install Visual studio code in Ubuntu using terminal](https://www.how2shout.com/linux/3-ways-install-visual-studio-code-in-ubuntu-using-terminal/)  

```bash
$ sudo apt install apt-transport-https
$ wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
$ sudo dpkg -i packages-microsoft-prod.deb
$ sudo apt-get update 
$ sudo apt-get install -y dotnet-sdk-6.0
$ sudo apt-get install -y aspnetcore-runtime-6.0
```

## Format curly brackets on the same line c# 

Create a omnisharp.json file in the root of your project or in `%userprofile%\.omnisharp\` folder :   
```json
{
    "FormattingOptions": {
        "NewLinesForBracesInLambdaExpressionBody": false,
        "NewLinesForBracesInAnonymousMethods": false,
        "NewLinesForBracesInAnonymousTypes": false,
        "NewLinesForBracesInControlBlocks": false,
        "NewLinesForBracesInTypes": false,
        "NewLinesForBracesInMethods": false,
        "NewLinesForBracesInProperties": false,
        "NewLinesForBracesInObjectCollectionArrayInitializers": false,
        "NewLinesForBracesInAccessors": false,
        "NewLineForElse": false,
        "NewLineForCatch": false,
        "NewLineForFinally": false
    }
}
```
<Ctrl+Shif+P> Open User Settings (JSON) :  
```json
  "omnisharp.autoStart": true,
  "omnisharp.enableEditorConfigSupport": false
```

<Ctrl+Shift+P> Restart Omnisharp  

## Hide References count in Editor 

<Ctrl+Shif+P> Open User Settings : uncheck "Controls whether the editor Show CodeLens"  

## Add "Open with Visual Studio Code" in Contextual Menu
vsCodeOpenFolder.reg :   
```
Windows Registry Editor Version 5.00
; Open files
[HKEY_CLASSES_ROOT\*\shell\Open with VS Code]
@="Edit with VS Code"
"Icon"="C:\\Users\\dgucc\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe,0"
[HKEY_CLASSES_ROOT\*\shell\Open with VS Code\command]
@="\"C:\\Users\\dgucc\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""
; This will make it appear when you right click ON a folder
; The "Icon" line can be removed if you don't want the icon to appear
[HKEY_CLASSES_ROOT\Directory\shell\vscode]
@="Open Folder as VS Code Project"
"Icon"="\"C:\\Users\\dgucc\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\",0"
[HKEY_CLASSES_ROOT\Directory\shell\vscode\command]
@="\"C:\\Users\\dgucc\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""
; This will make it appear when you right click INSIDE a folder
; The "Icon" line can be removed if you don't want the icon to appear
[HKEY_CLASSES_ROOT\Directory\Background\shell\vscode]
@="Open Folder as VS Code Project"
"Icon"="\"C:\\Users\\dgucc\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\",0"
[HKEY_CLASSES_ROOT\Directory\Background\shell\vscode\command]
@="\"C:\\Users\\dgucc\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%V\""
```

