Environment variables are a **must-have** in Node.js apps to keep sensitive info (API keys, DB credentials) and environment-specific settings out of your codebase. Let’s go step by step.

---

## 📂 1. Install `dotenv`

```bash
npm install dotenv
```

---

## 📖 2. Create a `.env` file

This lives in your project root (and should be **gitignored**):

```
PORT=4000
MONGO_URI=mongodb://localhost:27017/mydb
JWT_SECRET=supersecretkey
NODE_ENV=development
```

---

## ⚡ 3. Load `.env` in Node.js

In your entry point (`server.js` or `app.js`):

```js
require("dotenv").config();

console.log("PORT is:", process.env.PORT);
console.log("Environment:", process.env.NODE_ENV);
```

---

## 🛠️ 4. Using Variables in Your App

Example with Express and MongoDB:

```js
const express = require("express");
const mongoose = require("mongoose");
require("dotenv").config();

const app = express();

// Connect to MongoDB
mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log("DB connected"))
  .catch(err => console.error("DB error:", err));

app.listen(process.env.PORT, () => {
  console.log(`Server running on port ${process.env.PORT}`);
});
```

---

## 🔐 5. Security & Best Practices

- **Never commit `.env` files** → add `.env` to `.gitignore`.
    
- Use **`.env.example`** to show required vars without secrets:
    
    ```
    PORT=
    MONGO_URI=
    JWT_SECRET=
    NODE_ENV=
    ```
    
- Keep different `.env` files per environment:
    
    - `.env.development`
        
    - `.env.production`
        

👉 Then load the right one based on `NODE_ENV`.

---

## ✅ In Short

- Use `dotenv` to load `.env` files into `process.env`.
    
- Store **secrets and environment-specific configs** there.
    
- Never commit real `.env` files to git — use `.env.example`.
    
- Access vars via `process.env.KEY`.
    

---

