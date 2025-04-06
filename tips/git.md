<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [GIT](#git)
   * [GitHub - Getting started](#github-getting-started)
      + [Command gh needed to push modifications to remote repository  ](#command-gh-needed-to-push-modifications-to-remote-repository)
      + [How-to push existing repository from command line  ](#how-to-push-existing-repository-from-command-line)
   * [Resolve conflict with binary files](#resolve-conflict-with-binary-files)
   * [Git Pull Force](#git-pull-force)
   * [git commit --amend](#git-commit-amend)
   * [git stash : mettre de côté   ](#git-stash-mettre-de-côté)
   * [git stach pop : pour récupérer ce qui a été mis de côté  ](#git-stach-pop-pour-récupérer-ce-qui-a-été-mis-de-côté)
   * [git restore](#git-restore)
   * [git revert (new commit)](#git-revert-new-commit)
   * [git reset ](#git-reset)
   * [git rebase, cherry-pick	](#git-rebase-cherry-pick)
- [discard local changes](#discard-local-changes)
   * [Clone subdirectory only](#clone-subdirectory-only)
   * [Misc](#misc)
   * [Log](#log)
   * [Show git branch in Windows Command Prompt ](#show-git-branch-in-windows-command-prompt)
   * [Show git branch in Linux bash Prompt](#Show-git-branch-in-Linux-bash-Prompt)
   * [Preview html page hosted in GitHub](#preview-html-page-hosted-in-github)

<!-- TOC end -->

<!-- TOC --><a name="git"></a>
# GIT

<!-- TOC --><a name="github-getting-started"></a>
## GitHub - Getting started

[Bien démarrer avec GitHub](https://docs.github.com/fr/get-started)  

<!-- TOC --><a name="command-gh-needed-to-push-modifications-to-remote-repository"></a>
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

<!-- TOC --><a name="how-to-push-existing-repository-from-command-line"></a>
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

---

<!-- TOC --><a name="resolve-conflict-with-binary-files"></a>
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

<!-- TOC --><a name="git-pull-force"></a>
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

<!-- TOC --><a name="git-commit-amend"></a>
## git commit --amend
Modifier le dernier commit  (message et autres)
`git commit --amend -m 'message corrigé'` 


<!-- TOC --><a name="git-stash-mettre-de-côté"></a>
## git stash : mettre de côté   
`$ git stash` 

<!-- TOC --><a name="git-stach-pop-pour-récupérer-ce-qui-a-été-mis-de-côté"></a>
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

<!-- TOC --><a name="git-restore"></a>
## git restore
Sortir des fichiers de la staging area  
	`$ git restore --staged index.html` 

Annuler les modifs uncommited et revenir à l'état du dernier commit  
	`$ git restore --source=HEAD --staged --worktree index.html` 

Récupérer fichiers supprimés par erreur  
	`$ git checkout -f` 


<!-- TOC --><a name="git-revert-new-commit"></a>
## git revert (new commit)
Nouveau commit pour annuler et revenir au commit précédent  
	`$ git revert <hash du commit à annuler>` 

<!-- TOC --><a name="git-reset"></a>
## git reset 
Attention: reset (--soft --mixed et --hard) et rebase modifient l'histoire  
Revenir à un commit précédent avec les fichiers modifiés dernièrement (en --unstaged)  
HEAD~1 : un commit en arrière  
	`$ git reset HEAD~1`   

Revenir strictement à un commit sans aucun fichier modifié  
	`$ git reset --hard HEAD~1`  

<!-- TOC --><a name="git-rebase-cherry-pick"></a>
## git rebase, cherry-pick	
-i : Mode interactif  
	`$ git rebase -i HEAD~6` 

---------------------------------------
`$ git diff HEAD`  

<!-- TOC --><a name="discard-local-changes"></a>
# discard local changes
`$ git checkout -f HEAD`  
`$ git reset --hard HEAD~1` 

---

<!-- TOC --><a name="clone-subdirectory-only"></a>
## Clone subdirectory only

```bash
git clone -n --depth=1 --filter=tree:0 https://github.com/dgucc/javascript 
cd javascript
git sparse-checkout set --no-cone openstreetmap
git checkout
```

<!-- TOC --><a name="misc"></a>
## Misc

Commit and add all files in a single command  
`$ git commit -a -m 'Take all'`  

Remove unstaged files  
`$ git clean -df`  

Remove non commited change  
`$ git reset --hard`  

<!-- TOC --><a name="log"></a>
## Log

Display comments only  
%s for the subject  
%b for the body  
%B for both  
`git --no-pager log --pretty=format:"%cs: %s [%H]"` 

Display Graph [Mnemonic : no-pager log A DoG]  
%h for sha1 hash  
%ad for date  
`git --no-pager log --all --decorate --graph --pretty="%C(blue)%h %C(yellow)%s %C(white)%ad" --date=short`  

> ![git-log-graph](https://github.com/dgucc/sandbox/blob/main/tips/images/git-log-graph.png)


Git GUI : visualize all branch history  
`gitk --all` 

<!-- TOC --><a name="show-git-branch-in-windows-command-prompt"></a>
## Show git branch in Windows Command Prompt 
[stackoverflow](https://stackoverflow.com/questions/36047706/show-current-git-branch-name-in-windows-command-prompt)  

1. C:\bin\cd-git.bat :  
```bat
@echo off
CD %*
where git >nul 2>&1
if %errorlevel% neq 0 (
	goto :eof
)
for /f "usebackq tokens=* delims=" %%g in (`git rev-parse --is-inside-work-tree ^>nul 2^>^&1 ^&^& git branch --show-current`) do (
	set branchname=%%g
)
if "%branchname%"=="" (
	prompt $p$g
) else (
	prompt $p $c$e[36m%branchname%$e[0m$f$_$$$s
)
set branchname=
```

2. AutoRun

`> regtool -w -s set '/HKCU/Software/Microsoft/Command Processor/AutoRun' 'if exist C:\bin\cd-git.bat doskey cd=C:\bin\cd-git.bat $*'`  

3. References  
[AutoRun](https://ss64.com/nt/syntax-autoexec.html)  
[Doskey](https://superuser.com/questions/118655/auto-execute-command-after-going-to-a-folder-with-the-cd-command)  
[Prompt](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/prompt)  

## Show git branch in Linux bash Prompt 

in ~/.bashrc :  
```bash
# https://thucnc.medium.com/how-to-show-current-git-branch-with-colors-in-bash-prompt-380d05a24745
parse_git_branch() {
   git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\n\[\e[38;5;220m\]$(parse_git_branch)\[\e[00m\]$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```


<!-- TOC --><a name="preview-html-page-hosted-in-github"></a>
## Preview html page hosted in GitHub
Prepend **https://htmlpreview.github.io/?**  to url  
ex : https://htmlpreview.github.io/?https://github.com/dgucc/javascript/blob/main/d3js/map/world/index.html    
