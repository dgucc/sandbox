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

[GIT avancé : stash, revert, restore, reset](https://www.youtube.com/watch?v=Ayr17xFKMHU)

## git commit --amend
Modifier message du dernier commit  
`git commit --amend -m 'message corrigé'` 


## git stash : mettre de côté   
## git stach pop : pour récupérer ce qui a été mis de côté  
Mettre de côté ses modifs pour intervenir sur une autre branch  
```
	$ git stash
	$ git checkout main
	$ git add . 
	$ git commit -m 'commit dans le main'
	$ git checkout dev
	$ git stash pop
```

## git restore
Sortir des fichiers de la staging area  
	`$ git restore --staged index.html` 

Annuler les modifs uncommited et revenir à l'état du dernier commit  
	`$ git restore --source=HEAD --staged --worktree index.html` 

## git revert (new commit)
Nouveau commit pour annuler et revenir au commit précédent  
	`$ git revert <hash du commit à annuler>` 

## git reset 
Attention: reset (--soft --mixed et --hard) et rebase modifient l'histoire  
Revenir à un commit précédent avec les fichiers modifiés dernièrement (en --unstaged)  
HEAD~1 : un commit en arrière  
	`$ git reset HEAD~1`   

Revenir strictement à un commit sans aucun fichier modifié  
	`$ git reset --hard HEAD~1`  

## git rebase, cherry-pick	
-i : Mode interactif  
	`$ git rebase -i HEAD~6` 

---------------------------------------
`$ git diff HEAD`  

# discard local changes
`$ git checkout -f HEAD`  
`$ git reset --hard HEAD~1` 

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
`git --no-pager log --pretty=format:"%cs: %s [%H]"` 

Git GUI : visualize all branch history  
`gitk --all` 

## Preview html page hosted in GitHub
Prepend **https://htmlpreview.github.io/?**  to url  
ex : https://htmlpreview.github.io/?https://github.com/dgucc/javascript/blob/main/d3js/map/world/index.html    
