Let’s go deep into **Streams & Buffers** in Node.js, which are fundamental for handling **large data efficiently** without blocking the event loop.

---

## 1️⃣ What Are Buffers?

- **Buffer** = temporary memory storage for **binary data**.
    
- Useful because Node.js deals with **raw binary data** (files, network sockets, etc.).
    
- Unlike normal strings, buffers can handle **any type of data** (text, images, video).
    

### Creating Buffers

```js
// From a string
const buf1 = Buffer.from("Hello World", "utf8");

// Allocate buffer of 10 bytes
const buf2 = Buffer.alloc(10);

console.log(buf1.toString()); // "Hello World"
console.log(buf2); // <Buffer 00 00 00 00 00 00 00 00 00 00>
```

- Buffers are **fixed-size** and **not resizable**
    
- Can convert between strings and buffers (`toString`, `Buffer.from`)
    

---

## 2️⃣ What Are Streams?

- **Stream** = sequence of data **readable or writable over time**
    
- **Why streams?** Large files or network responses can **exceed memory** if loaded all at once.
    
- Node.js provides **4 types of streams**:
    

|Type|Description|
|---|---|
|Readable|Can be read (e.g., `fs.createReadStream`)|
|Writable|Can be written to (e.g., `fs.createWriteStream`)|
|Duplex|Both readable & writable (e.g., network sockets)|
|Transform|Can modify data while reading/writing (e.g., gzip compression)|

---

## 3️⃣ Basic Readable Stream Example

```js
const fs = require("fs");

const readable = fs.createReadStream("./largeFile.txt", { encoding: "utf8" });

readable.on("data", (chunk) => {
  console.log("Received chunk:", chunk.length);
});

readable.on("end", () => {
  console.log("Finished reading file");
});
```

- `.on("data")` → emits chunks of data
    
- `.on("end")` → emitted when stream finishes
    

---

## 4️⃣ Basic Writable Stream Example

```js
const writable = fs.createWriteStream("./output.txt");

writable.write("Hello World\n");
writable.write("Streaming is efficient!\n");

writable.end(); // signal that writing is finished
writable.on("finish", () => console.log("Done writing"));
```

---

## 5️⃣ Piping Streams

- **Pipe** allows you to **connect a readable stream to a writable stream** efficiently:
    

```js
const readStream = fs.createReadStream("./input.txt");
const writeStream = fs.createWriteStream("./output.txt");

readStream.pipe(writeStream);
```

- Handles **chunking** automatically
    
- More memory-efficient than reading/writing whole files
    

---

## 6️⃣ Transform Streams Example

- Useful for modifying data **on the fly** (compression, encryption, uppercase, etc.)
    

```js
const { Transform } = require("stream");

const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

fs.createReadStream("./input.txt")
  .pipe(upperCaseTransform)
  .pipe(fs.createWriteStream("./output.txt"));
```

- `chunk` = Buffer
    
- `callback()` signals that transformation is complete
    

---

## 7️⃣ Key Takeaways

1. **Buffers** = temporary storage for raw binary data
    
2. **Streams** = handle large data efficiently without blocking memory
    
3. **Readable** = emits chunks of data
    
4. **Writable** = accepts chunks of data
    
5. **Duplex & Transform** = read/write and transform data
    
6. **Pipe** = connects streams seamlessly
    

---

