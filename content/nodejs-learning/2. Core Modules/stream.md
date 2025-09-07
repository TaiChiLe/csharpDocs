Streams are one of the **most powerful and fundamental** parts of Node.js. They let you process data **chunk by chunk** instead of loading it all at once. Let’s go through the three main types: **Readable, Writable, and Transform**.

---

## 📂 1. What Are Streams?

- A **stream** is an abstract interface for working with streaming data.
    
- Built on top of **EventEmitter**.
    
- Useful for **large files**, **networking**, or **real-time data**.
    

---

## 📖 2. Readable Streams

A source of data that you can consume.  
Examples: `fs.createReadStream()`, HTTP requests (`req`).

```js
const fs = require("fs");

const readStream = fs.createReadStream("bigfile.txt", "utf8");

readStream.on("data", (chunk) => {
  console.log("Received chunk:", chunk.length);
});

readStream.on("end", () => {
  console.log("Finished reading file.");
});
```

Key events:

- `data` → emits chunks of data.
    
- `end` → no more data.
    
- `error` → something went wrong.
    

---

## ✍️ 3. Writable Streams

A destination you can write data into.  
Examples: `fs.createWriteStream()`, HTTP responses (`res`).

```js
const writeStream = fs.createWriteStream("output.txt");

writeStream.write("First line\n");
writeStream.write("Second line\n");
writeStream.end(); // finish the stream

writeStream.on("finish", () => {
  console.log("All data written!");
});
```

Key methods:

- `.write(chunk)` → write data.
    
- `.end()` → signal no more data will be written.
    

---

## 🔄 4. Piping (Readable → Writable)

Easiest way to connect streams:

```js
const readStream = fs.createReadStream("bigfile.txt");
const writeStream = fs.createWriteStream("copy.txt");

readStream.pipe(writeStream);
```

👉 Efficient: Node handles **backpressure** automatically (slowing down reading if writing is slower).

---

## 🔀 5. Transform Streams

A special type of **Duplex stream** that can read and write, but also **modify** data as it passes through.

Example: Convert text to uppercase:

```js
const { Transform } = require("stream");

const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

process.stdin.pipe(upperCaseTransform).pipe(process.stdout);
```

Usage:

- Type something in terminal → output is in uppercase.
    

---

## ⚙️ 6. Stream Types Recap

|Stream Type|Example in Node.js|Direction|
|---|---|---|
|**Readable**|`fs.createReadStream()`, `http.IncomingMessage`|Data **out**|
|**Writable**|`fs.createWriteStream()`, `http.ServerResponse`|Data **in**|
|**Duplex**|`net.Socket` (read + write)|Both|
|**Transform**|`zlib.createGzip()`, custom transforms|Both + modify|

---

## ✅ In Short

- **Readable** → consume data (files, requests).
    
- **Writable** → send data (files, responses).
    
- **Transform** → modify data in the middle (compression, encryption).
    
- Use `.pipe()` to connect streams efficiently and handle backpressure.
    

---

👉 Do you want me to go **deeper into backpressure & `stream.pipeline()`** (the modern safe way to connect streams), or move on to another Node.js core concept like **child processes**?