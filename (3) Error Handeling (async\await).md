---

# 1️⃣ What is Error Handling?

**Error handling** is how we **detect**, **control**, and **respond** when something goes wrong in our program instead of letting it crash.

 Examples of errors

* Invalid user input
* API call fails
* Database connection fails
* File not found
* Dividing by zero
* Accessing undefined values

Without error handling → **program crashes**
With error handling → **program survives gracefully**

---

## Types of Errors in JavaScript

### 1️⃣ Syntax Errors

Errors in writing code.

```js
if (true {
  console.log("Hello");
}
```

❌ Missing `)` → program won’t run at all.

---

### 2️⃣ Runtime Errors

Happen while the program is running.

```js
let x = undefined;
console.log(x.length);
```

❌ Cannot read property `length` of undefined

---

### 3️⃣ Logical Errors

Code runs, but result is wrong.

```js
let sum = 10 - 5; // wanted addition
```

❌ No error, but wrong logic.

---

# 2️⃣ try...catch – Core of Error Handling

### Basic Syntax

```js
try {
  // risky code
} catch (error) {
  // handle error
}
```

### Example

```js
try {
  let result = JSON.parse("{name: Guru}");
  console.log(result);
} catch (error) {
  console.log("Invalid JSON format");
}
```

 `JSON.parse` throws an error
 `catch` prevents app crash

---

## How try–catch Works (Important Concept)

* JS **tries** to execute code in `try`
* If an error occurs:

  * Execution jumps to `catch`
  * Remaining `try` code is skipped
* Program continues after `catch`

---

## The `error` Object

```js
catch (error) {
  console.log(error.name);    // SyntaxError
  console.log(error.message); // Unexpected token
  console.log(error.stack);   // Where it happened
}
```

---

## finally Block (Always Runs)

```js
try {
  console.log("Try block");
} catch (e) {
  console.log("Catch block");
} finally {
  console.log("Always runs");
}
```

 Used for

* Closing DB connections
* Releasing resources
* Cleanup tasks

---

# 3️⃣ Throwing Custom Errors

You can **create your own errors**.

```js
function withdraw(balance, amount) {
  if (amount > balance) {
    throw new Error("Insufficient balance");
  }
  return balance - amount;
}

try {
  withdraw(500, 1000);
} catch (e) {
  console.log(e.message);
}
```

 This is **very important in backend interviews**.

---

# 4️⃣ Why Async Code Needs Special Handling

JavaScript is **non-blocking** (single-threaded).

### Example of Async Code

```js
setTimeout(() => {
  console.log("Hello");
}, 1000);

console.log("World");
```

Output

```
World
Hello
```

Async code **does not wait**.

---

# 5️⃣ Promises – Foundation of async/await

### Promise States

* **Pending**
* **Fulfilled**
* **Rejected**

### Creating a Promise

```js
const promise = new Promise((resolve, reject) => {
  let success = true;

  if (success) {
    resolve("Data received");
  } else {
    reject("Something went wrong");
  }
});
```

---

## Handling Promises (Old Way)

```js
promise
  .then(data => console.log(data))
  .catch(err => console.log(err));
```

❌ Can become messy (promise chaining)

---

# 6️⃣ async / await – Clean & Modern Way

### What is `async`?

* Makes a function **always return a Promise**
* Allows usage of `await` inside it

```js
async function fetchData() {
  return "Hello";
}
```

Actually returns

```js
Promise.resolve("Hello")
```

---

### What is `await`?

* Pauses execution **inside async function**
* Waits for Promise to resolve or reject

```js
const data = await fetchData();
```

---

## Example Without async/await

```js
fetchUser()
  .then(user => fetchOrders(user.id))
  .then(orders => console.log(orders))
  .catch(err => console.log(err));
```

 Hard to read

---

## Same Example With async/await

```js
async function getOrders() {
  try {
    const user = await fetchUser();
    const orders = await fetchOrders(user.id);
    console.log(orders);
  } catch (err) {
    console.log(err);
  }
}
```

 Clean, readable, synchronous-like

---

# 7️⃣ Error Handling with async / await

 **Most important rule**:

> `await` errors must be handled using `try...catch`

### Example

```js
async function loadData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("API failed:", error.message);
  }
}
```

---

## What Happens Internally?

```js
await somePromise;
```

⬇️ equivalent to

```js
somePromise.then().catch();
```

---

# 8️⃣ Handling Multiple Async Calls

### Sequential (One by One)

```js
const a = await task1();
const b = await task2();
```

⏳ Slower

---

### Parallel (Best Practice)

```js
const [a, b] = await Promise.all([task1(), task2()]);
```

🚀 Faster

---

### Handling Partial Failure

```js
const results = await Promise.allSettled([task1(), task2()]);
```

✔️ Won’t crash if one fails

---

# 9️⃣ Common Mistakes (VERY IMPORTANT)

❌ Using `await` outside async function

```js
await fetchData(); // ERROR
```

❌ Forgetting try–catch

```js
await fetchData(); // Unhandled promise rejection
```

❌ Mixing `.then()` with `await`

```js
await fetchData().then();
```

---

# 🔟 Real Backend Example (Express.js)

```js
app.get("/users", async (req, res) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```


---

# One-Line Summary

> **Error handling** ensures applications don’t crash when something fails.
> **async/await** simplifies asynchronous code by making it look synchronous, and errors are handled using `try...catch` blocks.

---

