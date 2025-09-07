The **`fs` (File System)** module is one of the most useful in Node.js. Let’s explore it step by step: **basic read/write** → **async vs sync** → **streams**.

---

## 📂 1. The `fs` Module

- Built-in module for interacting with the file system.
    
- Gives access to both **synchronous** and **asynchronous** methods.
    
- Import it:
    

```js
const fs = require("fs");
```

---

## 📖 2. Reading Files

### Asynchronous (non-blocking) → preferred

```js
fs.readFile("data.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log("File content:", data);
});
console.log("Reading file...");
```

👉 Doesn’t block the event loop. The callback runs when reading is finished.

### Synchronous (blocking)

```js
const data = fs.readFileSync("data.txt", "utf8");
console.log("File content:", data);
```

👉 Blocks the thread until the file is fully read. Rarely recommended in servers.

---

## ✍️ 3. Writing Files

### Asynchronous

```js
fs.writeFile("output.txt", "Hello World!", (err) => {
  if (err) throw err;
  console.log("File saved!");
});
```

### Append to file

```js
fs.appendFile("output.txt", "\nAnother line", (err) => {
  if (err) throw err;
  console.log("Line appended!");
});
```

### Synchronous

```js
fs.writeFileSync("output.txt", "Hello World!");
```

---

## 🔄 4. Streams

For large files, reading/writing everything at once is inefficient.  
**Streams** let you process data in **chunks** without loading the entire file into memory.

### Read Stream

```js
const readStream = fs.createReadStream("bigfile.txt", "utf8");

readStream.on("data", (chunk) => {
  console.log("Received chunk:", chunk.length);
});

readStream.on("end", () => {
  console.log("Finished reading file.");
});
```

👉 Useful for video/audio streaming, logs, big datasets.

### Write Stream

```js
const writeStream = fs.createWriteStream("output.txt");

writeStream.write("First line\n");
writeStream.write("Second line\n");
writeStream.end();

writeStream.on("finish", () => {
  console.log("Write complete.");
});
```

---

## 🔗 5. Piping Streams

The **real power**: connecting streams together.

Example: Copy a file efficiently:

```js
const readStream = fs.createReadStream("bigfile.txt");
const writeStream = fs.createWriteStream("copy.txt");

readStream.pipe(writeStream);
```

👉 Data flows in chunks from the read stream into the write stream.

---

## ✅ In Short

- `fs.readFile` / `fs.writeFile` → simple file operations (async preferred).
    
- Sync methods block the event loop — avoid in servers.
    
- Streams (`createReadStream`, `createWriteStream`) let you handle **large files efficiently**.
    
- `pipe()` connects streams together for powerful data handling (file copy, compression, network).
    

---

