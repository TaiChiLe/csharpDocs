
## 🌍 1. JavaScript in the Browser

When you run JavaScript inside a browser (Chrome, Firefox, Safari, etc.):

- **Purpose:**  
    Mostly for client-side (front-end) tasks like manipulating the DOM, handling events, animations, and making network requests.
    
- **Available APIs:**
    
    - `document`, `window`, `navigator`, `localStorage`, `fetch`, etc.
        
    - These are provided by the **browser environment**, not JavaScript itself.
        
- **Execution:**  
    JavaScript code is executed by the browser’s **JavaScript engine** (e.g., V8 in Chrome).  
    But it runs inside a sandboxed environment with strict security (can’t directly touch the file system or OS).
    

---

## ⚙️ 2. Node.js Runtime Environment

When you run JavaScript with **Node.js**:

- **Purpose:**  
    Built for server-side tasks like building APIs, reading/writing files, networking, process management.
    
- **Available APIs:**
    
    - `fs` (file system), `http`, `os`, `path`, `process`, etc.
        
    - These come from **Node.js**, not from the language or browser.
        
- **Execution:**  
    Still uses the **V8 engine** (like Chrome), but Node.js adds bindings to C++ libraries for low-level operations.
    
- **No Browser APIs:**  
    You don’t get `document`, `window`, or `fetch` (until recently — Node 18+ added a WHATWG `fetch` implementation).
    

---

## 📊 Key Differences (Browser vs Node.js)

|Aspect|Browser JS|Node.js|
|---|---|---|
|**Environment**|Runs inside a browser|Runs on the OS|
|**APIs Available**|DOM, BOM, Web APIs (`fetch`, `localStorage`)|Core modules (`fs`, `http`, `os`, `process`)|
|**Security**|Sandboxed (no direct FS/OS access)|Full OS & network access|
|**Global Object**|`window`|`global`|
|**Use Case**|UI interactivity, client-side logic|Backend servers, tooling, scripts|

---

✅ **In short:**

- **Browser JS** = limited sandbox, DOM-focused, client-side.
    
- **Node.js** = full runtime environment with extra APIs for server-side & system-level tasks.
    
