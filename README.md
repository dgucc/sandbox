# Sandbox

## GitHub CLI - Getting started

Command ***gh*** needed to push modifications to remote repository
1. Install GitHub CLI on macOS, [Windows](https://github.com/cli/cli/releases/download/v2.11.3/gh_2.11.3_windows_amd64.msi), or Linux.  
2. In the command line, authenticate to GitHub.  
`$ gh auth login` 

## How to push existing repository from command line
```
$ git remote add origin https://github.com/dgucc/sandbox.git  
$ git branch -M main  
$ git push -u origin main
```
