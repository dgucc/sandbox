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

## Log

Display comments only  
%s for the subject  
%b for the body;  
%B for both  
`git --no-pager log --pretty=format:%s` 

Git GUI : visualize all branch history  
`gitk --all` 

## Preview html page hosted in GitHub
Prepend **https://htmlpreview.github.io/?**  to url  
ex : https://htmlpreview.github.io/?https://github.com/dgucc/javascript/blob/main/d3js/map/world/index.html    
