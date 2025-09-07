Module systems are a **big milestone** in Node.js learning. Let’s carefully go through **CommonJS vs ES Modules (ESM)**.

---

## 📦 1. CommonJS (CJS)

- **Default** module system in Node.js (since the beginning).
    
- Uses `require` and `module.exports`.
    
- Synchronous loading (good for server-side, not browsers originally).
    

### Example: Export

```js
// utils.js
function add(a, b) {
  return a + b;
}

module.exports = { add };
```

### Example: Import

```js
// app.js
const { add } = require("./utils");
console.log(add(2, 3)); // 5
```

🔑 Key points:

- Each file is wrapped in a function `(function(exports, require, module, __filename, __dirname){})`.
    
- Modules are **cached** after first load.
    
- Still widely used in existing Node projects.
    

---

## 🌐 2. ES Modules (ESM)

- **Modern, standardized** JavaScript module system (used in browsers).
    
- Node.js added support starting from v12 (stable in v14+).
    
- Uses `import` and `export`.
    
- Asynchronous by design (works well for the browser).
    

### Example: Export

```js
// utils.mjs
export function add(a, b) {
  return a + b;
}
```

### Example: Import

```js
// app.mjs
import { add } from "./utils.mjs";
console.log(add(2, 3)); // 5
```

🔑 Key points:

- No wrapper function — top-level `import`/`export` are part of the language.
    
- Supports **static analysis** (tree-shaking, better tooling).
    
- Must either:
    
    - Use `.mjs` extension, **or**
        
    - Set `"type": "module"` in `package.json`.
        

---

## ⚖️ Comparison: CommonJS vs ES Modules

|Feature|CommonJS (CJS)|ES Modules (ESM)|
|---|---|---|
|Syntax|`require`, `module.exports`|`import`, `export`|
|Default in Node|Yes (legacy)|Yes (modern, via `"type": "module"`)|
|File Extension|`.js`|`.mjs` (or `.js` with `"type": "module"`)|
|Loading|Synchronous|Asynchronous|
|Exports|`module.exports` (single object)|Named & default exports|
|Scope|Wrapped in a function|Top-level|
|Browser Support|No (without bundler)|Yes (native in modern browsers)|

---

## 🧩 Interop Between CJS and ESM

Sometimes you need both in one project.

- Importing CommonJS in ESM:
    

```js
import pkg from "lodash"; // works, entire export as default
```

- Importing ESM in CommonJS:
    

```js
const { readFile } = await import("fs/promises"); // dynamic import
```

---

✅ **In short:**

- **CommonJS (`require/module.exports`)** → traditional Node.js, synchronous, still common.
    
- **ESM (`import/export`)** → modern, async, works in both browsers and Node.js, better for new projects.
    

---

Do you want me to show you a **practical migration example** (converting a small CommonJS project to ESM), or should we move on to the next Node.js core concept like **Non-blocking I/O**?