The **`path` module** is another core Node.js tool that pairs perfectly with `fs`. It helps you work with file and directory paths in a **cross-platform safe** way.

---

## 📂 1. Importing `path`

```js
const path = require("path");
```

Why use it?

- Windows uses `\` (backslashes).
    
- Linux/macOS use `/` (forward slashes).
    
- `path` makes sure your code works everywhere.
    

---

## 🛠️ 2. Common Methods

### `path.join()`

- Joins path segments together using the correct separator.
    

```js
const filePath = path.join(__dirname, "folder", "file.txt");
console.log(filePath);
// /Users/tyson/project/folder/file.txt (mac/Linux)
// C:\Users\tyson\project\folder\file.txt (Windows)
```

---

### `path.resolve()`

- Resolves a sequence of paths into an **absolute path**.
    
- Similar to `cd` in a terminal.
    

```js
const absPath = path.resolve("folder", "file.txt");
console.log(absPath);
// /Users/tyson/currentDir/folder/file.txt
```

---

### `path.basename()`

- Returns the last portion (file name).
    

```js
console.log(path.basename("/users/tyson/project/file.txt"));
// file.txt
```

---

### `path.dirname()`

- Returns the directory part.
    

```js
console.log(path.dirname("/users/tyson/project/file.txt"));
// /users/tyson/project
```

---

### `path.extname()`

- Returns the extension.
    

```js
console.log(path.extname("file.txt"));  // .txt
console.log(path.extname("archive.tar.gz")); // .gz
```

---

### `path.parse()` & `path.format()`

- Breaks a path into parts, or builds it back.
    

```js
const parsed = path.parse("/users/tyson/project/file.txt");
console.log(parsed);
/*
{
  root: '/',
  dir: '/users/tyson/project',
  base: 'file.txt',
  ext: '.txt',
  name: 'file'
}
*/

// Rebuild path
const formatted = path.format(parsed);
console.log(formatted); // /users/tyson/project/file.txt
```

---

### `path.normalize()`

- Fixes weird/messy paths.
    

```js
console.log(path.normalize("/users/tyson//project/../file.txt"));
// /users/tyson/file.txt
```

---

## ✅ In Short

- `join()` → safely glue segments.
    
- `resolve()` → get absolute path.
    
- `basename()` / `dirname()` / `extname()` → dissect path.
    
- `parse()` / `format()` → full breakdown/rebuild.
    
- `normalize()` → clean up messy paths.
    

---
