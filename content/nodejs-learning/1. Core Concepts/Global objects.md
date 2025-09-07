These **global objects** are core to working with Node.js. Let’s break them down one by one.

---

## 🌍 1. `global`

- Equivalent to `window` in the browser.
    
- It’s the **global namespace object** in Node.js.
    
- Variables or functions attached to `global` can be accessed anywhere in your app.
    

Example:

```js
global.myVar = 123;

console.log(myVar); // 123
```

⚠️ Best practice: **Avoid polluting `global`** — it makes code harder to debug and maintain. Instead, use modules.

---

## ⚙️ 2. `process`

- A **built-in object** that gives info and control over the current Node.js process.
    
- Always available — no `require` needed. See [[Require]]
    

Common uses:

```js
console.log(process.pid); // Process ID
console.log(process.cwd()); // Current working directory
console.log(process.env.NODE_ENV); // Environment variables

process.on("exit", () => {
  console.log("Process is exiting");
});
```

Key things:

- `process.argv` → command-line arguments (`node app.js arg1 arg2`)
    
- `process.env` → environment variables (used for configs/secrets)
    
- `process.exit()` → force quit the app
    

---

## 📂 3. `__dirname`

- Returns the **absolute path** of the directory containing the currently executing file.
    

Example:  
If your file is at `/Users/tyson/project/app.js`:

```js
console.log(__dirname);
// /Users/tyson/project
```

Useful when working with file paths (`fs`, `path`).

---

## 📄 4. `__filename`

- Returns the **absolute path of the current file** (including the file name).
    

Example:

```js
console.log(__filename);
// /Users/tyson/project/app.js
```

---

## 📝 Key Difference: `process.cwd()` vs `__dirname`

- `process.cwd()` → current working directory where Node was started.
    
- `__dirname` → directory of the script file itself.
    

Example:

```bash
cd /Users/tyson
node project/app.js
```

Inside `app.js`:

- `process.cwd()` → `/Users/tyson`
    
- `__dirname` → `/Users/tyson/project`
    

---

✅ **In short:**

- `global` → global scope object (avoid using).
    
- `process` → info/control of Node process (env vars, args, lifecycle).
    
- `__dirname` → folder path of current file.
    
- `__filename` → full file path of current file.
    

---

Do you want me to show you **practical use cases** (like how `__dirname` + `path.join` are used when serving static files), or should we move on to the next Node core concept (e.g., Modules & `require`)?