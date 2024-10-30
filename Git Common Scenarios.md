
# Git Common Scenarios

## Console

### Clone an existing repo from Github

~~~
$ git clone https://github.com/ckopec-dev/Stem-Cheatsheets.git
~~~

### Create a new local repo from scratch (or existing folder)

~~~
$ mkdir MyProject
$ cd MyProject
$ git init
~~~

### Stage changes

~~~
# Create a new local repo (as per above)

# Create sample files
$ touch file1.txt
$ touch file2.txt
$ touch file3.txt

# Add a single file and show status
$ git add file1.txt
$ git status
# Add all unstaged created and modified files
$ git add .
$ git status
~~~

### Commit

~~~
# Stage changes (as per above)
$ git commit -m "Commit message - something relevant about the changes"
$ git status

# Forgot to add a file, modify the prior commit
$ touch file4.txt
$ git add .
$ git commit --ammend
~~~

### Push/Pull

~~~
# Commit changes (as per above)
# Push changes on the main branch to the repo host (e.g. Github)
$ git push origin main

# Manually create a new file on repo host and commit it
# Pull the new file
$ git pull --all
~~~

### Create a branch
### Create a PR
### Reject a PR
### Make changes and resubmitt a rejected PR 
### Approve a PR
### Merge branch 
