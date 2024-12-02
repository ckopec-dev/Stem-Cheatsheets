# Git Cheatsheet

 ### Necessary 

 ```
 <git add .>
 ```
 * This command adds all changes in the current directory to the staging area.
 ```
 <git init>
 ```
 * Description: Initializes a new Git repository in the current directory. This creates a hidden .git directory where all the repository's configuration files and history will be stored.
 ```
 <git clone https://github.com/user/repository.git>
 ```
 * Description: Copies an existing Git repository from a remote server to your local machine.
 ```
 <git clone https://github.com/user/repository.git my-directory>
 ```
 Optionally, specify a directory name:
 ```
 <git status>
 ```
 Description: Shows the status of changes in the working directory. It displays which files are staged, which are not, and which files are not being tracked by Git.
 ```
 <git add filename>
 ```
 Description: Adds changes in the working directory to the staging area. This is the first step in preparing changes for a commit.
 ```
 <git add> .
 ```
 To add all changes:
 
 Cloning into a New Directory and Replacing the Old One
 You can clone into a temporary directory and then replace the old directory.This approach avoids deleting the original directory until after the clone operation has succeeded.
 ```
 <git clone https://github.com/user/repository.git /path/to/new-temp-directory>
 
 rm -rf /path/to/existing/directory
 
 mv /path/to/new-temp-directory /path/to/existing/directory
 ```
 ```
 <git commit>
 ```
 
 ```
 <git push>
```


## Installation and configuration

### Installing on linux

`$ sudo apt install git`

### Show installed version

`$ git --version`

### Show all settings

`$ git config --list --show-origin`

### Configure identity

`$ git config --global user.name "John Doe"`  
`$ git config --global user.email johndoe@example.com`

## Getting and creating repos

### Turn a local directory (with or without existing code/content) into a git repo

`$ cd ~/Code/MyProject`  
`$ git init`

This creates a new folder named .git inside ~/Code/MyProject.

### Clone an existing remote repo

`$ git clone https://github.com/libgit2/libgit2`

This implicity sets the origin remotes.  
Origins are usually where the "main" code is hosted (e.g. github)

### Add an existing project to Github

~~~

# Create the repo on Github using the web UI.

$ git init
$ git add .
$ git commit -m "Add existing project files to Git"
$ git remote add origin https://github.com/{username}/example-website.git
$ git push -u -f origin master

# For username, enter {username}
# For password, enter the personal token

~~~

## Recording changes to the repo

- Staging is like a box full of changes in the repo
- Git status shows which files are in the box, and which changed files are not in the box

`$ git status`

This shows which files are in which state and also what branch you're on.

### Add a file and see how the status gets updated

`$ echo 'HelloWorld' > MyFile.txt`  
`$ git status`

Status now shows MyFile.txt is untracked.

### Track a file (add it to staging)

`$ git add MyFile.txt`  
`$ git status`

Status now shows MyFile.txt is staged (tracked).  
If the file is modified after it's been added to staging, it will be listed as both staged and unstagee.  

### Show a short status

`$ git status -s`

### Create an ignore file

`$ cat .gitignore`

Use a standard glob pattern (simmilar to regex). It will be applied recursively.  
Start a pattern with a forward slash to avoid recursivity.  
End a pattern with a forward slash to specify a directory.  
Negate a pattern by starting it with an explanation point.

### Show what is changed but not staged

`$ git diff`

### Show what is staged for the next commit

`$ git diff --staged`

### Show changes made to a specific file

`$ git diff myfile.txt`

### Show changes made to a specific directory

`$ git diff mydir`

### Compare changes to an existing commit

`$ git diff -r HEAD`

-r means compare to a particular version. HEAD is a shortcut for the for most recent commit.

### Compare changes with a specific file or directory to an existing commit

`$ git diff -r HEAD myfile.txt`

### Compare changes between two commits

`$ git diff {hash1}..{hash2}`

### Commit staged changes

`$ git commit -m "Your commit message."`

A commit is a snapshot of the code at a specific instance in time. It's like putting the box of changes in the mail. 
If -m is not specified, git will open an editor which allows a longer commit message.

## Viewing the commit history

### Show the full commit log

`$ git log`  

Lists commits in reverse chronological order (i.e. most recent commits first.)

### Show the differences in each commit (the patches)

`$ git log -p`

### Limit the number of log entries in the output

`$ git log -3`

This outputs three entries.

### Show abbreviated stats for each entry

`$ git log --stat`

### Format the log output

`$ git log --pretty=format:"%h - %an, %ar : %s"`

See [documentation](https://git-scm.com/docs/pretty-formats) for formatting options.  

## Undoing changes

### Redo a commit

`$ git commit --amend`

This takes the current staging area and uses it for the commit. If nothing has changed, the only thing that is updates is the commit message. 

### Unstage a file

`$ git reset HEAD MyFile.txt`

### Discard changes to a file since the last commit

`$ git checkout -- MyFile.txt`

## Working with remotes

Remotes are versions of your project that are hosted on the Internet or some network (or even your own local machine.) A repo can have multiple remotes, and they can be read-only or read/write to you.

### Show your remotes

`$ git remote -v`

The -v switch shows the URL associated with each remote.

### Explicity add a remote

`$ git remote add <shortname> <url>`

### Retrieve data from a remote

`$ git fetch <remote>`

A fetch just gets the data, it doesn't merge it into your local code.

### Retrieve data from your remote and merge it into your local code

`$ git pull`

This will work if you originally created your repo via a "git clone" from a remote repo, which sets your local master branch to track the remote master branch.

### Send data to a remote

`$ git push <remote> <branch>`

This will work if nobody else has done a similar push before you have, otherwise it will be rejected. To fix this, do a fetch and then a push.

### Show detailed information about a remote

`$ git remote show <remote>`

### Rename the shortname for a remote

`$ git remote rename <oldname> <newname>`

### Remove a remote 

`$ git remote remove <remote>`

## Tagging

There are two types of tags:  

- Lightweight: just a pointer to a specific commit
- Annotated: an object that contains the tagger name, email, date, and tagging message. This is the recommended type to always use. 

### List existing tags

`$ git tag`

### Search for tags that match a pattern

`$ git tag -l "v1.8.5*"`

### Create a lightweight tag

`$ git tag v1.4-lw`

### Create an annotated tag

`$ git tag -a v1.4 -m "Version 1.4"`

### Show the tag details

`$ git show v1.4`

### Tag an existing commit

`$ git tag -a v1.2 <commit checksum>`

### Push tags to remote server

`$ git push origin --tags`

### Delete a local tag

`$ git tag -d v1.4-lw`

### Delete a remote tag

`$ git push origin --delete <tagname>`

## Branching

### Show all branches

`$ git branch`

The branch prefixed with an '*' is the branch you have currently checked out.  

The --merged and --no-merged flags will filter the list to branches that you have or have not merged into your current branch.

### Show all branches with the last commit on each branch

`$ git branch -v`

### Show all differences between two branches

`$ git diff branch1 branch2`

### Create a new branch

`$ git branch <new-branch-name>`

### Switch to a different branch

`$ git checkout <branch-name>`

### Create a new branch and switch to it with one command

~~~
$ git checkout -b <new-branch-name>
# OR
$ git switch -c <new-branch-name>
~~~

### Merge a branch into your current branch

`$ git merge <branch-to-be-merged>`

### Merge a branch into another branch

~~~
$ git merge <source-branch> <destination-branch>
# OR
$ git switch <destination-branch>
$ git merge <source-branch>
~~~

### Delete a branch

`$ git branch -d <branch-to-be-deleted>`

### Get a branch from a remote repo and switch to it

~~~
$ git fetch origin
$ git checkout --track origin/<branch-name>
~~~

### Rename a branch
~~~
$ git checkout <branch-name>
$ git branch -m <new-branch-name>
# OR
$ git branch -m <original-branch-name> <new-branch-name>
~~~

## Common workflows 

### Work on a new user story/feature

* Create a new branch for the story
* Do work on the branch (coding/etc)
* Submit pull request
* Approved PR gets merged

 ### Work on a hotfix while working on a new story/feature

 * Switch to production branch
 * Create a new branch for the hotfix
 * Merge the hotfix branch
 * Push to production
 * Switch back to story branch



