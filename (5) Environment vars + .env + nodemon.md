
## 1️ What are Environment Variables?

**Environment variables** are values stored **outside your code**.

Examples

* Database password
* JWT secret key
* Port number
* API keys

 **Why not hard-code them?**

* Security 🔐
* Different values for **dev / test / prod**
* Easy to change without touching code

Example (❌ bad)

```js
const password = "mySecret123";
```

Example (✅ good)

```js
process.env.DB_PASSWORD
```

---

## 2️ What is `.env` file?

A `.env` file is used to **store environment variables locally**.

### 📁 Project structure

```
project/
│── node_modules/
│── .env
│── index.js
│── package.json
```

### 📄 `.env` file

```env
PORT=5000
DB_URL=mongodb://localhost:27017/mydb
JWT_SECRET=mySuperSecretKey
```

 **Rules**

* No quotes
* No spaces around `=`
* Never commit `.env` to Git ❌
  (add it to `.gitignore`)

---

## 3️ How Node.js reads environment variables

Node gives access via

```js
process.env.VARIABLE_NAME
```

Example

```js
console.log(process.env.PORT);
```

But ❗ Node **cannot read `.env` automatically**.

So we use a package 

---

## 4️ `dotenv` package (VERY IMPORTANT)

###  Install

```bash
npm install dotenv
```

### 📄 index.js

```js
import dotenv from 'dotenv';
dotenv.config();

const PORT = process.env.PORT || 3000;

console.log("Server running on port", PORT);
```

 `dotenv.config()` loads variables from `.env` into `process.env`.

---

## 5️ What is nodemon?

**nodemon** automatically **restarts the server** when files change.

Without nodemon :

```bash
node index.js
# stop
# run again
```

With nodemon :

```bash
nodemon index.js
```

---

## 6️ Install nodemon

### Option 1️ (Dev dependency – recommended)

```bash
npm install --save-dev nodemon
```

### Option 2️ (Global)

```bash
npm install -g nodemon
```

---

## 7️ Using nodemon with `.env`

### 📄 package.json

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  }
}
```

### ▶ Run

```bash
npm run dev
```

nodemon will:

* Read `.env`
* Restart server on file change
* Keep development fast 🚀

---

## 8️ Complete small example

### 📄 `.env`

```env
PORT=4000
```

### 📄 index.js

```js
import express from 'express';
import dotenv from 'dotenv';

dotenv.config();

const app = express();

const PORT = process.env.PORT;

app.get('/', (req, res) => {
  res.send("Hello World");
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### ▶ Run

```bash
npm run dev
```

---

## 9️ Common Interview Questions 💡

**Q: Why use `.env`?**
👉 To store sensitive data securely and change config without touching code.

**Q: Difference between `.env` and environment variables?**
👉 `.env` is just a file to **load env vars locally**.

**Q: Is `.env` used in production?**
👉 Usually ❌
Production uses **server-level environment variables**.

---
