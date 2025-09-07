Let’s go through **file upload and download handling** in Node.js, which is essential for APIs that deal with **user files, images, or documents**. We’ll cover it using **Express**.

---

## 1️⃣ File Upload Handling

### a) Using `multer`

- `multer` = middleware for handling **`multipart/form-data`**, which is the encoding used for file uploads.
    

### Install

```bash
npm install multer
```

### b) Basic Setup

```js
const express = require("express");
const multer = require("multer");
const path = require("path");

const app = express();

// Storage configuration
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, "uploads/"); // folder to save files
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + path.extname(file.originalname)); // unique filename
  }
});

const upload = multer({ storage });

// Single file upload
app.post("/upload", upload.single("file"), (req, res) => {
  res.json({ message: "File uploaded", file: req.file });
});

// Multiple files upload
app.post("/upload-multiple", upload.array("files", 5), (req, res) => {
  res.json({ message: "Files uploaded", files: req.files });
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

**Notes:**

- `req.file` → for single file
    
- `req.files` → for multiple files
    
- Always **validate file type and size**
    

---

### c) File Type & Size Validation

```js
const upload = multer({
  storage,
  limits: { fileSize: 5 * 1024 * 1024 }, // 5MB
  fileFilter: (req, file, cb) => {
    const allowedTypes = /jpeg|jpg|png|pdf/;
    const extname = allowedTypes.test(path.extname(file.originalname).toLowerCase());
    if (extname) return cb(null, true);
    cb(new Error("Only images or PDFs are allowed"));
  }
});
```

---

## 2️⃣ File Download Handling

- Use `res.download()` or `res.sendFile()` in Express
    

```js
app.get("/download/:filename", (req, res) => {
  const filePath = path.join(__dirname, "uploads", req.params.filename);
  res.download(filePath, (err) => {
    if (err) res.status(404).json({ error: "File not found" });
  });
});
```

- `res.download()` → triggers **file download prompt** in browser
    
- `res.sendFile()` → serves file inline
    

---

## 3️⃣ Streaming Large Files

- For **large files**, use **streams** instead of reading the entire file into memory:
    

```js
const fs = require("fs");

app.get("/stream/:filename", (req, res) => {
  const filePath = path.join(__dirname, "uploads", req.params.filename);
  const readStream = fs.createReadStream(filePath);
  
  readStream.on("error", () => res.status(404).json({ error: "File not found" }));
  readStream.pipe(res); // stream file directly to response
});
```

- Efficient for **large media files** and **video/audio streaming**
    

---

## 4️⃣ Security Considerations

1. **Validate file type and size** → prevent malicious files
    
2. **Store files outside of the public root** → avoid direct access
    
3. **Generate unique filenames** → prevent collisions and overwrites
    
4. **Scan uploaded files** (optional) → virus/malware scanning
    

---

## ✅ Summary

|Operation|Method / Library|Notes|
|---|---|---|
|Upload|`multer`|Single/multiple files, validation, disk/memory storage|
|Download|`res.download` / `res.sendFile`|Trigger download or inline serve|
|Streaming|`fs.createReadStream()`|Efficient for large files|
|Security|Validate type/size, store safely|Prevent malicious access|

---

