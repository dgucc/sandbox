<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [vscode](#vscode)
   * [shortcut](#shortcut)
   * [Configuration](#configuration)
   * [Create Custom Snippet](#create-custom-snippet)
   * [Ollama-Continue](#ollama-continue)
   * [Installation on Linux  ](#installation-on-linux)
   * [Clean extensions](#clean-extensions)
   * [Format curly brackets on the same line c# ](#format-curly-brackets-on-the-same-line-c)
   * [Hide References count in Editor ](#hide-references-count-in-editor)
   * [Add "Open with Visual Studio Code" in Contextual Menu](#add-open-with-visual-studio-code-in-contextual-menu)
   * [C/C++ with WSL](#c-c++-with-wsl)

<!-- TOC end -->

<!-- TOC --><a name="vscode"></a>
# vscode

<!-- TOC --><a name="shortcut"></a>
## shortcut

Open Terminal : <Ctrl+J>  
Focus on Project Explorer : <Ctrl+0>

Auto-complete [Emmet abbreviations](https://docs.emmet.io/cheat-sheet/) : <Tab>

<!-- TOC --><a name="configuration"></a>
## Configuration
[Configuration Visual Studio Code](https://grafikart.fr/tutoriels/vscode-settings-2096)  
<Ctrl+Shift+P> : Preferences  
Edit /home/<user>/.config/Code/User/settings.json :

<details>
<summary>settings.json</summary>

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
  // Zoom
  "editor.mouseWheelZoom": true,
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
</details>

## Create Custom snippets 
Create file (suffix .code-snippets) into  
- %APPDATA%\Code\User\snippets\custom.code-snippets (Windows)
- $HOME/.config/Code/User/snippets/custom.code-snippets
  
```json
{
	"XML annotation": {
		"scope": "",
		"prefix": "anno",
		"body": [
			"<xs:annotation>",
			"\t<xs:documentation>${1:description}</xs:documentation>",
			"</xs:annotation>"
		],
		"description": "Add a XML annotation"
	}
}
```

## Ollama-Continue

Continue can automatically detect available Ollama models. You can configure this in your YAML:
~/.continue/config.yaml

```
models:
  - name: Autodetect
    provider: ollama
    model: AUTODETECT
    roles:
      - chat
      - edit
      - apply
      - rerank
      - autocomplete

# List of all the contexts providers
context:    
# Reference any file in your current workspace.
  - provider: file
# Reference specific functions or classes from throughout your project.
  - provider: code
# Reference the most relevant snippets from your codebase.
  - provider: codebase
# Reference all of the changes you've made to your current branch.
  - provider: diff
# Reference the contents of the currently open file.
  - provider: currentFile
# Reference the contents of your IDE's terminal.
  - provider: terminal
# Reference the contents from any added documentation site.
  - provider: docs
# Reference the contents of a specific folder in your workspace.
  - provider: folder
# Reference the results of codebase search.
  - provider: search
# Reference the markdown converted contents of a given URL.
  - provider: url
# Reference the contents of your clipboard.
  - provider: clipboard
# Reference the structure of your current workspace.
  - provider: tree
# Reference reported problems or errors in your workspace.
  - provider: problems
# Reference the contents of all open files in your IDE. Set onlyPinned to true to only reference pinned files.
  - provider: open
    params:
      onlyPinned: False

rules:
  - You are a senior developer in java
  - You are a senior developer in python
  - You are a senior developer in web
  - You are a senior developer in xml
  - You are a senior analyst in uml

```

---

<!-- TOC --><a name="installation-on-linux"></a>
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

## Clean extensions

Remove folder extensions :  

 - Windows: `%USERPROFILE%\.vscode\extensions` 
 - Linux: `~/.vscode/extensions` 



<!-- TOC --><a name="format-curly-brackets-on-the-same-line-c"></a>
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

<!-- TOC --><a name="hide-references-count-in-editor"></a>
## Hide References count in Editor 

<Ctrl+Shif+P> Open User Settings : uncheck "Controls whether the editor Show CodeLens"  

<!-- TOC --><a name="add-open-with-visual-studio-code-in-contextual-menu"></a>
## Add "Open with Visual Studio Code" in Contextual Menu
"C:\home\apps\VSCode-win32-x64-1.84.2\Code.exe %1"  

```
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\vscode" /f /ve /d "Open with &vscode"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\vscode" /v "Icon2" /t REG_SZ /f /d "C:\home\apps\VSCode-win32-x64-1.84.2\Code.exe,0"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\vscode\command" /f /ve /d "C:\home\apps\VSCode-win32-x64-1.84.2\Code.exe %1"

reg.exe add "HKCU\Software\Classes\Directory\Shell\vscode" /f /ve /d "Open with &vscode"
reg.exe add "HKCU\Software\Classes\Directory\Shell\vscode" /v "Icon" /t REG_SZ /f /d "C:\home\apps\VSCode-win32-x64-1.84.2\Code.exe,0"
reg.exe add "HKCU\Software\Classes\Directory\Shell\vscode\command" /d "C:\home\apps\VSCode-win32-x64-1.84.2\Code.exe %1"
```

for linux ~/.local/share/nemo/actions/vscode.nemo_action :   

```
[Nemo Action]
Name=Open in VS Code
Comment=Open in VS Code
Exec=code "%F"
Icon-Name=visual-studio-code
Selection=Any
Extensions=dir;
```

## C/C++ with WSL

cf. https://code.visualstudio.com/docs/cpp/config-wsl  

```
sudo apt-get install build-essential gdb

cd $HOME/projects/helloworld
code .
```  

