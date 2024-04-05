# GIT

## GitHub - Getting started

### Command gh needed to push modifications to remote repository  
Install GitHub CLI on macOS, Windows, or Linux.  

```
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
&& sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```

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

How-to git pull over all subdirectories  
```bash
#!/bin/bash
find . -type d -name .git -exec git --git-dir={} --work-tree=$PWD/{}/.. pull origin main \;
```

## Resolve conflict with binary files

```bash
$ git checkout branch1
$ git merge branch2
# Merge conflict in book.xlsx...
$ git checkout --theirs book.xls
$ git add book.xls
$ git commit -m 'Resolve conflict by keeping the branch2 version'
$ git merge branch2
```

## Git Pull Force

[Git Pull Force – How to Overwrite Local Changes With Git](https://www.freecodecamp.org/news/git-pull-force-how-to-overwrite-local-changes-with-git/)

`@{u}` : shortcut for `$CURRENT_BRANCH`  

```bash
git fetch
git reset --hard HEAD
git merge '@{u}'
```

---

## Clone subdirectory only

```bash
git clone -n --depth=1 --filter=tree:0 https://github.com/dgucc/javascript 
cd javascript
git sparse-checkout set --no-cone openstreetmap
git checkout
```

## Misc

Commit and add all files in a single command  
`$ git commit -a -m 'Take all'`  

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
