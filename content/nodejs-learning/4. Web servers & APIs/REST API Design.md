Now we’re moving from **Express basics** to **REST API design**, which is how most Node.js backend apps structure their endpoints. Let’s go step by step.

---

## 🌐 1. What is a REST API?

**REST (Representational State Transfer)** is an architectural style for web services.  
Key principles:

1. **Stateless**: Each request contains all info to process it.
    
2. **Resource-based**: Endpoints represent resources (e.g., users, posts).
    
3. **HTTP methods**: Use standard verbs to act on resources:
    
    - `GET` → retrieve data
        
    - `POST` → create a resource
        
    - `PUT` / `PATCH` → update a resource
        
    - `DELETE` → remove a resource
        
4. **JSON**: Responses are usually in JSON format.
    

---

## 🛠️ 2. REST API Folder Structure

Common structure with **Express**:

```
src/
├── routes/
│   └── userRoutes.js
├── controllers/
│   └── userController.js
├── models/
│   └── User.js
├── middlewares/
│   └── authMiddleware.js
├── app.js
└── server.js
```

- `routes/` → define endpoints
    
- `controllers/` → business logic
    
- `models/` → database schema
    
- `middlewares/` → auth, validation, error handling
    

---

## 📄 3. CRUD Routes Example

### Model Example (`User.js`)

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: String,
  email: String
});

module.exports = mongoose.model("User", userSchema);
```

### Controller Example (`userController.js`)

```js
const User = require("../models/User");

exports.getUsers = async (req, res) => {
  const users = await User.find();
  res.json(users);
};

exports.createUser = async (req, res) => {
  const user = new User(req.body);
  await user.save();
  res.status(201).json(user);
};

exports.updateUser = async (req, res) => {
  const { id } = req.params;
  const user = await User.findByIdAndUpdate(id, req.body, { new: true });
  res.json(user);
};

exports.deleteUser = async (req, res) => {
  const { id } = req.params;
  await User.findByIdAndDelete(id);
  res.status(204).send();
};
```

### Routes Example (`userRoutes.js`)

```js
const express = require("express");
const router = express.Router();
const userController = require("../controllers/userController");

router.get("/", userController.getUsers);
router.post("/", userController.createUser);
router.put("/:id", userController.updateUser);
router.delete("/:id", userController.deleteUser);

module.exports = router;
```

### App Setup (`app.js`)

```js
const express = require("express");
const userRoutes = require("./routes/userRoutes");

const app = express();
app.use(express.json());
app.use("/users", userRoutes);

module.exports = app;
```

---

## ⚡ 4. Query Parameters & Filtering

REST APIs often support **filtering, sorting, pagination** via query params:

```js
// GET /users?limit=10&sort=name
exports.getUsers = async (req, res) => {
  const { limit = 10, sort = "name" } = req.query;
  const users = await User.find().limit(Number(limit)).sort(sort);
  res.json(users);
};
```

---

## 🔐 5. Authentication & Authorization

- **JWT tokens** are commonly used:
    
    - Middleware checks `Authorization` header
        
    - Verifies token
        
    - Adds user info to `req.user`
        

Example middleware:

```js
const jwt = require("jsonwebtoken");

module.exports = (req, res, next) => {
  const token = req.headers["authorization"];
  if (!token) return res.status(401).json({ error: "Unauthorized" });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ error: "Invalid token" });
  }
};
```

---

## ✅ 6. REST API Best Practices

1. Use **plural resource names** (`/users`, `/posts`).
    
2. Use **HTTP status codes** correctly (200, 201, 204, 400, 404, 500).
    
3. Keep **consistent JSON response format**:
    

```json
{
  "success": true,
  "data": {...},
  "error": null
}
```

4. Validate **request body** using libraries like `Joi` or `express-validator`.
    
5. Version your API: `/api/v1/users`.
    

---

