<!-- markdownlint-disable MD029 -->

# Javascript Cheatsheet

## **1. Introduction to JavaScript**

JavaScript is a versatile, high-level programming language primarily used for web development. It runs in the browser, enabling dynamic content, but can also run on servers via Node.js.

**Key Features:**

* Client-side and server-side development
* Dynamic typing
* Event-driven programming
* Object-oriented and functional programming
* Asynchronous programming with Promises and async/await

**Setup:**

1. Browser Console: Open Chrome/Firefox → F12 → Console.
2. HTML Integration:

```html
<script>
  console.log("Hello, JavaScript!");
</script>
```

3. External JS File:

```html
<script src="app.js"></script>
```

---

## **2. Basics of JavaScript**

### **2.1 Variables**

```javascript
// Modern variable declarations
let name = "Chris"; // Can change
const age = 30;     // Constant value

// Old-style (avoid in modern code)
var city = "New York";
```

### **2.2 Data Types**

```javascript
let string = "Hello";        // String
let number = 42;             // Number
let boolean = true;          // Boolean
let object = { name: "Chris" }; // Object
let array = [1, 2, 3];       // Array
let nothing = null;          // Null
let notDefined;              // Undefined
```

### **2.3 Operators**

```javascript
// Arithmetic
let sum = 5 + 3; 
let remainder = 10 % 3;

// Comparison
console.log(5 === "5"); // false, strict equality
console.log(5 == "5");  // true, loose equality

// Logical
console.log(true && false); // false
console.log(true || false); // true
```

---

## **3. Control Structures**

### **3.1 Conditional Statements**

```javascript
let score = 85;

if (score >= 90) {
    console.log("A");
} else if (score >= 75) {
    console.log("B");
} else {
    console.log("C");
}
```

### **3.2 Loops**

```javascript
// For loop
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// While loop
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}

// For-of loop (arrays)
let colors = ["red", "green", "blue"];
for (let color of colors) {
    console.log(color);
}

// For-in loop (objects)
let person = { name: "Chris", age: 30 };
for (let key in person) {
    console.log(key, person[key]);
}
```

---

## **4. Functions**

### **4.1 Function Declaration**

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}
console.log(greet("Chris"));
```

### **4.2 Function Expression**

```javascript
const add = function(a, b) {
    return a + b;
};
```

### **4.3 Arrow Functions**

```javascript
const multiply = (a, b) => a * b;
console.log(multiply(2, 3));
```

---

## **5. Objects and Arrays**

### **5.1 Objects**

```javascript
let person = {
    name: "Chris",
    age: 30,
    greet() {
        console.log(`Hi, I'm ${this.name}`);
    }
};

person.greet();
console.log(person.age);
```

### **5.2 Arrays**

```javascript
let fruits = ["apple", "banana", "cherry"];

// Array methods
fruits.push("date");       // Add to end
fruits.pop();              // Remove last
fruits.shift();            // Remove first
fruits.unshift("avocado"); // Add to beginning

fruits.forEach(fruit => console.log(fruit)); // Iterate
```

---

## **6. ES6+ Features**

### **6.1 Template Literals**

```javascript
let name = "Chris";
let message = `Hello, ${name}!`;
```

### **6.2 Destructuring**

```javascript
let person = { name: "Chris", age: 30 };
let { name, age } = person;

let colors = ["red", "green"];
let [first, second] = colors;
```

### **6.3 Spread & Rest Operators**

```javascript
// Spread
let arr1 = [1,2];
let arr2 = [...arr1, 3,4];

// Rest
function sum(...numbers) {
    return numbers.reduce((a,b) => a + b, 0);
}
```

### **6.4 Modules**

```javascript
// export.js
export const pi = 3.14;

// import.js
import { pi } from './export.js';
```

---

## **7. DOM Manipulation**

```javascript
// Select elements
let title = document.getElementById("title");
let buttons = document.querySelectorAll(".btn");

// Change content
title.textContent = "Hello World!";

// Event listener
buttons.forEach(btn => btn.addEventListener("click", () => alert("Clicked!")));
```

---

## **8. Events**

```javascript
document.addEventListener("DOMContentLoaded", () => console.log("Page loaded"));

window.addEventListener("resize", () => console.log("Window resized"));

document.querySelector("button").onclick = function() {
    console.log("Button clicked");
};
```

---

## **9. Asynchronous JavaScript**

### **9.1 Callbacks**

```javascript
function fetchData(callback) {
    setTimeout(() => callback("Data loaded"), 1000);
}

fetchData(data => console.log(data));
```

### **9.2 Promises**

```javascript
let promise = new Promise((resolve, reject) => {
    let success = true;
    setTimeout(() => success ? resolve("Done") : reject("Failed"), 1000);
});

promise.then(result => console.log(result)).catch(err => console.error(err));
```

### **9.3 Async/Await**

```javascript
async function getData() {
    try {
        let result = await promise;
        console.log(result);
    } catch(err) {
        console.error(err);
    }
}
getData();
```

---

## **10. Advanced Topics**

### **10.1 Closures**

```javascript
function outer() {
    let count = 0;
    return function inner() {
        count++;
        return count;
    };
}
const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

### **10.2 Classes**

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    greet() {
        console.log(`Hi, I'm ${this.name}`);
    }
}

let chris = new Person("Chris", 30);
chris.greet();
```

### **10.3 Error Handling**

```javascript
try {
    throw new Error("Oops!");
} catch(e) {
    console.error(e.message);
} finally {
    console.log("Always runs");
}
```

### **10.4 Local Storage**

```javascript
localStorage.setItem("name", "Chris");
console.log(localStorage.getItem("name"));
localStorage.removeItem("name");
```

---

## **11. JavaScript Best Practices**

1. Use `let` and `const` instead of `var`.
2. Prefer strict equality `===`.
3. Keep functions small and focused.
4. Avoid global variables.
5. Use modular code and ES6 imports/exports.
6. Handle asynchronous operations with `async/await` for clarity.
7. Always validate input data.

---

## **12. Next Steps**

* Learn a modern framework: **React**, **Vue**, or **Angular**.
* Practice APIs and fetch data from **REST APIs**.
* Dive deeper into **Node.js** for server-side JavaScript.
* Explore **TypeScript** for type safety.
