
# Salesforce Cheatsheet

## Development tools

### Install Visual Studio Code

- Download: [https://code.visualstudio.com/download](https://code.visualstudio.com/download)
- On Ubuntu, easiest way to install is via the Software Center.
- After installation, install extension pack: Salesforce Extension Pack (Expanded)

### Install Java Runtime Environment

`$ sudo apt install default-jre`

### Install Salesforce CLI

- Download: [https://developer.salesforce.com/tools/salesforcecli](https://developer.salesforce.com/tools/salesforcecli)

 `$ npm install @salesforce/cli --global`

### Check CLI version

`$ sfdx --version`
  
### Update 

`$ sudo snap refresh`  
`$ sudo apt update`  
`$ sudo apt upgrade`  

## Coding with VS Code

### Create a new Salesforce project

- In the command palette, select "SFDX: Create Project".
- Accept the Standard option.
- Enter a project name.
- Select a folder. A folder with the project's name will be created inside this folder.
- Click Create Project.

### Authorize an org

- In the command palette, select "SFDX: Authorize an Org".
- Choose Production and then provide an alias.
- Your browser will open with a credential screen. Enter your username and password.

### Open the org

- There is a small "Open Org" icon that looks like a window in the bottom left of VS Code (to the left of "Quick Badges"). Click to open and it will open the org in a new browser window.

### Retrieve metadata from the org

- In the VS Code Activity Bar (big icons on the left), click the cloud "Org Browser" icon.
- For a custom object's metadata, expand "Custom Objects" and find the object of interest. Select it, and click the icon to the right of the name "Retrieve Source from Org".
- Object metadata is now available in your project in "force-app/main/default/objects".

