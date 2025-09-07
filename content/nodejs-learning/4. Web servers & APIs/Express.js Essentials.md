**Express.js** is the most widely used Node.js framework for building APIs and web apps. Let’s go **step by step** through the essentials.

---

## 📦 1. Setting Up Express

```bash
npm install express
```

```js
const express = require("express");
const app = express();

app.use(express.json()); // Middleware to parse JSON bodies

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

## 🌐 2. Routing

Express routes define how your app responds to HTTP requests.

```js
// GET route
app.get("/", (req, res) => {
  res.send("Home Page");
});

// POST route
app.post("/users", (req, res) => {
  const user = req.body;
  res.status(201).json({ message: "User created", user });
});

// PUT route
app.put("/users/:id", (req, res) => {
  const { id } = req.params;
  res.send(`Updating user with ID: ${id}`);
});

// DELETE route
app.delete("/users/:id", (req, res) => {
  const { id } = req.params;
  res.send(`Deleting user with ID: ${id}`);
});
```

---

## ⚙️ 3. Middleware

Middleware functions run **before route handlers** and can modify `req`, `res`, or handle errors.

### Example: Logging Middleware

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // Must call next() to continue
});
```

### Example: Custom Middleware for Auth

```js
const authMiddleware = (req, res, next) => {
  const token = req.headers["authorization"];
  if (!token) return res.status(401).json({ error: "Unauthorized" });
  next();
};

app.use("/users", authMiddleware);
```

---

## 📄 4. `req` & `res` Objects

- **`req` (request):** info about client request
    
    - `req.body` → JSON or form data
        
    - `req.params` → URL parameters
        
    - `req.query` → query string parameters
        
    - `req.headers` → HTTP headers
        
- **`res` (response):** sending data back
    
    - `res.send()` → send text/html
        
    - `res.json()` → send JSON
        
    - `res.status()` → set HTTP status
        
    - `res.redirect()` → redirect client
        

---

## 🔗 5. Query Params vs URL Params

### URL Params

Defined in the route:

```js
app.get("/users/:id", (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});
```

Request: `/users/123` → `req.params.id = '123'`

### Query Params

Passed in the URL after `?`:

```js
app.get("/search", (req, res) => {
  res.send(`Search query: ${req.query.q}`);
});
```

Request: `/search?q=nodejs` → `req.query.q = 'nodejs'`

---

## ❌ 6. Error Handling

Express has **special middleware for errors**: `(err, req, res, next)`

```js
// Route that throws an error
app.get("/error", (req, res) => {
  throw new Error("Something went wrong!");
});

// Error-handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: err.message });
});
```

- Errors can also be passed with `next(err)`:
    

```js
app.get("/fail", (req, res, next) => {
  next(new Error("Failed request"));
});
```

---

## ✅ Summary

- **Routing:** map HTTP methods + paths to handlers.
    
- **Middleware:** reusable functions that run before or after routes.
    
- **`req` & `res`:** request info and response methods.
    
- **Params:** URL params (`req.params`) vs query params (`req.query`).
    
- **Error handling:** use error-handling middleware `(err, req, res, next)` for clean centralized error responses.
    

---

