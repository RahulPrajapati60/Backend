### 1. Event Loop – The Heart of Node.js 

Node.js is **single-threaded** (JavaScript execution happens on one thread), but it handles thousands of concurrent connections thanks to the **event loop** + **libuv** (C library).

#### Phases of the Event Loop (very important order!)

| Phase                  | What happens                                                                 | Typical callbacks                                      | Important functions                          |
|-----------------------|-------------------------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------|
| 1. Timers             | Executes callbacks from `setTimeout()` and `setInterval()` whose delay expired | Delayed functions                                              | setTimeout, setInterval                       |
| 2. Pending Callbacks  | Executes I/O callbacks deferred to the next loop (rarely used directly)       | Some system errors                                             | —                                             |
| 3. Idle, Prepare      | Internal only (almost never touch)                                            | —                                                              | —                                             |
| 4. Poll               | **Most important phase** — retrieves new I/O events (most file/socket operations) | `fs.readFile`, http requests, net connections, most async stuff | —                                             |
| 5. Check              | Executes `setImmediate()` callbacks                                           | setImmediate callbacks                                         | setImmediate                                  |
| 6. Close callbacks    | Executes 'close' event handlers (sockets, files, etc.)                        | socket.on('close'), process.on('exit')                         | —                                             |

Between phases → **microtask queue** is drained (very important!)

Microtasks (executed **before** next phase starts):

- `process.nextTick()` callbacks (highest priority)
- `Promise.then/catch/finally` / `await` resolutions

#### Classic tricky example – Guess the output:

```js
console.log("1");

setTimeout(() => console.log("2"), 0);
setImmediate(() => console.log("3"));

Promise.resolve().then(() => console.log("4"));

process.nextTick(() => console.log("5"));

console.log("6");
```



### 2. Modules – CommonJS vs ESM
Node.js now supports **both** systems very well, but direction is clear:

| Feature                  | CommonJS (require/module.exports)               | ES Modules (import/export)                        |
|--------------------------|--------------------------------------------------|----------------------------------------------------|
| Syntax                   | `const fs = require('fs')`                       | `import fs from 'fs'`                              |
| Loading                  | Synchronous                                      | Asynchronous (but top-level await allowed)         |
| File extension           | Usually `.js`                                    | Needs `.js` or `.mjs` (or `"type":"module"`)       |
| Circular dependencies    | Works (partial)                                  | Works but stricter                                 |
| Top-level await          | No                                               | Yes                                                |
| Dynamic import()         | Yes (but returns Promise)                        | Yes (native)                                       |
| npm ecosystem maturity   | Still ~60–70% of packages                        | Growing very fast (most new libs are dual)         | 
| Recommended for new code | Only if maintaining old project                  | Yes — future-proof                                 | 


package.json (pick one):

```json
{
  "type": "module"          // ← all .js files are ESM
}
```

or

```json
{
  "type": "commonjs",       // default if missing
  "exports": {
    "./utils": "./src/utils.js"   // modern way even in CJS projects
  }
}
```

Quick ESM + top-level await example:

```js
// server.js
import http from 'node:http';

const data = await fetchDataFromDB();   // ← legal in ESM!

const server = http.createServer(...);
```

### 3. fs – File System Module

Two APIs exist 

| Style          | Module               | Best for                          | Example                                |
|----------------|----------------------|------------------------------------|----------------------------------------|
| **Callback**   | `fs`                 | Very old code                     | `fs.readFile(..., cb)`                 |
| **Sync**       | `fs` + `Sync` suffix | CLI scripts, startup only         | `fs.readFileSync()`                    |
| **Promise**    | `fs/promises`        | Modern async/await code (★)       | `await fs.readFile(...)`               |

**Recommended pattern**

```js
import { readFile, writeFile } from 'node:fs/promises';

async function main() {
  try {
    const data = await readFile('config.json', 'utf-8');
    const json = JSON.parse(data);

    await writeFile('output.txt', 'Hello from async fs!', 'utf-8');
    
    console.log('Done');
  } catch (err) {
    console.error('FS error:', err);
  }
}

main();
```

Watch out for:

- `fs.watch` / `fs.watchFile` — still callback-based mostly
- Streams — very powerful for large files

```js
import { createReadStream } from 'node:fs';

createReadStream('bigfile.log', { encoding: 'utf-8' })
  .pipe(process.stdout);
```

### 4. http – Classic HTTP Server (still relevant)

many people start with the built-in `http` module before jumping to Express/Fastify/Hono.

Minimal but complete server (ESM style):

```js
// server.js
import http from 'node:http';

const server = http.createServer((req, res) => {
  const { method, url } = req;

  if (url === '/' && method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello from pure Node.js HTTP 🎉');
    return;
  }

  if (url === '/json' && method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ message: 'Modern Node 2026', version: process.version }));
    return;
  }

  // 404
  res.writeHead(404);
  res.end('Not Found');
});

const PORT = process.env.PORT || 3000;

server.listen(PORT, () => {
  console.log(`Server running → http://localhost:${PORT}`);
});
```
