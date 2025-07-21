
# Salesforce Cheatsheet

## Scratch orgs

A dedicated, configurable, and short-term Salesforce environment that you can quickly spin up when starting a new project, a new feature branch, or a feature test.

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

### View logs

- In the command palette, select "SFDX: Turn On Apex Debug Log for Replay Debugger".
- Reproduce the issue for which you want to review log info.
- In the command palette, select "SFDX: Get Apex Debug Logs".
- Select the log you wish to view.

### Run unit tests

[https://developer.salesforce.com/tools/vscode/en/apex/testing](https://developer.salesforce.com/tools/vscode/en/apex/testing)
  
## Apex

### Comments

~~~
// This is a single-line comment.
/* This is a
   multiple-line comment. */
~~~

### Data types

- Primitives
  - Integer (-2,147,483,648 to 2,147,483,647)
  - Double
  - Long (64 bit
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
- User-defined Apex classes, objects, and interfaces
- System-supplied Apex classes

~~~
/* Examples */
Integer i = 100;
system.debug('value of i: ' + i);
Boolean active = true;
Date dtShipped = date.today();
Long debt = 100000000000000L;
Account acct = new Account(Name = 'ACME');
MyClass cls = new MyClass();
String name = 'Bob';
public enum Status {Open, Closed}
Status s = Status.Open;
~~~

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

## SOQL

To include SOQL queries within your Apex code, wrap the SOQL statement within square brackets and assign the return value to an array of sObjects.  
You must specify every field you want to get explicitly.  

`Account[] accts = [SELECT Name,Phone FROM Account];`

### Filter results

`SELECT Name,Phone FROM Account WHERE (Name='SFDC Computing' OR (NumberOfEmployees>25 AND BillingCity='Los Angeles'))`

### Order results

`SELECT Name,Phone FROM Account ORDER BY Name DESC`

### Limit results

`Account oneAccountOnly = [SELECT Name,Phone FROM Account LIMIT 1];`

### Access Apex variables

~~~
String targetDepartment = 'Wingo';
Contact[] techContacts = [SELECT FirstName,LastName 
                          FROM Contact WHERE Department=:targetDepartment];
~~~

### Query related records

~~~
Account[] acctsWithContacts = [SELECT Name, (SELECT FirstName,LastName FROM Contacts)
                               FROM Account 
                               WHERE Name = 'SFDC Computing'];
// Get child records
Contact[] cts = acctsWithContacts[0].Contacts;
System.debug('Name of first associated contact: ' 
             + cts[0].FirstName + ', ' + cts[0].LastName);

// Get parent fields
Contact[] cts = [SELECT Account.Name FROM Contact 
                 WHERE FirstName = 'Carol' AND LastName='Ruiz'];
Contact carol = cts[0];
String acctName = carol.Account.Name;
System.debug('Carol\'s account name is ' + acctName);
~~~

## SOSL

Used to perform text searches in records.

### Basic syntax in Apex

`FIND 'SearchQuery' [IN SearchGroup] [RETURNING ObjectsAndFields]`

### Basic syntax in Query Editor and API

`FIND {SearchQuery} [IN SearchGroup] [RETURNING ObjectsAndFields]`

### SearchGroup (optional)

- ALL FIELDS
- NAME FIELDS
- EMAIL FIELDS
- PHONE FIELDS
- SIDEBAR FIELDS

### Apex example

~~~
String soslFindClause = 'Wingo OR SFDC';
List<List<sObject>> searchList = [FIND :soslFindClause IN ALL FIELDS
                    RETURNING Account(Name),Contact(FirstName,LastName,Department)];
Account[] searchAccounts = (Account[])searchList[0];
Contact[] searchContacts = (Contact[])searchList[1];
System.debug('Found the following accounts.');
for (Account a : searchAccounts) {
    System.debug(a.Name);
}
System.debug('Found the following contacts.');
for (Contact c : searchContacts) {
    System.debug(c.LastName + ', ' + c.FirstName);
}
~~~

## Lightning Web Components

[Component Library](https://developer.salesforce.com/docs/component-library/overview/components)  
[Lightning Web Components Recipes](https://github.com/trailheadapps/lwc-recipes)  
[E-Bikes Demo](https://github.com/trailheadapps/ebikes-lwc)  

### Hello world example

~~~
<!-- Html -->
<template>
  <input value={message}></input>
</template>

<!-- Javascript -->
import { LightningElement } from 'lwc';
export default class App extends LightningElement {
  message = 'Hello World';
}

/* Css */
input {
  color: blue;
}
~~~

### Events

- Information can be passed up using events and event listeners.
	- The child component dispatches the event and the parent component listens for it. Dispatching the event includes creating an event object the child can pass to the parent component. The parent has a handler to respond to the event.
- Information can be passed down using public properties and public methods.
	- You can make a component property public by prefacing it with the @api decorator. Then, set the public property by an external component.

## Rest API

- User must have API Enabled permission in their profile. For Developer edition, this is enabled by default.
- Some things you can do with the API:
	- CRUD records (incl querying + searching)
 	- Retrieve metadata
  	- Retrieve instance configuration info
- Elements used in a request:
	- URI, basic structure: https://MyDomainName.my.salesforce.com/services/data/vXX.X/resource/
 	- Headers: used to pass params and options, e.g.:
  		- Http Accept: format the cllient accepts in the response body, e.g. application/json
    		- Http Content-type: format of the request body, e.g. application/json
        	- Http Authorization: provides OAuth 2.0 token to authorize the client
           	- Compression header: compresses the request or response
           	- Conditional header request: validates records against a precondition
  	- Http method: HEAD, GET, POST, PATCH, PUT, and DELETE
  	- Request body: when attaching json with cURL, use -data-binary option
  - Requests can use 15 or 18 character IDs, but responses always have 18 character IDs
 
  ## Create a Connected App for API usage

  - Go to Setup / Apps / App Manager
  - Click the "New Connected App" button
  - Enter a Connected App Name, e.g. API Demo
  - API Name should auto-populate
  - Enter your email for Contact Email
  - Make sure Enable OAuth Settings is checked
  - For Callback URL, enter https://localhost
  - For Selected OAuth Scopes, select "Manage user data via APIs (api)"
  - For Permitted Users, select "All users may self-authorize"
  - Click the Save button
 
    ## Retrieve and use and access token

~~~
# Request:
$ curl -d 'grant_type=password'
-d 'client_id=consumer-key' -d 'client_secret=consumer-secret' -d
'username=my-login@domain.com' -d 'password=my-password' https://MyDomainName.my.salesforce.com/services/oauth2/token

# Response:
{"access_token":"00D5e000001N20Q!ASAAQEDBeG8bOwPu8NWGsvFwWNfqHOp5ZcjMpFsU6yEMxTKdBuRXNzSZ8xGVyAiY8xoy1KYkaadzRlA2F5Zd3JXqLVitOdNS",
"instance_url":"https://MyDomainName.my.salesforce.com",
"id":"https://login.salesforce.com/id/00D5e000001N20QEAS/0055e000003E8ooAAC",
"token_type":"Bearer",
"issued_at":"1627237872637",
"signature":"jmaZOgQyqUxFKAesVPsqVfAWxI62O+aH/mJhDrc8KvQ="}
~~~
    
    
