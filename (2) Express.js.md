---

## Express.js Basics

Express.js is a fast, minimal, and flexible Node.js web framework. Its core functionality revolves around three main concepts:


1. **Routing**
2. **Middleware**
3. **Serving Static Files**

---

## 1. Routing

Routing defines how an application responds to client requests for specific URLs (endpoints) and HTTP methods such as **GET, POST, PUT, DELETE**, etc.

Each route can have one or more handler functions that are executed when the route matches.

### Syntax

```js
app.METHOD(PATH, HANDLER)
```

### Example

```js
const express = require('express');
const app = express();

// Responds to GET requests at the root URL (/)
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Responds to POST requests at /api/users
app.post('/api/users', (req, res) => {
  // Logic to handle user creation
  res.send('User created.');
});
```

### express.Router

For better code organization, Express provides the `express.Router()` class.
It allows you to create **modular, mountable route handlers** (often called “mini-apps”).

This is useful when your application grows and you want to separate routes into different files.

---

## 2. Middleware

Middleware functions are the backbone of an Express application.
They run **between receiving a request and sending a response**.

Each middleware function has access to:

* `req` (request object)
* `res` (response object)
* `next()` (passes control to the next middleware)

### What Middleware Can Do

* Execute any code
* Modify the request or response objects
* End the request–response cycle
* Call the next middleware using `next()`

### Using Middleware

Middleware is commonly added using `app.use()`.

#### Example: Custom Logging Middleware

```js
app.use((req, res, next) => {
  console.log(`${req.method} request to ${req.url}`);
  next(); // Pass control to the next middleware
});
```

### Built-in Middleware

Express provides built-in middleware such as:

* `express.json()` – parses JSON request bodies
* `express.urlencoded()` – parses form data

```js
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

---

## 3. Serving Static Files

Express includes the built-in `express.static()` middleware to serve static assets like:

* HTML files
* CSS files
* Images
* JavaScript files

This allows clients to access these files directly without creating routes for each one.

### Basic Usage

```js
const path = require('path');

app.use(express.static(path.join(__dirname, 'public')));
```

If your folder structure is:

```
public/
 └── css/
     └── style.css
```

The file can be accessed at:

```
http://localhost:3000/css/style.css
```

### Virtual Path Prefix

You can mount static files under a virtual path prefix:

```js
app.use('/assets', express.static('public'));
```

Now the same file will be accessible at:

```
http://localhost:3000/assets/css/style.css
```

---
