Package management is where Node.js projects really come alive 🚀. Let’s go through it step by step.

---

## 📦 1. Package Managers (NPM & Yarn)

### **NPM (Node Package Manager)**

- Default package manager that comes with Node.js.
    
- Lets you:
    
    - Install packages (`npm install express`)
        
    - Manage dependencies in `package.json`
        
    - Run scripts (`npm run start`)
        

### **Yarn**

- Alternative to npm (created by Facebook).
    
- Focused on speed, deterministic installs, and workspaces.
    
- Syntax is very similar:
    
    - `yarn add express` = `npm install express`
        
    - `yarn start` = `npm run start`
        

🔑 Nowadays, both npm (v7+) and Yarn are fast and reliable. Many teams pick one and stick to it.

---

## 📂 2. `package.json`

The **manifest file** for every Node project.  
It describes your project and its dependencies.

Example:

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "A simple Node.js app",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  }
}
```

### Key sections:

- `"name"`, `"version"` → metadata.
    
- `"main"` → entry point of your app.
    
- `"dependencies"` → packages needed at runtime.
    
- `"devDependencies"` → packages only for development (e.g., testing tools).
    
- `"scripts"` → custom commands.
    

---

## ⚙️ 3. Scripts

Defined in `package.json` under `"scripts"`.

Run them with:

```bash
npm run <script-name>
# or
yarn <script-name>
```

Example:

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js",
  "test": "jest",
  "lint": "eslint ."
}
```

Usage:

```bash
npm run dev
# Runs "nodemon index.js"
```

Special cases:

- `"start"` and `"test"` can be run without `run`:
    
    ```bash
    npm start
    npm test
    ```
    

---

## 🧩 4. Lock Files

- `package-lock.json` (npm) or `yarn.lock` (Yarn).
    
- Ensures consistent dependency versions across machines.
    
- Should always be committed to version control.
    

---

## ✅ In Short

- **NPM** is the default package manager; **Yarn** is an alternative with similar commands.
    
- `package.json` defines your app’s metadata, dependencies, and scripts.
    
- Scripts let you automate tasks (start, test, lint, build, etc.).
    
- Lock files ensure reproducible installs.
    

---

👉 Do you want me to go deeper into **how dependencies are resolved** (semver `^`, `~`, exact versions) and how `node_modules` works internally, or should we move to the next core Node.js concept like **Non-blocking I/O**?