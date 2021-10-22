2021-10-22 14:11
#tag
# Start_git
### …or create a new repository on the command line
```shell
echo "# my_obsidian" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/Petrov-Vodkin/my_obsidian.git
git push -u origin main
```
### …or push an existing repository from the command line
```shell
git remote add origin https://github.com/Petrov-Vodkin/my_obsidian.git
git branch -M main
git push -u origin main
```
### …or import code from another repository

You can initialize this repository with code from a Subversion, Mercurial, or TFS project.
_____________
#### Links
