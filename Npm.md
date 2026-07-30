# Complete npm Tutorial

## From Beginner to Advanced

This tutorial is designed to take you from having no npm experience to understanding how professional software projects use it. Throughout the tutorial we'll use a small JavaScript project as an example.

---

# Chapter 1 - What is npm?

npm stands for **Node Package Manager**.

Originally it was only a package manager for Node.js, but today it has become the standard package manager for the entire JavaScript ecosystem.

npm provides three major capabilities:

* Installing libraries
* Managing project dependencies
* Running project automation scripts

Almost every modern JavaScript project—including:

* React
* Vue
* Angular
* Express
* Next.js
* Electron
* Vite

uses npm.

---

# Chapter 2 - Installing Node.js

npm comes with Node.js.

Download and install Node.js.

After installation, verify everything works.

```bash
node --version

npm --version
```

Example

```
v24.3.0

11.2.0
```

If both commands work, npm is ready.

---

# Chapter 3 - Creating Your First Project

Create a new folder.

```bash
mkdir hello-npm

cd hello-npm
```

Initialize the project.

```bash
npm init
```

npm asks several questions.

```
package name:
version:
description:
entry point:
author:
license:
```

For tutorials it's easier to accept defaults.

Or skip all prompts.

```bash
npm init -y
```

This creates

```
package.json
```

---

# Chapter 4 - Understanding package.json

Open

```text
package.json
```

Example

```json
{
  "name": "hello-npm",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"No tests\""
  },
  "author": "",
  "license": "ISC"
}
```

This file is the heart of every npm project.

It stores

* project name
* version
* dependencies
* scripts
* metadata

---

# Chapter 5 - Your First Program

Create

```
index.js
```

```javascript
console.log("Hello npm!");
```

Run it

```bash
node index.js
```

Output

```
Hello npm!
```

---

# Chapter 6 - Installing Packages

Let's install a package.

Example:

```bash
npm install chalk
```

npm downloads the package and updates

```
package.json
```

and creates

```
node_modules/
```

---

# Chapter 7 - Using Installed Packages

```javascript
import chalk from "chalk";

console.log(chalk.green("Success!"));
```

Run

```bash
node index.js
```

---

# Chapter 8 - What is node_modules?

After installing a package you'll see

```
node_modules/
```

This contains

* every installed package
* all dependencies
* package metadata

Large projects may contain tens of thousands of files.

Never edit files here manually.

---

# Chapter 9 - package-lock.json

npm also creates

```
package-lock.json
```

This file locks every dependency version.

Without it

```
Developer A

might install

Version 4.2

Developer B

might install

Version 4.5
```

With package-lock.json

Everyone installs the exact same versions.

---

# Chapter 10 - Installing Specific Versions

```bash
npm install express@5
```

or

```bash
npm install express@4.18.2
```

---

# Chapter 11 - Removing Packages

```bash
npm uninstall chalk
```

npm automatically updates

```
package.json
```

---

# Chapter 12 - Updating Packages

Check outdated packages.

```bash
npm outdated
```

Update them.

```bash
npm update
```

---

# Chapter 13 - Installing Development Packages

Development tools shouldn't be required by users.

Install with

```bash
npm install --save-dev eslint
```

or

```bash
npm install -D eslint
```

package.json becomes

```json
"devDependencies": {
    "eslint":"..."
}
```

---

# Chapter 14 - Dependencies vs DevDependencies

dependencies

```
Needed by the application
```

Examples

* Express
* React
* Axios

Development dependencies

```
Needed only while developing
```

Examples

* ESLint
* Jest
* TypeScript
* Prettier

---

# Chapter 15 - Installing Globally

Sometimes software should be available everywhere.

```bash
npm install -g typescript
```

Now

```bash
tsc
```

works from any folder.

---

# Chapter 16 - npm Scripts

One of npm's most useful features.

package.json

```json
{
    "scripts": {
        "start":"node index.js"
    }
}
```

Run

```bash
npm start
```

---

# Chapter 17 - Multiple Scripts

```json
"scripts": {

"start":"node app.js",

"dev":"node app.js",

"test":"node test.js",

"build":"node build.js"

}
```

Run

```bash
npm run build

npm run dev

npm run test
```

---

# Chapter 18 - Passing Arguments

```bash
npm run build -- --verbose
```

Everything after `--` goes to your script.

---

# Chapter 19 - Installing Multiple Packages

```bash
npm install express axios dotenv
```

---

# Chapter 20 - Installing Exact Versions

```bash
npm install express@4.18.2
```

---

# Chapter 21 - Semantic Versioning

Suppose package.json contains

```json
"express":"^5.1.0"
```

The symbols matter.

```
^

Allow compatible updates

5.1.1

5.2.0

5.9.0

Not

6.0
```

---

```
~

Allow patch updates

5.1.1

5.1.5

Not

5.2
```

---

No symbol

```
5.1.0

Always exactly

5.1.0
```

---

# Chapter 22 - npm Search

Search packages.

```bash
npm search markdown
```

---

# Chapter 23 - Package Information

```bash
npm view express
```

or

```bash
npm info express
```

---

# Chapter 24 - Listing Installed Packages

```bash
npm list
```

Top level only

```bash
npm list --depth=0
```

---

# Chapter 25 - Running Package Executables

Many packages include commands.

Example

```bash
npx eslint
```

Unlike installing globally, `npx` runs the project's local version.

---

# Chapter 26 - Creating Your Own Package

Create

```
math.js
```

```javascript
export function add(a,b){

return a+b;

}
```

---

Create

```
index.js
```

```javascript
import {add} from "./math.js";

console.log(add(2,3));
```

---

# Chapter 27 - Publishing a Package

Login

```bash
npm login
```

Publish

```bash
npm publish
```

Anyone can install

```bash
npm install your-package
```

---

# Chapter 28 - Versioning

Increase patch version

```bash
npm version patch
```

Minor

```bash
npm version minor
```

Major

```bash
npm version major
```

---

# Chapter 29 - Useful Commands

```
npm init

Create project

-------------------------

npm install

Install packages

-------------------------

npm uninstall

Remove package

-------------------------

npm update

Update packages

-------------------------

npm list

Installed packages

-------------------------

npm outdated

Outdated packages

-------------------------

npm run

Run scripts

-------------------------

npm start

Run start script

-------------------------

npm test

Run tests

-------------------------

npm publish

Publish package

-------------------------

npm login

Log in

-------------------------

npm cache clean --force

Clear cache
```

---

# Chapter 30 - Best Practices

Keep `package-lock.json` in version control.

Never edit `node_modules`.

Use `npm ci` in continuous integration (CI) environments.

Use development dependencies appropriately.

Keep packages up to date.

Avoid installing packages globally unless they are truly system-wide tools.

Read package documentation before using unfamiliar libraries.

Regularly run `npm audit` to identify known security vulnerabilities in dependencies, and use `npm audit fix` where appropriate.

---

# Chapter 31 - Professional Project Structure

A typical npm project looks like this:

```text
my-project/
├── package.json
├── package-lock.json
├── node_modules/
├── src/
│   ├── index.js
│   ├── utils.js
│   └── config.js
├── test/
│   └── index.test.js
├── public/
│   ├── index.html
│   └── styles.css
├── .gitignore
├── README.md
└── LICENSE
```

---

# Chapter 32 - Where to Go Next

Once you're comfortable with npm, the next topics to explore are:

1. ECMAScript modules (ESM) vs. CommonJS modules
2. Creating reusable npm packages
3. Monorepos using npm workspaces
4. Build tools such as Vite, Rollup, or Webpack
5. TypeScript integration
6. Automated testing with Jest or Vitest
7. Code quality tools like ESLint and Prettier
8. Continuous Integration (CI) with GitHub Actions
9. Dockerizing npm applications
10. Publishing and maintaining open-source packages

By mastering these topics alongside npm, you'll have a solid foundation for developing and maintaining modern JavaScript applications of any size.
