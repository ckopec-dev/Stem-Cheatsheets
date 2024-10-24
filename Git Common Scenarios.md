
# Git Common Scenarios

## Create a new local repo from scratch (or existing folder)

### Console

~~~
$ mkdir MyProject
$ cd MyProject
$ git init
~~~

### VSCode

- Create or open a folder that will contain your repo
- Open it in VSCode (File / Open Folder)
- In Activity Bar: Choose Source Control, then click "Initialize Repository"

### Visual Studio 2022

- File / New / Repository...
- Chose "Local only"
- Fill out other options as desired
- Click the "Create" button

### Jetbrains Rider

- Click "New Solution" button
- Select "Empty Solution"
- Fill out the other options as desired
- Make sure "Create Git repository" is checked
- Click the "Create" button

## Clone an existing repo from Github

### Console

~~~
$ git clone https://github.com/ckopec-dev/Stem-Cheatsheets.git
~~~

### VSCode

- In Activity Bar: Choose Source Control, then click "Clone Repository"
- Enter url of repo
- Select folder to clone into (the parent folder where the repo folder will be created)

### Visual Studio 2022

- Git / Clone Repository
- Where prompted, enter URL and Path (the folder where the repo will be created)

### Jetbrains Rider

- With "Projects" selected, Click "Get from VCS" button
- Enter url of repo
- Select folder to clone into (the parent folder where the repo folder will be created)
  
## Stage changes
## Commit
## Push/pull
## Create a branch
## Create a PR
## Reject a PR
## Make changes and resubmitt a rejected PR 
## Approve a PR
## Merge branch 
