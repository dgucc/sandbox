# Pour Samuel

## Git

Downloads

**Git**

[TortoiseGit](https://download.tortoisegit.org/tgit/2.17.0.0/TortoiseGit-2.17.0.2-64bit.msi)  
[guide git fr](https://github.com/progit/progit2-fr/releases/download/2.1.77/progit.pdf)  

**GitHub**  
[github CLI](https://github.com/cli/cli/releases/download/v2.68.1/gh_2.68.1_windows_amd64.msi)  

S'authentifier auprès de  GitHub.com:   
```bash
$ gh auth login
? Where do you use GitHub? GitHub.com
? What is your preferred protocol for Git operations on this host? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Login with a web browser

! First copy your one-time code: DB5A-C92B
Press Enter to open https://github.com/login/device in your browser...
✓ Authentication complete.
- gh config set -h github.com git_protocol https
✓ Configured git protocol
✓ Logged in as dgucc
```

Récupérer son 1er projet depuis GitHub
```bash
git clone dgucc/sandbox
```

## Other Tools
[vscode](https://code.visualstudio.com/docs/?dv=win32arm64zip)  
[winmerge](https://downloads.sourceforge.net/winmerge/winmerge-2.16.46-x64-exe.zip)  

