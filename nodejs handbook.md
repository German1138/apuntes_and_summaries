# Node.js

Node.js is a free, open-source JavaScript runtime.

It lets you execute JavaScript code outside of a web browser, enabling server-side development with JavaScript. Is designed for building scalable network applications efficiently

Node.js uses an **event-driven, non-blocking** model. It excels at handling many simultaneous connections with minimal overhead. It's perfect for

- Real-time applications (chats, gaming, collaboration tools)
- APIs and microservices
- Data streaming applications
- Command-line tools
- Server-side web applications

The difference with JavaScript in the browser is that Node.js comes with many built-in modules to help you work with files, servers, paths, the operating system, and more.

### Asynchronous Programming

Node.js uses **asynchronous (non-blocking)** programming. This means it can keep working while waiting for tasks like reading files or talking to a database. With asynchronous code, Node.js can handle many things at once-making it fast and efficient.

In the following example Node.js continues to the next line while reading the file, executing the console.log, when the file is finally read our callback function runs.

```js
// Load the filesystem module
const fs = require("fs");

// Read file asynchronously
fs.readFile("myfile.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Error reading file: " + err);
    return;
  }
  console.log("File content: " + data);
});

console.log("Reading file... (this runs first!)");
```

Node.js can do

- Web Servers: Create fast, scalable network applications
- File Operations: Read, write, and manage files on the server
- Database Interaction: Work with databases like MongoDB, MySQL, and more
- APIs: Build RESTful services and GraphQL APIs
- Real-time: Handle WebSockets for live applications
- CLI Tools: Create command-line applications

Simple web server example

```js
const http = require("http");
http
  .createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World!");
  })
  .listen(8080);
```

> All the node files usally have the `.js` extension and can be started/executed with the `node` command in a terminal. They use the JavaScript syntax. Running a node.js file would be: `node app.js`

## Node.js vs browsers

Node.js is designed for server-side development, while browsers are for client-side applications.

- APIs: Node.js provides APIs for file system, networking, and OS, which browsers do not. Browsers provide DOM, fetch, and UI APIs not available in Node.js.
- Global Objects: Node.js uses global; browsers use window or self.
- Modules: Node.js uses CommonJS (require) and ES modules (import); browsers use ES modules or plain `<script>` tags.
- Security: Browsers run in a sandbox with limited access; Node.js has full access to the file system and network.
- Event Loop: Both environments use an event loop, but Node.js has additional APIs for timers, process, etc.
- Environment Variables: Node.js can access environment variables (process.env); browsers cannot.
- Package Management: Node.js uses npm/yarn; browsers use CDNs or bundlers.

## Node.js Commands

```shell
# Run a JavaScript file
node app.js

# Run in watch mode (restarts on file changes)
node --watch app.js

# Check syntax without executing
node --check app.js

# Show stack traces for warnings
node --trace-warnings app.js
```

### Environment Variables

```js
// env.js
console.log("Environment:", process.env.NODE_ENV || "development");
console.log("Custom variable:", process.env.MY_VARIABLE);
console.log("Database URL:", process.env.DATABASE_URL || "Not set");

// Example usage with environment variables in another file:
// NODE_ENV=production
// MY_VARIABLE=test node env.js
```

### Debugging

Powerful debugging system that integrates with Chrome DevTools:

```shell
# Start with inspector (listens on default port 9229)
node --inspect app.js

# Break on first line of application
node --inspect-brk app.js

# Specify a custom port
node --inspect=9222 app.js

# Enable remote debugging (be careful with this in production)
node --inspect=0.0.0.0:9229 app.js
```

1. Start your application with `node --inspect app.js`
2. Open Chrome and navigate to `chrome://inspect`
3. Click on "Open dedicated DevTools for Node"
4. Set breakpoints and debug your application

### Common NPM Commands

```shell
npm init # Initialize a new project
npm install # Install dependencies
npm update # Update packages
npm audit # Check for vulnerabilities
```

## What is the V8 Engine?

The V8 engine is Google's open-source JavaScript engine, used by Chrome and Node.js. It compiles JavaScript to native machine code for fast execution.

Features: Just-In-Time compilation, efficient garbage collection, ES6+ support

It allows Node.js to:

- Execute JavaScript code outside the browser
- Access operating system functionality (file system, networking, etc.)

## Node.js Architecture and the Event Loop

Node.js uses a **single-threaded, event-driven** architecture that is designed to handle many connections at once, efficiently and without blocking the main thread. The **event loop** is what makes Node.js non-blocking and efficient. It handles asynchronous operations by delegating tasks to the system and processing their results through callbacks, allowing Node.js to manage thousands of concurrent connections with a single thread.

### Diagram

1. **Client Request Phase**

- Clients send requests to the Node.js server
- Each request is added to the **Event Queue**

2. **Event Loop Phase**

- The Event Loop continuously checks the **Event Queue**
- Picks up requests one by one in a loop

3. **Request Processing**

- Simple (non-blocking) tasks are handled immediately by the main thread
- Complex/blocking tasks are offloaded to the Thread Pool

4. **Response Phase**

- When blocking tasks complete, their callbacks are placed in the **Callback Queue**
- Event Loop processes callbacks and sends responses

> We can determine if one event blocks the entire process or not.

Node.js follows these steps to handle operations, this is the priority level or the order of the execution:

1. Execute the main script (synchronous code)
2. Process any microtasks (Promises, process.nextTick)
3. Execute timers (setTimeout, setInterval)
4. Run I/O callbacks (file system, network operations)
5. Process setImmediate callbacks
6. Handle close events (like socket.on('close'))

## Asynchronous Programming

In Node.js, asynchronous operations let your program do other work while waiting for tasks like file I/O or network requests to complete.

|           Synchronous            |               Asynchronous               |
| :------------------------------: | :--------------------------------------: |
| Blocks execution until complete  |          Non-blocking execution          |
|       Simple to understand       |            Better performance            |
|         Can cause delays         |          More complex to handle          |
| Uses functions like readFileSync | Uses callbacks, promises, or async/await |

To avoid callback hell **always use Async/Await** for better readability (or Promises)

```js
// Nested callbacks are hard to read and maintain (callback hell)
User.findById(userId, (err, user) => {
  if (err) return console.error(err);
  Order.find({ userId }, (err, orders) => {
    if (err) return console.error(err);
    // Process orders...
  });
});

// do this

// Use async/await for better readability
async function getUserData(userId) {
  try {
    const user = await User.findById(userId);
    const orders = await Order.find({ userId });
    return { user, orders };
  } catch (error) {
    console.error("Failed to fetch user data:", error);
    throw error; // Re-throw or handle appropriately
  }
}
```

- ✅ Always handle errors with `try/catch`
- ✅ Run independent operations in parallel with `Promise.all`
- ❌ Avoid mixing sync and async code patterns
- ❌ Don't forget to `await` promises

### Promises

Promises in Node.js provide a cleaner way to handle asynchronous operations compared to traditional callbacks. Represent the completion (or failure) of an asynchronous operation and its result.

- Pending: Initial state, operation not completed
- ulfilled: Operation completed successfully
- Rejected: Operation failed

Once a promise is settled (either fulfilled or rejected), its state cannot change.

We can create a new one with `new Promise`, it accepts a an executor function with two parameters: `resolve` and `reject`.

```js
const myPromise = new Promise((resolve, reject) => {
  // Simulate an async operation (e.g., API call, file read)
  setTimeout(() => {
    const success = Math.random() > 0.5;

    if (success) {
      resolve("Operation completed successfully");
    } else {
      reject(new Error("Operation failed"));
    }
  }, 1000); // Simulate delay
});

// Using the Promise
myPromise
  .then((result) => console.log("Success:", result))
  .catch((error) => console.error("Error:", error.message));
```

They're useful when executing a task that might take time, like a file read, an API call or a DataBase operation. Normally in a webapp we will just use them with `fetch()` alongside `.them()` methods, but it's possible to also use `Promise.all()` to handle Parallel Execution.

Proper error handling is important, **always** use `.catch` method for them.

Example of all the method mentioned above:

```js
const fs = require("fs").promises;
const promise1 = Promise.resolve("First result");
const promise2 = new Promise((resolve) =>
  setTimeout(() => resolve("Second result"), 1000),
);
const promise3 = fs.readFile("data.txt", "utf8"); // Read local file instead of fetch

Promise.all([promise1, promise2, promise3])
  .then((results) => {
    console.log("Results:", results);
    // results[0] is from promise1
    // results[1] is from promise2
    // results[2] is the content of data.txt
  })
  .catch((error) => {
    console.error("Error in one of the promises:", error);
  });
```

| Instance Methods                                                 |                       Static Methods                       |                                     Utility Methods |
| :--------------------------------------------------------------- | :--------------------------------------------------------: | --------------------------------------------------: |
| `then(onFulfilled, onRejected)` Handles fulfillment or rejection | `Promise.all(iterable)` Waits for all promises to resolve  | `Promise.resolve(value)` Creates a resolved promise |
| `catch(onRejected)` Handles rejections                           | `Promise.race(iterable)` Waits for first promise to settle | `Promise.reject(reason)` Creates a rejected promise |
| `finally(onFinally)` Runs after promise settles                  |   `Promise.allSettled(iterable)` Waits for all to settle   |                                                     |

### Async/Await

Async/await is basically Promises with a more readable syntax. This makes your code cleaner and more maintainable. It does not block the main thread.

- `async`: Used to declare an asynchronous **function** that returns a Promise
- `await`: Used to pause execution until a Promise is resolved, can only be used inside async functions

```js
const fs = require("fs").promises;

async function readFile() {
  try {
    const data = await fs.readFile("myfile.txt", "utf8");
    console.log(data);
  } catch (error) {
    console.error("Error reading file:", error);
  }
}

readFile();
```

One of the advantages of async/await is that you can use traditional try/catch blocks for error handling.

You can combine async/await with promises.

Sometimes is better to use `Promise.all()` method when doing multiple async operations, due that it handles the fetch parallely instead of sequentialy, performing better at the end.

```js
// Sequential operation - takes ~3 seconds
async function fetchSequential() {
  console.time("sequential");
  const data1 = await fetchData(1);
  const data2 = await fetchData(2);
  const data3 = await fetchData(3);
  console.timeEnd("sequential");
  return [data1, data2, data3];
}

// Parallel operation - takes ~1 second
async function fetchParallel() {
  console.time("parallel");
  const results = await Promise.all([fetchData(1), fetchData(2), fetchData(3)]);
  console.timeEnd("parallel");
  return results;
}
```

- Remember that async functions always return Promises
- Use `Promise.all()` for concurrent operations
- Always handle errors
- Keep async functions focused on a single responsibility

## Error Handling

Understanding different error types helps in handling them appropriately:

Standard JavaScript Errors

```js
// SyntaxError
JSON.parse("{invalid json}");

// TypeError
null.someProperty;

// ReferenceError
unknownVariable;
```

System Errors

```js
// ENOENT: No such file or directory
const fs = require("fs");
fs.readFile("nonexistent.txt", (err) => {
  console.error(err.code); // 'ENOENT'
});

// ECONNREFUSED: Connection refused
const http = require("http");
const req = http.get("http://nonexistent-site.com", (res) => {});
req.on("error", (err) => {
  console.error(err.code); // 'ECONNREFUSED' or 'ENOTFOUND'
});
```

**Error-First Callbacks** are the most common pattern in Node.js core modules where the first argument to a callback is an error object (if any occurred).

**Modern Error Handling** is applied using try...catch with Async/Await. Used for both synchronous and asynchronous code.

### Custom Error Types

```js
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
    this.statusCode = 400;
  }
}

class NotFoundError extends Error {
  constructor(resource) {
    super(`${resource} not found`);
    this.name = "NotFoundError";
    this.statusCode = 404;
  }
}

// Usage
function getUser(id) {
  if (!id) {
    throw new ValidationError("User ID is required", "id");
  }
  // ...
}
```

Best practicies

| Do                                             |                                         Don't |
| :--------------------------------------------- | --------------------------------------------: |
| Handle errors at the appropriate level         |            Ignore errors (empty catch blocks) |
| Log errors with sufficient context             |     Expose sensitive error details to clients |
| Use custom error types for different scenarios |                Use try/catch for flow control |
| Clean up resources in finally blocks           |           Swallow errors without logging them |
| Validate input to catch errors early           | Continue execution after unrecoverable errors |

Remember that good error handling is not just about preventing crashes-it's about providing meaningful feedback, maintaining data integrity, and ensuring a good user experience even when things go wrong.

More about this topic in [w3schools](https://www.w3schools.com/nodejs/nodejs_error_handling.asp)

## Node.js Modules

The way we import and export modules. This can be done with **CommonJS** or **ES Modules**.

Modules are the building blocks of Node.js applications, allowing you to organize code into logical, reusable components. Modules are cached after first load. Any file with a `.js` extension is a module.

They help in:

- Organizing code into manageable files
- Encapsulating functionality
- Preventing global namespace pollution
- Improving code maintainability and reusability

> Node.js supports two module systems: CommonJS (used by default, require()) and ES Modules (ECMAScript modules, import ... from). Developers can choose between them based on their project needs.

Built-in modules, use/import them with the `require()` function:

- `fs` - File system operations
- `http` - HTTP server and client
- `path` - File path utilities
- `os` - Operating system utilities
- `events` - Event handling
- `util` - Utility functions
- `stream` - Stream handling
- `crypto` - Cryptographic functions
- `url` - URL parsing
- `querystring` - URL query string handling

```js
const http = require("http");

//Now you can use the module's features, like creating a server:

http
  .createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/html" });
    res.end("Hello World!");
  })
  .listen(8080);
```

### Creating and Exporting Modules

In Node.js, any file with a `.js` extension is a module. You can export functionality from a module in several ways:

#### Exporting Multiple Items

```js
const getCurrentDate = () => new Date().toISOString();

const formatCurrency = (amount, currency = "USD") => {
  //code
};

// Method 1: Exporting multiple items
exports.getCurrentDate = getCurrentDate;
exports.formatCurrency = formatCurrency;

// Method 2: Exporting an object with multiple properties
module.exports = { getCurrentDate, formatCurrency };
```

#### Exporting a Single Item

```js
class Logger {
  //code
}

// Exporting a single class (also function, object, etc.)
module.exports = Logger;
```

#### Using Your Modules

Import and use your custom modules using `require()` with a relative or absolute path:

```js
//importing in CommonJS
const http = require("http");
const path = require("path");

// Importing custom modules
const { getCurrentDate, formatCurrency } = require("./utils");
const Logger = require("./logger");

const logger = new Logger("App");

//...
```

### Best Practices

Module Organization

- Keep modules focused on a single responsibility
- Use meaningful file and directory names
- Group related functionality together
- Use `index.js` for module entry points

Export Patterns

- Prefer named exports for utilities
- Use default exports for single-class modules
- Document your module's API
- Handle module initialization if needed

## ES Modules

ES Modules provides a more structured and statically analyzable way to work with modules compared to CommonJS, with benefits like tree-shaking for smaller builds.

Here's how CommonJS and ES Modules differ:

| Feature             | CommonJS                 | ES Modules                       |
| ------------------- | ------------------------ | -------------------------------- |
| File Extension      | .js (default)            | .mjs (or .js with proper config) |
| Import Syntax       | require()                | import                           |
| Export Syntax       | module.exports / exports | export / export default          |
| Import Timing       | Dynamic (runtime)        | Static (parsed before execution) |
| Top-level Await     | Not supported            | Supported                        |
| File URL in Imports | Not required             | Required for local files         |

### How to enable ES Modules

1. Using the `.mjs` File Extension. Node.js will automatically treat these files as ES Modules.
2. Setting "type": "module" in `package.json`. To use ES Modules with regular .js files in all your project.
3. Running the file with the flag: `node --input-type=module script.js`.

### Exporting and importing in ES Modules

We can export each function directly

```js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

//We could also do the following:
export { add, subtract };
```

Default export. Only one per module (file).

```js
// Only one default export per module
export default function mainFunction() {
  console.log("I am the default export");
}

//or at the end of the file instead of the beggining the function
export default mainFunction;
```

Import specific named exports, renaming imports and import all named exports as an object.

```js
import { sayHello, sayGoodbye } from "./greetings.mjs";
sayHello(); // Hello

// Rename imports to avoid naming conflicts
import { add as sum, subtract as minus } from "./math.mjs";

// Import all named exports as an object
import * as math from "./math.mjs";
console.log(math.add(7, 4)); // 11
```

Import the default export. You can name the default import anything you want.

```js
import mainFunction from "./main.mjs";
mainFunction();
```

Import both default and named exports

```js
import main, { VERSION } from "./main.mjs";
console.log(VERSION); // 1.0.0
main(); // Main function
```

Import Node.js built in modules. Using `node:` makes it explicit that you're importing Node.js built-ins.

```js
import fs from "node:fs";
import { join } from "path";
```

### Dynamic Imports

ES Modules support **dynamic imports**, allowing you to load modules conditionally or on-demand. Use Case: they're great for code-splitting, lazy-loading modules, or conditionally loading modules based on runtime conditions.

```js
// app.mjs
async function loadModule(moduleName) {
  try {
    // Dynamic import returns a promise
    const module = await import(`./${moduleName}.mjs`);
    return module;
  } catch (error) {
    console.error(`Failed to load ${moduleName}:`, error);
  }
}

// Load a module based on a condition
const moduleName = process.env.NODE_ENV === "production" ? "prod" : "dev";

loadModule(moduleName).then((module) => {
  module.default(); // Call the default export
});

// Or with simpler await syntax
(async () => {
  const mathModule = await import("./math.mjs");
  console.log(mathModule.add(10, 5)); // 15
})();
```

### Top-level Await

Unlike CommonJS, ES Modules support top-level await, allowing you to use await outside of async functions at the module level.

```js
// Top-level await - the module's execution pauses here
const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
const data = await response.json();

export { data };

// When another module (file) imports this one, it will only get the exports after all the top-level await operations have completed
```

Especially useful for:

- Loading configuration from files or remote sources
- Connecting to databases before exporting functionality
- Conditional imports or module initialization

### Best Practices

1. Be Clear About File Extensions. Always include file extensions in your import statements for local files. Do `'./utils.mjs'`, Do not `'./utils'`.
2. Choose the Right Export Style. Use named exports for multiple functions/values, and default exports for main functionality
3. When working with a codebase that mixes CommonJS and ES Modules:
   - ES Modules can import from CommonJS modules using default import
   - CommonJS can require() ES Modules only with **dynamic import()**

   ```js
   // Importing CommonJS module from ESM
   import fs from "fs"; // The default import is module.exports

   // Importing ESM from CommonJS (Node.js 12+)
   // In a CommonJS module:
   (async () => {
     const { default: myEsmModule } = await import("./my-esm-module.mjs");
   })();
   ```

4. Dual Package Hazard. For npm packages that support both module systems, use the "exports" field in `package.json` to specify different entry points:

```js
{
  "name": "my-package",
  "exports": {
    ".": {
      "import": "./index.mjs",
      "require": "./index.cjs"
    }
  }
}
```

## What is NPM?

NPM is a package manager for Node.js packages, or modules if you like. A package contains all the files you need for a module. Modules are JavaScript libraries you can include in your project.

NPM creates a folder named "node_modules", where the package will be placed after install. Can be done as `npm install package-name`.

Once the package is installed, it is ready to use. Include the package the same way you include any other module in a file: `let uc = require('upper-case');`

#### Global Packages

Packages can be installed globally, making them available as command-line tools anywhere on your system. Global packages are typically used for CLI tools and utilities. `npm install -g package-name`

- Update all packages in your project: `npm update`
- Update a specific package: `npm update package-name`
- Check for outdated packages: `npm outdated`

> To update npm itself, run: `npm install -g npm@latest`

- Remove a package: `npm uninstall package-name`
- Remove a global package: `npm uninstall -g package-name`
- Remove a package and its dependencies: `npm uninstall --save package-name`

### What is package.json?

package.json is a special file that describes your Node.js project. It contains information about your app, such as its name, version, dependencies, scripts, and more. This file is essential for managing and sharing Node.js projects, especially when using npm (Node Package Manager).

Create a package.json file: `npm init`

Example

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "A simple Node.js app",
  "main": "index.js",
  "type": "module", // or "commonjs"
  "keywords": ["example", "package", "node"],

  "scripts": {
    "start": "node index.js"
  },
  "author": "Your Name",
  "license": "ISC",
  "homepage": "https://example.com/my-package",

  //Specify Node.js and npm version requirements:
  "engines": {
    "node": ">=14.0.0 <17.0.0",
    "npm": ">=6.0.0"
  }
}
```

When you install a package with npm, it is added to the dependencies section of `package.json`. It also provides the version ranges.

There are also **Development-only dependencies** the idea is to add packages that will not be installed in production, examples are: Jest, ESlint.

- Installing Without Saving to package.json: `npm install package-name --no-save`
- Install and save to devDependencies: `npm install --save-dev package-name`
- Install exact version: `npm install package-name@1.2.3`
- Install optional dependencies: `npm install package-name -O`. These packages enhance functionality but aren't required for the core application to work.

```json
"dependencies": {
  "express": "^4.18.2",
  "mongoose": "~7.0.0",
  "lodash": "4.17.21"
},
```

Define custom scripts that can be run with `npm run script-name`. This to automate tasks like: Running your app, Testing, Building, Cleaning up files.

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js",
  "test": "jest",
  "build": "webpack --mode production",
  "lint": "eslint .",
  "prepare": "husky install"
}
```

#### Best Practices (shared with below)

- Use `npm ci` in CI/CD pipelines for reproducible builds
- Use `.npmignore` to exclude unnecessary files from published packages

### Dependency Management

The key components of Node.js dependency management include:

- The `package.json` file for declaring dependencies
- Lock files (`package-lock.json` or `yarn.lock`) for dependency versioning
- Package manager commands to install, update, and remove packages
- Security tools to identify and fix vulnerabilities

Example: Different Version Specifications

```json
{
  "dependencies": {
    "express": "^2.8.1", // Any 2.x.x version (2.8.1 or higher)
    "lodash": "~2.8.1", // Any 2.8.x version (2.8.1 or higher)
    "moment": "2.8.1", // Exactly version 2.8.1
    "axios": ">=2.8.1", // Version 2.8.1 or any higher version
    "debug": "2.x", // Any version starting with 2
    "upper-case": "*" // Any version (not recommended for production)
  }
}
```

**Lock files** (package-lock.json and yarn.lock) ensure consistent installations across different environments by recording the exact version of each package and its dependencies.

- Audit Your dependencies for known vulnerabilities: `npm audit`
- Fix security vulnerabilities: `npm audit fix`
- Clearing the npm cache: npm cache clean --force

Deleting node_modules and Reinstalling

```shell
rm -rf node_modules
rm package-lock.json
npm install
```

Fixing broken dependencies: `npm rebuild`

#### Best Practices

- **Use exact versions in production:** Pin your dependencies to exact versions to prevent unexpected updates.
  - **Minimize dependencies:** Only include packages that you actually need.
  - **Regularly update dependencies:** Keep your dependencies up to date to benefit from security patches and new features.
- **Audit your dependencies:** Regularly check for known vulnerabilities in your dependencies.
- **Use scoped packages:** For internal packages, use scopes to avoid naming conflicts.
- **Document your dependencies:** Include information about why each dependency is needed in your project's documentation.
- **Use a lock file (package-lock.json and yarn.lock):** Always commit your lock file to version control (git and github).

## HTTP Module
