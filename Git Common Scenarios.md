
# Git Common Scenarios

## Console

### Clone an existing repo from Github

~~~bash
git clone https://github.com/ckopec-dev/Stem-Cheatsheets.git
~~~

### Create a new local repo from scratch (or existing folder)

~~~bash
mkdir MyProject
cd MyProject
git init
~~~

### Stage changes

~~~bash
# Create a new local repo (as per above)

# Create sample files
touch file1.txt
touch file2.txt
touch file3.txt

# Add a single file and show status
git add file1.txt
git status
# Add all unstaged created and modified files
git add .
git status
~~~

### Commit

~~~bash
# Stage changes (as per above)
git commit -m "Commit message - something relevant about the changes"
git status

# Forgot to add a file, modify the prior commit
touch file4.txt
git add .
git commit --ammend
~~~

### Push/Pull

~~~bash
# Commit changes (as per above)
# Push changes on the main branch to the repo host (e.g. Github)
git push origin main

# Manually create a new file on repo host and commit it
# Pull the new file
git pull --all
~~~

### Create/Merge a branch

~~~bash
# Create a branch named my_new_feature
git branch my_new_feature

# Show the main branch and the new branch we just created. The asterisk next to the branch name means it's the current branch.
git branch

# Switch to our new branch
git checkout my_new_feature

# Add a new file and commit it
touch file5.txt
git add .
git commit -m 'Added file5.txt'

# Switch back to main branch
git checkout main

# Merge changes from the new branch into main and push changes
git merge my_new_feature
git push

# Delete feature branch now that it's been merged
git -d my_new_feature
~~~
