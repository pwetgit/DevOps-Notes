![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/320px-Git-logo.svg.png)


## 🧑🏻‍💻 Useful commands
```bash

# Configuration
git clone git@github.com:nomduGituser/repo.git
cd repo.git
git config --global user.name “Jean Git”
git config --global user.email “JeanGit@truc.com”

# Modifier le fichier README par exemple
git status 
git diff README.md
git add README.md
git status 
git commit -m “updated readme file, initial commit”
git push origin master

# Show all branch
git branch -a 

# Show remote branch
gir branch -r

# Delete remote branch
git push origin -d <branch_name>

# Renaming a remote Git branch
git push origin --delete old-name
git push origin -u new-name
perform a reset of the upstream branch

# Remove/restore file from previous/specific commit
git checkout commit_hash file

```
