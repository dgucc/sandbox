## Notepad++

reg add "HKCU\Software\Classes\*\shell\Notepad++" /v "Icon" /t REG_SZ /d "C:\home\apps\textpad\textpad.exe,0" /f
reg add "HKCU\Software\Classes\*\shell\Notepad++\command" /ve /t REG_SZ /d "\"C:\home\apps\textpad\textpad.exe\" \"%1\"" /f

## VSCode
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\vscode" /f /ve /d "Open with &vscode"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\vscode" /v "Icon2" /t REG_SZ /f /d "C:\home\apps\VSCode-win32-x64-1.115.0\Code.exe,0"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\vscode\command" /f /ve /d "C:\home\apps\VSCode-win32-x64-1.115.0\Code.exe %1"

reg.exe add "HKCU\Software\Classes\Directory\Shell\vscode" /f /ve /d "Open with &vscode"
reg.exe add "HKCU\Software\Classes\Directory\Shell\vscode" /v "Icon" /t REG_SZ /f /d "C:\home\apps\VSCode-win32-x64-1.115.0\Code.exe,0"
reg.exe add "HKCU\Software\Classes\Directory\Shell\vscode\command" /f /d "C:\home\apps\VSCode-win32-x64-1.115.0\Code.exe %1"

## Cygwin
reg.exe add "HKCU\Software\Classes\Drive\Shell\cygwin64_bash" /f /ve /d "&Cygwin"
reg.exe add "HKCU\Software\Classes\Drive\Shell\cygwin64_bash" /v "Icon" /t REG_SZ /f /d "C:\home\apps\cygwin\cygwin64\Cygwin-Terminal.ico"
reg.exe add "HKCU\Software\Classes\Drive\Shell\cygwin64_bash\command" /f /d "C:\home\apps\cygwin\cygwin64\bin\mintty.exe -e /bin/xhere /bin/bash.exe \"%V\""

reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\cygwin64_bash" /f /ve /d "&Cygwin"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\cygwin64_bash" /v "Icon" /t REG_SZ /f /d "C:\home\apps\cygwin\cygwin64\Cygwin-Terminal.ico"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\cygwin64_bash\command" /f /ve /d "C:\home\apps\cygwin\cygwin64\bin\mintty.exe -e /bin/xhere /bin/bash.exe \"%V\""

reg.exe add "HKCU\Software\Classes\Directory\Shell\cygwin64_bash" /f /ve /d "&Cygwin"
reg.exe add "HKCU\Software\Classes\Directory\Shell\cygwin64_bash" /v "Icon" /t REG_SZ /f /d "C:\home\apps\cygwin\cygwin64\Cygwin-Terminal.ico"
reg.exe add "HKCU\Software\Classes\Directory\Shell\cygwin64_bash\command" /f /d "C:\home\apps\cygwin\cygwin64\bin\mintty.exe -e /bin/xhere /bin/bash.exe \"%V\""


## IntelliJ
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ" /f /ve /d "&IntelliJ"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ" /v "Icon" /t REG_SZ /f /d "C:\home\apps\ideaIC-2023.3.2.win\bin\idea.ico"
reg.exe add "HKCU\Software\Classes\Directory\Background\Shell\IntelliJ\command" /f /ve /d "C:\home\apps\ideaIC-2023.3.2.win\bin\idea64.exe \"%V\""

reg.exe add "HKCU\Software\Classes\Directory\Shell\IntelliJ" /f /ve /d "&IntelliJ"
reg.exe add "HKCU\Software\Classes\Directory\Shell\IntelliJ" /v "Icon" /t REG_SZ /f /d "C:\home\apps\ideaIC-2023.3.2.win\bin\idea.ico"
reg.exe add "HKCU\Software\Classes\Directory\Shell\IntelliJ\command" /f /d "C:\home\apps\ideaIC-2023.3.2.win\bin\idea64.exe \"%V\""
