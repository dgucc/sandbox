# Sandbox

## GitHub CLI - Getting started

Command ***gh*** needed to push modifications to remote repository
1. Install GitHub CLI on macOS, [Windows](https://github.com/cli/cli/releases/download/v2.11.3/gh_2.11.3_windows_amd64.msi), or Linux.  

```bash
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
&& sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```

3. In the command line, authenticate to GitHub.  
`$ gh auth login` 

3. Git clone alternative  
`$ gh repo clone dgucc/sandbox` 


## How-to push existing repository from command line   
Link local and remote repositories  
`$ git remote add origin https://github.com/dgucc/sandbox.git`  

Rename master into main  
`$ git branch -M main`  

Push modifications to github  
`$ git push -u origin main`

## How-to Update local repository from github  
`$ git pull origin main`  

## Open directly Git GUI - History of all branches  
`$ gitk --all`  

## Preview html page hosted in GitHub
Prepend "https://htmlpreview.github.io/?"  to url  
ex : https://htmlpreview.github.io/?https://github.com/dgucc/javascript/blob/main/d3js/map/world/index.html    

## Clone subdirectory from a github repo (Slow...)
1. URL : replace '/tree/main/' by '/trunk/' in the URL
2. Use svn command (sudo apt-get install subversion)  
3. Be patient...
`$ svn export 'https://github.com/MarimerLLC/csla/trunk/Samples/CslaFastStart'`  

## Reorganize folders in github (move...)  
`$ mkdir -p newFolder`  
`$ git mv existingFolder/ newFolder/existingFolder/`  
`$ git commit -a -m 'reorganize folders'`  
`$ git push origin main`  


