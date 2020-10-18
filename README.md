# Transferring a Github Repo (with Commit History) while Hiding Sensitive Commit History

It's been quite an adventure trying to transfer my files over from my General Assembly Github enterprise account after the course was over, and I'm sure other people are having similar issues trying to get things transferred over and configured properly, so here's a quick post that should go through the steps for you (all the steps).

## Open up Git Bash or Terminal


```Bash
# Mirror the original repo, choosing GA_projects as the folder to clone the repo to on my local machine
my_username@my_computer_name /g/DS/DSIR_GH
$ git clone --mirror [old_repo_link] GA_projects

# Go into new project directory
my_username@my_computer_name /g/DS/DSIR_GH
$ cd GA_projects/

# You may get a warning after this code
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (BARE:master)
$ git remote remove origin

# Go to github.com and make a new repo where you'll store all of this
# Add new repo link from your personal Github
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (BARE:master)
$ git remote add origin [new_repo_link]

# Pushing all to new repo
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (BARE:master)
$ git push --all

# Pushing tags to new repo
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (BARE:master)
$ git push --tags

# Exit the repo, go back up one folder
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (BARE:master)
$ cd ..

# Remove the repo from your local machine
my_username@my_computer_name /g/DS/DSIR_GH
$ rm -rf GA_projects

# Clone the new repo (from your personal Github) to a folder on your original machine (again, I'm going to call it GA_projects)
my_username@my_computer_name /g/DS/DSIR_GH
$ git clone [new_repo_link] GA_projects

# Go back into that folder
my_username@my_computer_name /g/DS/DSIR_GH
$ cd GA_projects/

# Remove the cached files in repo so you can update a .gitignore, the . means all
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ git rm --cached -r .

# Create .gitignore
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ touch .gitignore

# Edit .gitignore to include the folders you need to hide (for me it was labs/, quizzes/, and .ipynb_checkpoints/)
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ atom .gitignore

my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ git add .

my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ git commit -m 'removed irrelevant folders'

my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ git push

# Use filter-branch to remove all commits involving the directory 'labs'
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ git filter-branch --tree-filter "rm -rf labs" --prune-empty HEAD

# To remove all commits containing the directory 'quizzes'
my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ git filter-branch --tree-filter "rm -rf quizzes" -f --prune-empty HEAD

my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (master)
$ git checkout -b clean

my_username@my_computer_name /g/DS/DSIR_GH/GA_projects (clean)
$ git push origin clean

```

## Head to Github, Change your Default Branch!

Now you'll need to change the default branch to the new branch you just made (we just called it clean)

You can do this through the settings on your repo via github.com, but for step-by-step instructions, go to:

https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/changing-the-default-branch

## Back to Git Bash or Terminal

```Bash
# Delete the master branch (repeat this if there are other branches that still contain old commits)
iplay@DESKTOP-I4VPRR3 MINGW64 /g/DS/DSIR_GH/GA_projects (clean)
$ git push origin --delete master

# Rename the branch "clean" as "main" for consistency
iplay@DESKTOP-I4VPRR3 MINGW64 /g/DS/DSIR_GH/GA_projects (clean)
$ git branch -m clean main
```

## You're all done!
You should now have a git repo that has all the folders you want while hiding that sweet, sweet, proprietary information!