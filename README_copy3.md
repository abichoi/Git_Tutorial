# Git Guide
## Install Git
Go to the Git website https://git-scm.com/install/windows and download the correct installer for your system.
## Set user.name/ user.email
To set your Git username and email for every repository on your laptop. You can use your GitHub-provided noreply email address or any email address that is **linked to your GitHub account.** Remember to toggle on "Keep my email addresses private" if you are using your private email and would like to hide it
```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
```
Check if the setting is correct
```bash
git config --global user.name
git config --global user.email
```
```Output
Your Name
youremail@domain.com
```

## Fork the repo
Go to GitHub and fork this repo https://github.com/abichoi/Git_Tutorial.git

## Clone a repo (HTTPS)
```bash
git clone <your-forked-repo-url>

# or specify destination path
git clone <your-forked-repo-url> <an/empty/destination/folder/path>

```

## Use SSH keys to clone/push
### Create a ssh key
```bash
ssh-keygen -t rsa -b 4096 -C "youremail@domain.com"
```
When prompted, you can specify where to save the key or press "Enter" to save it to the default location ('~/.ssh/id_rsa')

### Add SSH key to ssh-agent
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa #change the address to where your ssh key is
```

### Add SSH key to yuor GitHub
1. Go to your GitHub settings page
2. Under SSH and GPG keys session, click on New SSH key
3. Copy your public key from your id_rsa.pub file and paste it onto GitHub and SAVE it.

### Clone repo using SSH
```bash
git clone git@github.com:Dkaka/COMP0219_Tutorials.git
```
## Stage/commit with meaningful messages
```bash
#cd to where your repo is cloned to
cd <path/of/cloned/repo>
git status
touch code.py
git add <path/to/code.py>     
git commit -m "add code file"
git push
```

## Create/switch branches (switch/checkout)
```bash
# create a new branch (change new_branch_name to your desired name)
git checkout -b new_branch_name
#push the new branch to the repo
git push origin new_branch_name
#switch back to your main branch
git switch main
```

## Merge and resolve a simple conflict
```bash
# switch to branch
git switch new_branch_name
# To create a change in the new branch, we will duplicate the readme file
cp -R README.md README_Copy1.md
git add README_Copy1.md
git commit -m "add read me copy 1"

git switch main
git merge new_branch_name

# push if there is no conflict
git push
```
### If conflicts happen
1. Edit the conflicted files.
2. Then:
```bash
git add <conflicted-file>
git commit
git push
```

## Rebase my branch onto main safely
This takes the commits that exist on new_branch_name but not on main, and puts them on top of the main. 

Let's say the original state of the repo is like this:

```
main: A---B---C
             \
new_branch_name:   D---E
```
Then your groupmate updated the main branch
```
main: A---B---C---F---G
             \
new_branch:   D---E
```

After running the code below, you will get this:
```
main: A---B---C---F---G
                     \
new_branch:           D'---E'
```

```bash
git checkout new_branch_name
git rebase main # you should see code.py being added to your branch

## If conflicts happen, edit the conflicted files, then:
git add <fixed-file>
git rebase --continue
## If you need to back out:
git rebase --abort
# Force-with-lease updates your remote branch safely:
git push --force-with-lease
```

## Open a Pull/Merge Request and request review
1. Push your branch
```bash
git push -u origin new_branch_name
```
2. Open a Pull/Merge Request in the "Pull requests" tab in the repo by clicking the "Create Pull Request" button
3. Select your main branch as the base branch and new_branch_name as the compare branch
4. Click "Create pull request"

## Use .gitignore effectively
```bash
# Create a gitignore file
touch .gitignore

# Create an example file to ignore
touch secret.py
```
Then, open .gitignore in an editor you prefer and add the following to .gitignore

```bash
secret.py
# if you want git to ignore a folder, you can add
secret_folder/
```

```bash
git add .
git commit -m "upload gitignore and copy 2"
```

## Undo safely (revert, not reset --hard on shared history)
### git reset -- Rewrite/ move history
git reset moves your branch's HEAD, and possibly updates the stage (index) and working tree.
Example:
```bash
# Uncommit but keep changes staged (i.e. your changes won't be lost and are ready to be committed again)
git reset --soft HEAD~1
```
### git restore -- Safely undo changes to files
git restore was introduced to make Git safer and reduce accidental data loss.
Example:
```bash
# Unstage a file but keep changes 
git restore --staged path/to/file
# Discard local changes in a file
git restore path/to/file
```
### git revert -- Safely undo committed history
git revert creates a new commit that undoes the changes from a previous commit.
Example:
```bash
# Revert a commit
git revert <commit-sha>
```

## Stash and apply work
git stash is used to save changes that are not ready to be committed
```bash
# Add your changes to the current branch 
git switch new_branch_name
cp -R README.md README_Copy3.md
git add .
# Stash your changes
git stash
# Switch to another branch (we use the main branch here)
git checkout main
# Edit the main branch by duplicating a file and commit the edit
cp -R README.md README_Copy4.md
git add .
git commit -m "Add copy 4"
git push main
# Switch back to the original branch
git checkout new_branch_name
# Retrieve your stashed changes
git stash pop
git commit -m "add copy 3"
git push 
```

## Inspect history (log, show, blame)
```bash
# List all the commits made in the repo
git log # type 'q' to exit the current screen
# List all the commits and show the difference introduced in each commit in a specific file
git log -p path/to/file
# Show the most recent commit on the current branch
git show
# Show a specific commit        
git show <commit-sha>
# Show the author name and commit information of each line of a specific file
git blame path/to/file     
```            

## Use tags/releases
```bash
# List out all the tags
git tag 
# Create a tag and specify a tagging message using -m
git tag -a v0.1 -m "Tutorial"
# Show the tag data
git show v0.1
# By default, the git push command doesn't transfer tags to GitHub. You have to specificly push the tags to Github by:
git push origin v0.1
#or use --tags if you have a lot of tags to push all at once
git push origin --tags
```

## Submodule vs Subtree
Submodule links two repos and subtree merges two repos.
Git submodule lets you include a submodule within your repo, where the submodule is a reference to a specific commit in another repo.
Git Subtree lets you include a subdirectory within another repo, while still being able to push and pull changes to and from the subtree's repository. 

### Fork the sub repo
https://github.com/abichoi/Git_Tutorial_sub_repo.git

### Handle submodules
A git submodule is a repo embedded inside another repo. 
```bash
# Add the submodule
git switch main
git submodule add <submodule-repo-url><path-the directory-where-the-submodule should-be-added>
# initialise and fetch the submodule
git submodule update --init
```
If you want to clone a repo with a submodule:
```bash
git clone --recurse-submodules <repository-url>
```

To commit changes in submodules
```bash
cd <submodule-path>
# make changes and commit
git add.
git commit -m "Changed submodule content"
```

### Handle subtrees
Git Subtree is used to include a repo as a subdirectory within another repo.

```bash
git switch new_branch_name
# A tree cannot be added if there are uncommitted changes
git add .
git commit -m "commit changes before subtree pull"
# Add a subtree to the parent repository
git remote add remote-name <path/of/cloned/sub/repo>
# Clone your cloned sub repo into the subtreeDirectory folder
git subtree add --prefix=subtreeDirectory <path/of/cloned/sub/repo> main --squash

# Pull changes from the subtree repo to subtreeDirectory
git subtree pull --prefix subtreeDirectory <path/of/cloned/sub/repo> master --squash
# Push changes from the subtreeDirectory to the subtree repo
git subtree push --prefix subtreeDirectory <path/of/cloned/sub/repo> master
```
