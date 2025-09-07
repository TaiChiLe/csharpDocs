The **`os` module** is another built-in Node.js module that gives you information about the system where your Node process is running. Super handy for logging, monitoring, or system-aware apps 🚀.

---

## 📂 1. Importing

```js
const os = require("os");
```

---

## 🛠️ 2. Common Methods & Properties

### System Info

```js
console.log(os.type());       // 'Linux', 'Darwin' (macOS), 'Windows_NT'
console.log(os.platform());   // 'linux', 'darwin', 'win32'
console.log(os.arch());       // 'x64', 'arm64'
console.log(os.release());    // OS version
```

---

### CPU Info

```js
console.log(os.cpus());
// Array of objects describing each logical CPU core
console.log(os.cpus().length); // number of cores
```

---

### Memory Info

```js
console.log(os.totalmem()); // Total system memory (bytes)
console.log(os.freemem());  // Free system memory (bytes)
```

👉 Often wrapped with `(value / 1024 / 1024 / 1024).toFixed(2)` to display in GB.

---

### User & Host Info

```js
console.log(os.userInfo());
// { uid: 501, username: 'tyson', homedir: '/Users/tyson', shell: '/bin/zsh' }

console.log(os.hostname()); // Machine name
```

---

### Network Info

```js
console.log(os.networkInterfaces());
// { eth0: [ { address, family, mac, internal, ... } ], ... }
```

---

### Paths

```js
console.log(os.homedir());  // Home directory path
console.log(os.tmpdir());   // Temp directory path
```

---

### System Uptime

```js
console.log(os.uptime()); // Seconds since system boot
```

---

## ✅ In Short

The `os` module helps you query:

- **System details** → type, platform, arch, release
    
- **CPU & memory** → `cpus()`, `totalmem()`, `freemem()`
    
- **User info** → `userInfo()`, `homedir()`
    
- **Network info** → `networkInterfaces()`
    
- **Uptime** → how long system has been running
    

---

👉 Want me to show you a **practical example** — like building a small Node.js script that logs system stats (CPU usage %, free memory, uptime) in a clean format — or should we continue straight to the **`http` module (servers)** next?