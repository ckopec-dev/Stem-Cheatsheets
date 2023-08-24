
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

### Update Salesforce CLI
`$ sudo npm install --global @salesforce/cli`

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

### Create an Apex class

- Under force-app/main/default, right-click classes and select SFDX: Create Apex Class.

### Deploy class to org

- Right-click the class and select SFDC: Deploy Source to Org.

### Create a Lightning Web Component

- Under force-app/main/default, right-click lwc folder and select SFDX: Create Lightning Web Component.

### Deploy LWC to org

- Right-click the lwc's folder and select SFDC: Deploy Source to Org.

## Apex

### Comments

~~~
// This is a single-line comment.
/* This is a
   multiple-line comment. */
~~~

### Data types

- Primitives
  - Integer
  - Double
  - Long
  - Date
  - Datetime
  - String
  - ID
  - Boolean
- sObject (either generic or specific, such as Account, Contact, etc)
- Collections
  - List
  - Set
  - Map
- Enum: typed list of values
- User-defined Apex classes
- System-suppled Apex classes

### Lists

Hold ordered collections of objects. Synonymous and interchangable with arrays.

~~~
List<String> colors = new List<String>();
// Equivalent to:
String[] colors = new List<String();

// Add item to list
colors.add('red');

// Iterate over a list
for(Integer i = 0; i < colors.size(); i++) {
 System.debug(colors[i]);
}
~~~

### Classes

~~~
public class EmailMananger {

  // Public method
  public void SendEmail(String address, String subject, String body) {
    // Do something
  }

  public static void ShowLog() {
    // Do something
  }

  // Private static method
  private static Boolean CheckResult() {
    // Do something
  }

}

// Call a static method
EmailManager.ShowLog();
~~~

### sObjects

Every record in SFDC is represented as an sObject in Apex.

~~~
// Create a sObject variable
Account acct = new Account(Name='Acme');

// Field names are referenced via their API names.
// Custom fields have a '__c' suffix.
// Custom relationships have a '__r' suffix.
acct.Phone = '(555)555-5555';

// Cast a generic sObject to a specific type
Account acct = (Account)mySObject;
~~~

### DML

There is a DML limit of 150 statements per Apex transaction.

- insert
- update
- upsert
- delete
- undelete
- merge

~~~
// Create the account sObject 
Account acct = new Account(Name='Acme', Phone='(415)555-1212', NumberOfEmployees=100);

// Insert the account by using DML
insert acct;

// Get the new ID on the inserted sObject argument
ID acctID = acct.Id;

// Display this ID in the debug log
System.debug('ID = ' + acctID);

// Debug log result (the ID will be different in your case)
// DEBUG|ID = 001D000000JmKkeIAF
~~~

### Bulk DML

~~~
// Create a list of contacts
List<Contact> conList = new List<Contact> {
    new Contact(FirstName='Joe',LastName='Smith',Department='Finance'),
        new Contact(FirstName='Kathy',LastName='Smith',Department='Technology'),
        new Contact(FirstName='Caroline',LastName='Roth',Department='Finance'),
        new Contact(FirstName='Kim',LastName='Shain',Department='Education')};

// Bulk insert all contacts with one DML call
insert conList;

// List to hold the new contacts to update
List<Contact> listToUpdate = new List<Contact>();

// Iterate through the list and add a title only if the department is Finance
for(Contact con : conList) {
    if (con.Department == 'Finance') {
        con.Title = 'Financial analyst';
        // Add updated contact sObject to the list.
        listToUpdate.add(con);
    }
}

// Bulk update all contacts with one DML call
update listToUpdate;
~~~

### Upserts

~~~
// Insert the Josh contact
Contact josh = new Contact(FirstName='Josh',LastName='Kaplan',Department='Finance');       
insert josh;

// Josh's record has been inserted so the variable josh has now an ID which will be used to match the records by upsert.
josh.Description = 'Josh\'s record has been updated by the upsert operation.';

// Create the Kathy contact, but don't persist it in the database
Contact kathy = new Contact(FirstName='Kathy',LastName='Brown',Department='Technology');

// List to hold the new contacts to upsert
List<Contact> contacts = new List<Contact> { josh, kathy };

// Call upsert

upsert contacts;
// Result: Josh is updated and Kathy is created.
~~~

### DML Exceptions

~~~
try {
    // This causes an exception because the required Name field is not provided.
    Account acct = new Account();

    // Insert the account 
    insert acct;
} catch (DmlException e) {
    System.debug('A DML exception has occurred: ' + e.getMessage());
}
~~~

### Database methods

Similar to DML statements, but allow partial success.

- Database.insert()
- Database.update()
- Database.upsert()
- Database.delete()
- Database.undelete()
- Database.merge()

~~~
// Create a list of contacts
List<Contact> conList = new List<Contact> {
        new Contact(FirstName='Joe',LastName='Smith',Department='Finance'),
        new Contact(FirstName='Kathy',LastName='Smith',Department='Technology'),
        new Contact(FirstName='Caroline',LastName='Roth',Department='Finance'),
        new Contact()};

// Bulk insert all contacts with one DML call
Database.SaveResult[] srList = Database.insert(conList, false);

// Iterate through each returned result
for (Database.SaveResult sr : srList) {
    if (sr.isSuccess()) {
        // Operation was successful, so get the ID of the record that was processed
        System.debug('Successfully inserted contact. Contact ID: ' + sr.getId());
    } else {
        // Operation failed, so get all errors
        for(Database.Error err : sr.getErrors()) {
            System.debug('The following error has occurred.');
            System.debug(err.getStatusCode() + ': ' + err.getMessage());
            System.debug('Contact fields that affected this error: ' + err.getFields());
	 }
 }
}
~~~
