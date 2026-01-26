<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [vscode](#vscode)
   * [shortcut](#shortcut)
   * [Custom keybindings](#custom-keybindings)
   * [Configuration](#configuration)
   * [Disable CoPilot](#disable-copilot)
   * [Create Custom Snippets](#create-custom-snippets)
   * [Add Cygwin as terminal](#add-cygwin-as-terminal)
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

## custom keybindings

Duplicate line is missing in linux version      
create ~/.config/Code/User/keybindings.json :  

```json
// Place your key bindings in this file to override the defaults
[
    {
        "key": "ctrl+shift+up",
        "command": "editor.action.copyLinesUpAction",
        "when": "editorTextFocus"
    },
    {
        "key": "ctrl+shift+down",
        "command": "editor.action.copyLinesDownAction",
        "when": "editorTextFocus"
    }
]
```
<details>
	<summary>[vscode internal commands](https://gist.github.com/skfarhat/4e88ef386c93b9dceb98121d9457edbf)</summary>
<pre>
editor.action.selectAll
editor.action.moveCarretLeftAction
editor.action.moveCarretRightAction
editor.action.transposeLetters
editor.action.clipboardCutAction
editor.action.clipboardCopyAction
editor.action.clipboardPasteAction
editor.action.clipboardCopyWithSyntaxHighlightingAction
editor.action.commentLine
editor.action.addCommentLine
editor.action.removeCommentLine
editor.action.blockComment
editor.action.showContextMenu
editor.action.fontZoomIn
editor.action.fontZoomOut
editor.action.fontZoomReset
editor.action.formatDocument
editor.action.formatSelection
editor.action.format
editor.action.indentationToSpaces
editor.action.indentationToTabs
editor.action.indentUsingTabs
editor.action.indentUsingSpaces
editor.action.detectIndentation
editor.action.reindentlines
editor.action.reindentselectedlines
editor.action.copyLinesUpAction
editor.action.copyLinesDownAction
editor.action.duplicateSelection
editor.action.moveLinesUpAction
editor.action.moveLinesDownAction
editor.action.sortLinesAscending
editor.action.sortLinesDescending
editor.action.trimTrailingWhitespace
editor.action.deleteLines
editor.action.indentLines
editor.action.outdentLines
editor.action.insertLineBefore
editor.action.insertLineAfter
editor.action.joinLines
editor.action.transpose
editor.action.transformToUppercase
editor.action.transformToLowercase
editor.action.transformToTitlecase
editor.action.onTypeRename
editor.action.smartSelect.grow
editor.action.smartSelect.expand
editor.action.smartSelect.shrink
editor.action.toggleTabFocusMode
editor.action.forceRetokenize
editor.action.diffReview.next
editor.action.diffReview.prev
editor.action.selectToBracket
editor.action.jumpToBracket
editor.action.inPlaceReplace.up
editor.action.inPlaceReplace.down
editor.action.openLink
editor.action.quickFix
editor.action.refactor
editor.action.sourceAction
editor.action.organizeImports
editor.action.autoFix
editor.action.fixAll
editor.action.codeAction
editor.action.triggerParameterHints
editor.action.rename
editor.action.wordHighlight.next
editor.action.wordHighlight.prev
editor.action.wordHighlight.trigger
editor.action.marker.next
editor.action.marker.prev
editor.action.marker.nextInFiles
editor.action.marker.prevInFiles
editor.action.nextMatchFindAction
editor.action.previousMatchFindAction
editor.action.nextSelectionMatchFindAction
editor.action.previousSelectionMatchFindAction
editor.action.startFindReplaceAction
editor.action.replaceOne
editor.action.replaceAll
editor.action.selectAllMatches
editor.action.insertCursorAbove
editor.action.insertCursorBelow
editor.action.insertCursorAtEndOfEachLineSelected
editor.action.addSelectionToNextFindMatch
editor.action.addSelectionToPreviousFindMatch
editor.action.moveSelectionToNextFindMatch
editor.action.moveSelectionToPreviousFindMatch
editor.action.selectHighlights
editor.action.changeAll
editor.action.addCursorsToBottom
editor.action.addCursorsToTop
editor.action.goToDeclaration
editor.action.revealDefinition
editor.action.openDeclarationToTheSide
editor.action.revealDefinitionAside
editor.action.previewDeclaration
editor.action.peekDefinition
editor.action.revealDeclaration
editor.action.peekDeclaration
editor.action.goToTypeDefinition
editor.action.peekTypeDefinition
editor.action.goToImplementation
editor.action.peekImplementation
editor.action.goToReferences
editor.action.referenceSearch.trigger
editor.action.goToLocations
editor.action.peekLocations
editor.action.findReferences
editor.action.showReferences
editor.action.showHover
editor.action.showDefinitionPreviewHover
editor.action.triggerSuggest
editor.action.showAccessibilityHelp
editor.action.toggleColumnSelection
editor.action.toggleMinimap
editor.action.toggleRenderControlCharacter
editor.action.toggleRenderWhitespace
editor.action.insertSnippet
editor.action.showSnippets
editor.action.dirtydiff.previous
editor.action.dirtydiff.next
editor.action.formatDocument.multiple
editor.action.formatSelection.multiple
editor.action.measureExtHostLatency
editor.action.defineKeybinding
editor.action.nextCommentThreadAction
editor.action.startDebugTextMate
editor.action.customEditor.undo
editor.action.customEditor.redo
editor.action.toggleWordWrap
editor.action.inspectTMScopes
editor.action.formatDocument.none
editor.action.webvieweditor.copy
editor.action.webvieweditor.paste
editor.action.webvieweditor.cut
editor.action.webvieweditor.undo
editor.action.webvieweditor.redo
editor.action.extensioneditor.showfind
editor.action.extensioneditor.findNext
editor.action.extensioneditor.findPrevious
		
</pre>

</details>

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

## Add Cygwin as terminal 

%AppData%\Code\User\settings.json :  
```json
 "terminal.integrated.profiles.windows": {
	[...]
	"Cygwin": {
      "path": "C:\\home\\apps\\cygwin\\cygwin64\\bin\\bash.exe",
      "args": ["--login"],
      "env": {"CHERE_INVOKING": "1"}
    },
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
## Disable CoPilot

Ctrl+Shift+P  

Chat:Disable AI Features  

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

