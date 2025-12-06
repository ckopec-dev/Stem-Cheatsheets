<!-- markdownlint-disable MD014 -->

# Comprehensive Node.js Tutorial

## Table of Contents
1. Introduction to Node.js
2. Installation and Setup
3. Core Concepts
4. Working with Modules
5. Asynchronous Programming
6. File System Operations
7. Building a Web Server
8. Working with NPM
9. Express.js Framework
10. Database Integration
11. Error Handling
12. Best Practices

---

## 1. Introduction to Node.js

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. It allows you to run JavaScript on the server side, enabling full-stack JavaScript development.

**Key Features:**
- Asynchronous and event-driven
- Single-threaded with non-blocking I/O
- Fast execution with V8 engine
- Large ecosystem (NPM)
- Cross-platform

**Common Use Cases:**
- REST APIs
- Real-time applications (chat, notifications)
- Microservices
- Streaming applications
- Command-line tools

---

## 2. Installation and Setup

**Installing Node.js:**

Visit [nodejs.org](https://nodejs.org) and download the LTS version for your operating system.

**Verify Installation:**
```bash
node --version
npm --version
```

**Creating Your First Project:**
```bash
mkdir my-node-app
cd my-node-app
npm init -y
```

This creates a `package.json` file that manages your project dependencies and metadata.

---

## 3. Core Concepts

### The Event Loop

Node.js uses an event-driven, non-blocking I/O model. The event loop allows Node.js to perform non-blocking operations despite JavaScript being single-threaded.

**Phases of the Event Loop:**
1. Timers: Executes callbacks scheduled by `setTimeout()` and `setInterval()`
2. Pending callbacks: Executes I/O callbacks deferred to the next loop iteration
3. Idle/prepare: Internal use only
4. Poll: Retrieves new I/O events
5. Check: Executes `setImmediate()` callbacks
6. Close callbacks: Executes close event callbacks

### Global Objects

Node.js provides several global objects:

```javascript
console.log(__dirname);  // Current directory path
console.log(__filename); // Current file path
console.log(process.env); // Environment variables
console.log(process.argv); // Command line arguments
```

---

## 4. Working with Modules

### CommonJS Modules

Node.js uses the CommonJS module system by default.

**Exporting:**
```javascript
// math.js
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = { add, subtract };
// or
exports.add = add;
exports.subtract = subtract;
```

**Importing:**
```javascript
// app.js
const math = require('./math');
console.log(math.add(5, 3)); // 8
```

### ES Modules

Modern Node.js also supports ES modules. Add `"type": "module"` to your `package.json`.

**Exporting:**
```javascript
// math.mjs
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

**Importing:**
```javascript
// app.mjs
import { add, subtract } from './math.mjs';
console.log(add(5, 3)); // 8
```

### Core Modules

Node.js includes built-in modules you can use without installation:

```javascript
const fs = require('fs');      // File system
const path = require('path');  // File paths
const http = require('http');  // HTTP server
const os = require('os');      // Operating system
const events = require('events'); // Events
```

---

## 5. Asynchronous Programming

### Callbacks

Traditional approach to handling async operations:

```javascript
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error:', err);
    return;
  }
  console.log(data);
});
```

**Callback Hell Example:**
```javascript
fs.readFile('file1.txt', (err, data1) => {
  if (err) return console.error(err);
  fs.readFile('file2.txt', (err, data2) => {
    if (err) return console.error(err);
    fs.readFile('file3.txt', (err, data3) => {
      if (err) return console.error(err);
      console.log(data1, data2, data3);
    });
  });
});
```

### Promises

A cleaner way to handle async operations:

```javascript
const fs = require('fs').promises;

fs.readFile('file.txt', 'utf8')
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

**Chaining Promises:**
```javascript
fs.readFile('file1.txt', 'utf8')
  .then(data1 => {
    console.log(data1);
    return fs.readFile('file2.txt', 'utf8');
  })
  .then(data2 => {
    console.log(data2);
    return fs.readFile('file3.txt', 'utf8');
  })
  .then(data3 => console.log(data3))
  .catch(err => console.error(err));
```

### Async/Await

Modern syntax for working with promises:

```javascript
const fs = require('fs').promises;

async function readFiles() {
  try {
    const data1 = await fs.readFile('file1.txt', 'utf8');
    const data2 = await fs.readFile('file2.txt', 'utf8');
    const data3 = await fs.readFile('file3.txt', 'utf8');
    console.log(data1, data2, data3);
  } catch (err) {
    console.error('Error:', err);
  }
}

readFiles();
```

**Parallel Execution:**
```javascript
async function readFilesParallel() {
  try {
    const [data1, data2, data3] = await Promise.all([
      fs.readFile('file1.txt', 'utf8'),
      fs.readFile('file2.txt', 'utf8'),
      fs.readFile('file3.txt', 'utf8')
    ]);
    console.log(data1, data2, data3);
  } catch (err) {
    console.error('Error:', err);
  }
}
```

---

## 6. File System Operations

### Reading Files

```javascript
const fs = require('fs').promises;

// Read entire file
async function readFile() {
  const data = await fs.readFile('example.txt', 'utf8');
  console.log(data);
}

// Read file synchronously (blocking)
const data = fs.readFileSync('example.txt', 'utf8');
```

### Writing Files

```javascript
// Write to file
await fs.writeFile('output.txt', 'Hello, World!');

// Append to file
await fs.appendFile('output.txt', '\nNew line');
```

### Working with Directories

```javascript
// Create directory
await fs.mkdir('new-folder');

// Read directory contents
const files = await fs.readdir('.');
console.log(files);

// Delete directory
await fs.rmdir('new-folder');
```

### Checking File Stats

```javascript
const stats = await fs.stat('example.txt');
console.log(stats.isFile());      // true
console.log(stats.isDirectory()); // false
console.log(stats.size);          // file size in bytes
```

### Streams

For large files, use streams to avoid loading everything into memory:

```javascript
const fs = require('fs');

// Read stream
const readStream = fs.createReadStream('large-file.txt', 'utf8');
readStream.on('data', (chunk) => {
  console.log('Chunk:', chunk);
});

// Write stream
const writeStream = fs.createWriteStream('output.txt');
writeStream.write('Line 1\n');
writeStream.write('Line 2\n');
writeStream.end();

// Pipe streams
const read = fs.createReadStream('input.txt');
const write = fs.createWriteStream('output.txt');
read.pipe(write);
```

---

## 7. Building a Web Server

### Basic HTTP Server

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}/`);
});
```

### Routing

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  const { url, method } = req;
  
  res.setHeader('Content-Type', 'application/json');
  
  if (url === '/' && method === 'GET') {
    res.statusCode = 200;
    res.end(JSON.stringify({ message: 'Welcome to the API' }));
  } else if (url === '/users' && method === 'GET') {
    res.statusCode = 200;
    res.end(JSON.stringify({ users: ['Alice', 'Bob'] }));
  } else {
    res.statusCode = 404;
    res.end(JSON.stringify({ error: 'Not Found' }));
  }
});

server.listen(3000);
```

### Handling POST Requests

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.url === '/data' && req.method === 'POST') {
    let body = '';
    
    req.on('data', chunk => {
      body += chunk.toString();
    });
    
    req.on('end', () => {
      const data = JSON.parse(body);
      res.setHeader('Content-Type', 'application/json');
      res.end(JSON.stringify({ received: data }));
    });
  }
});

server.listen(3000);
```

---

## 8. Working with NPM

### Installing Packages

```bash
# Install a package
npm install express

# Install as dev dependency
npm install --save-dev nodemon

# Install globally
npm install -g typescript

# Install specific version
npm install express@4.18.0
```

### Package.json Scripts

```json
{
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "jest"
  }
}
```

Run with: `npm start`, `npm run dev`, etc.

### Useful Packages

- **express**: Web framework
- **dotenv**: Environment variables
- **nodemon**: Auto-restart during development
- **axios**: HTTP client
- **joi**: Data validation
- **jsonwebtoken**: JWT authentication
- **bcrypt**: Password hashing
- **morgan**: HTTP request logger

---

## 9. Express.js Framework

### Basic Express App

```javascript
const express = require('express');
const app = express();

// Middleware to parse JSON
app.use(express.json());

// Routes
app.get('/', (req, res) => {
  res.send('Hello from Express!');
});

app.get('/api/users', (req, res) => {
  res.json([
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
  ]);
});

app.post('/api/users', (req, res) => {
  const user = req.body;
  res.status(201).json(user);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Route Parameters

```javascript
app.get('/api/users/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ id: userId, name: 'Alice' });
});

// Query parameters
app.get('/api/search', (req, res) => {
  const { query, page } = req.query;
  res.json({ query, page });
});
```

### Middleware

```javascript
// Custom middleware
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};

app.use(logger);

// Built-in middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static('public'));
```

### Error Handling

```javascript
// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});

// 404 handler
app.use((req, res) => {
  res.status(404).json({ error: 'Route not found' });
});
```

### Router

```javascript
// routes/users.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.json({ users: [] });
});

router.post('/', (req, res) => {
  res.status(201).json(req.body);
});

module.exports = router;

// app.js
const userRoutes = require('./routes/users');
app.use('/api/users', userRoutes);
```

---

## 10. Database Integration

### MongoDB with Mongoose

```bash
npm install mongoose
```

```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/myapp', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

// Define Schema
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: Number,
  createdAt: { type: Date, default: Date.now }
});

// Create Model
const User = mongoose.model('User', userSchema);

// Create
async function createUser() {
  const user = new User({
    name: 'Alice',
    email: 'alice@example.com',
    age: 25
  });
  await user.save();
}

// Read
async function getUsers() {
  const users = await User.find();
  const alice = await User.findOne({ name: 'Alice' });
  const userById = await User.findById('123');
}

// Update
async function updateUser() {
  await User.updateOne({ name: 'Alice' }, { age: 26 });
  const user = await User.findByIdAndUpdate('123', { age: 26 }, { new: true });
}

// Delete
async function deleteUser() {
  await User.deleteOne({ name: 'Alice' });
  await User.findByIdAndDelete('123');
}
```

### PostgreSQL with pg

```bash
npm install pg
```

```javascript
const { Pool } = require('pg');

const pool = new Pool({
  user: 'user',
  host: 'localhost',
  database: 'myapp',
  password: 'password',
  port: 5432,
});

// Query
async function queryUsers() {
  const result = await pool.query('SELECT * FROM users');
  console.log(result.rows);
}

// Parameterized query
async function createUser(name, email) {
  const query = 'INSERT INTO users(name, email) VALUES($1, $2) RETURNING *';
  const values = [name, email];
  const result = await pool.query(query, values);
  return result.rows[0];
}
```

---

## 11. Error Handling

### Try-Catch with Async/Await

```javascript
async function fetchData() {
  try {
    const data = await someAsyncOperation();
    return data;
  } catch (error) {
    console.error('Error:', error.message);
    throw error; // Re-throw if needed
  }
}
```

### Custom Error Classes

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
    this.statusCode = 400;
  }
}

class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = 'NotFoundError';
    this.statusCode = 404;
  }
}

// Usage
if (!user) {
  throw new NotFoundError('User not found');
}
```

### Express Error Handler

```javascript
// Async handler wrapper
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Use with routes
app.get('/api/users/:id', asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    throw new NotFoundError('User not found');
  }
  res.json(user);
}));

// Global error handler
app.use((err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  res.status(statusCode).json({
    error: {
      message: err.message,
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
});
```

---

## 12. Best Practices

### Environment Variables

```javascript
// .env file
PORT=3000
DATABASE_URL=mongodb://localhost:27017/myapp
JWT_SECRET=your-secret-key

// Load with dotenv
require('dotenv').config();
const port = process.env.PORT || 3000;
```

### Security

```bash
npm install helmet cors
```

```javascript
const helmet = require('helmet');
const cors = require('cors');

app.use(helmet()); // Security headers
app.use(cors());   // CORS configuration
```

### Logging

```bash
npm install winston
```

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

logger.info('Server started');
logger.error('An error occurred');
```

### Project Structure

```
my-app/
├── src/
│   ├── config/
│   │   └── database.js
│   ├── controllers/
│   │   └── userController.js
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   └── userRoutes.js
│   ├── middleware/
│   │   └── auth.js
│   └── app.js
├── tests/
├── .env
├── .gitignore
├── package.json
└── README.md
```

### Testing

```bash
npm install --save-dev jest supertest
```

```javascript
// user.test.js
const request = require('supertest');
const app = require('../src/app');

describe('User API', () => {
  test('GET /api/users returns users', async () => {
    const response = await request(app).get('/api/users');
    expect(response.statusCode).toBe(200);
    expect(Array.isArray(response.body)).toBe(true);
  });
  
  test('POST /api/users creates user', async () => {
    const newUser = { name: 'Alice', email: 'alice@test.com' };
    const response = await request(app)
      .post('/api/users')
      .send(newUser);
    expect(response.statusCode).toBe(201);
    expect(response.body.name).toBe('Alice');
  });
});
```

### Performance Tips

1. **Use async/await** instead of callbacks
2. **Implement caching** for frequently accessed data
3. **Use connection pooling** for databases
4. **Enable compression** for responses
5. **Use clustering** for multi-core systems
6. **Implement rate limiting** to prevent abuse
7. **Use streams** for large data processing
8. **Monitor with tools** like PM2 or New Relic

---

## Conclusion

This tutorial covers the fundamentals of Node.js development. To continue learning, practice building projects such as REST APIs, real-time chat applications, or authentication systems. Explore the Node.js documentation and the vast ecosystem of NPM packages to expand your skills further.
