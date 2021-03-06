# GIT

## GitHub - Getting started

### Command gh needed to push modifications to remote repository  
Install GitHub CLI on macOS, Windows, or Linux.  

In the command line, authenticate to GitHub.  
`$ gh auth login`  

Git clone alternative  
`$ gh repo clone dgucc/sandbox`  

### How-to push existing repository from command line  

Link local and remote repositories  
` $ git remote add origin https://github.com/dgucc/sandbox.git`  

Rename master into main  
`$ git branch -M main`  

Push modifications to github  
`$ git push -u origin main`  

How-to Update local repository from github  
`$ git pull origin main`  

---

## Misc

Remove unstaged files  
`$ git clean -df`  

Remove non commited change  
`$ git reset --hard`  
