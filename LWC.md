
# ⚡ Lightning Web Components (LWC) – Comprehensive Tutorial

## 📌 What is LWC?

Lightning Web Components is Salesforce’s UI framework based on **native web standards**:

* Custom Elements
* Shadow DOM
* ES6+ JavaScript modules

It replaces Aura as the preferred way to build UI on the Salesforce platform.

Used to build:

* Lightning Experience apps
* Salesforce Mobile
* Experience Cloud sites

---

# 🧰 Prerequisites & Setup

## ✅ What you need

* Salesforce Developer Account
* Node.js (LTS)
* VS Code
* Salesforce CLI
* Salesforce Extension Pack for VS Code

👉 Install Salesforce CLI:
[https://developer.salesforce.com/tools/sfdxcli](https://developer.salesforce.com/tools/sfdxcli)

👉 Install VS Code extensions:

* Salesforce Extension Pack

---

## ✅ Create a Salesforce DX Project

```bash
sf org login web -d -a DevHub
sf project generate --name lwc-tutorial
cd lwc-tutorial
sf org create scratch -f config/project-scratch-def.json -a lwcOrg -s
```

Open folder in VS Code.

---

# 📁 LWC Project Structure

```
force-app/
 └── main/default/lwc/
      └── helloWorld/
           ├── helloWorld.html
           ├── helloWorld.js
           ├── helloWorld.js-meta.xml
```

Every component has **3 core files**.

---

# 🧩 Your First Component

## helloWorld.html

```html
<template>
    <lightning-card title="Hello LWC">
        <div class="slds-p-around_medium">
            <p>Hello, Lightning Web Components!</p>
        </div>
    </lightning-card>
</template>
```

---

## helloWorld.js

```js
import { LightningElement } from 'lwc';

export default class HelloWorld extends LightningElement {

}
```

---

## helloWorld.js-meta.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>61.0</apiVersion>
    <isExposed>true</isExposed>
    <targets>
        <target>lightning__AppPage</target>
        <target>lightning__RecordPage</target>
        <target>lightning__HomePage</target>
    </targets>
</LightningComponentBundle>
```

---

## Deploy

```bash
sf project deploy start
sf org open
```

Add the component to a page in **Lightning App Builder**.

---

# 🎯 Data Binding

## Template

```html
<template>
    <lightning-card title="Binding Example">
        <div class="slds-p-around_medium">
            <p>Hello {name}</p>
        </div>
    </lightning-card>
</template>
```

## JS

```js
import { LightningElement } from 'lwc';

export default class BindingExample extends LightningElement {
    name = 'Chris';
}
```

---

# ✍️ Handling Events

```html
<template>
    <lightning-input label="Enter name" onchange={handleChange}></lightning-input>
    <p>Hello {name}</p>
</template>
```

```js
import { LightningElement } from 'lwc';

export default class EventExample extends LightningElement {
    name = '';

    handleChange(event) {
        this.name = event.target.value;
    }
}
```

---

# 🎨 Conditional Rendering

```html
<template>
    <lightning-button label="Toggle" onclick={toggle}></lightning-button>

    <template if:true={visible}>
        <p>Now you see me</p>
    </template>
</template>
```

```js
visible = false;

toggle() {
    this.visible = !this.visible;
}
```

---

# 🔁 Looping

```html
<template>
    <template for:each={contacts} for:item="c">
        <p key={c.Id}>{c.Name}</p>
    </template>
</template>
```

```js
contacts = [
    { Id: 1, Name: 'Alice' },
    { Id: 2, Name: 'Bob' }
];
```

---

# 🔗 Parent → Child Communication

## childComponent.js

```js
import { LightningElement, api } from 'lwc';

export default class ChildComponent extends LightningElement {
    @api message;
}
```

## childComponent.html

```html
<template>
    <p>Message from parent: {message}</p>
</template>
```

## parentComponent.html

```html
<c-child-component message="Hello from parent"></c-child-component>
```

---

# 📣 Child → Parent (Custom Events)

## child.js

```js
send() {
    this.dispatchEvent(new CustomEvent('notify', {
        detail: 'Hello Parent'
    }));
}
```

```html
<lightning-button label="Send" onclick={send}></lightning-button>
```

---

## parent.html

```html
<c-child onnotify={handleNotify}></c-child>
<p>{msg}</p>
```

```js
msg;

handleNotify(event) {
    this.msg = event.detail;
}
```

---

# 🌐 Calling Apex from LWC

---

## Apex Controller

```apex
public with sharing class ContactController {
    @AuraEnabled(cacheable=true)
    public static List<Contact> getContacts() {
        return [SELECT Id, Name, Email FROM Contact LIMIT 10];
    }
}
```

---

## Wire Service (Reactive)

```js
import { LightningElement, wire } from 'lwc';
import getContacts from '@salesforce/apex/ContactController.getContacts';

export default class WireExample extends LightningElement {
    @wire(getContacts) contacts;
}
```

```html
<template>
    <template if:true={contacts.data}>
        <template for:each={contacts.data} for:item="c">
            <p key={c.Id}>{c.Name} - {c.Email}</p>
        </template>
    </template>
</template>
```

---

# ⚡ Imperative Apex Call

```js
import getContacts from '@salesforce/apex/ContactController.getContacts';

async loadContacts() {
    this.contacts = await getContacts();
}
```

---

# 🧠 Lifecycle Hooks

```js
connectedCallback() {
    console.log('Component inserted');
}

renderedCallback() {
    console.log('Rendered');
}

disconnectedCallback() {
    console.log('Removed');
}

errorCallback(error, stack) {
    console.error(error);
}
```

---

# 🎯 Lightning Base Components

Common built-ins:

* `<lightning-input>`
* `<lightning-button>`
* `<lightning-card>`
* `<lightning-datatable>`
* `<lightning-record-form>`
* `<lightning-combobox>`
* `<lightning-layout>`

Example:

```html
<lightning-record-form
    object-api-name="Contact"
    layout-type="Full"
    mode="edit">
</lightning-record-form>
```

---

# 🗃️ Lightning Datatable Example

```html
<lightning-datatable
    data={contacts}
    columns={columns}
    key-field="Id">
</lightning-datatable>
```

```js
columns = [
    { label: 'Name', fieldName: 'Name' },
    { label: 'Email', fieldName: 'Email' }
];
```

---

# 🧬 Reactive Properties

```js
import { track } from 'lwc';

@track contact = { Name: '', Email: '' };
```

(Note: shallow reactivity is default now; `@track` rarely needed.)

---

# 🛡️ Security Model

* Locker Service
* Shadow DOM isolation
* No direct DOM manipulation
* CSP enforced

Safe DOM access:

```js
this.template.querySelector('input')
```

---

# 🎨 Styling LWC

## component.css

```css
p {
    color: blue;
    font-weight: bold;
}
```

Scoped automatically.

Global styles → `staticresources` or SLDS.

---

# 📦 Using Static Resources

```js
import LOGO from '@salesforce/resourceUrl/logo';
```

```html
<img src={logo}>
```

---

# 🚀 Navigation Service

```js
import { NavigationMixin } from 'lightning/navigation';

export default class NavExample extends NavigationMixin(LightningElement) {

    navigate() {
        this[NavigationMixin.Navigate]({
            type: 'standard__recordPage',
            attributes: {
                recordId: '001XXXXXXXX',
                actionName: 'view'
            }
        });
    }
}
```

---

# 🧪 Unit Testing (Jest)

```bash
sf force lightning lwc test setup
npm run test:unit
```

Test example:

```js
import { createElement } from 'lwc';
import HelloWorld from 'c/helloWorld';

describe('hello-world', () => {
    it('renders greeting', () => {
        const element = createElement('c-hello-world', { is: HelloWorld });
        document.body.appendChild(element);
        expect(element.shadowRoot.textContent).toContain('Hello');
    });
});
```

---

# 📦 Deployment

```bash
sf project deploy start
```

Or:

```bash
sf project deploy start --source-dir force-app
```

---

# 🏗️ Real-World Component Example: Contact Search

Apex:

```apex
@AuraEnabled(cacheable=true)
public static List<Contact> search(String term) {
    return [
        SELECT Id, Name, Email
        FROM Contact
        WHERE Name LIKE :('%' + term + '%')
        LIMIT 10
    ];
}
```

JS:

```js
searchTerm = '';
contacts;

handleChange(e) {
    this.searchTerm = e.target.value;
}

async handleSearch() {
    this.contacts = await search({ term: this.searchTerm });
}
```

HTML:

```html
<lightning-input label="Search"></lightning-input>
<lightning-button label="Go" onclick={handleSearch}></lightning-button>

<template for:each={contacts} for:item="c">
    <p key={c.Id}>{c.Name}</p>
</template>
```

---

# 📐 Best Practices

✅ Use wire when possible
✅ Cache Apex calls
✅ Keep components small
✅ Use Lightning base components
✅ Avoid inline SOQL in JS
✅ Use events, not DOM reach-through
✅ Build reusable UI primitives

---

# 🧭 Learning Path Forward

Next steps you may want:

* Build full CRUD UI
* Lightning Message Service
* Dynamic forms
* Custom datatable types
* LWC + Flow integration
* Pub/sub patterns
* Offline-first mobile components


Just tell me 👍
