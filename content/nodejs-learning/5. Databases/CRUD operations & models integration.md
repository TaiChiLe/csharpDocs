Now we’re tying together **CRUD operations** with **models**, which is the backbone of most Node.js backend apps, whether you use **MongoDB/Mongoose** or **PostgreSQL/Sequelize**. I’ll break it down step by step with a **generic pattern** you can adapt to either SQL or NoSQL.

---

## 1️⃣ Folder Structure Recap

```
src/
├── routes/
│   └── userRoutes.js
├── controllers/
│   └── userController.js
├── models/
│   └── User.js
├── app.js
└── server.js
```

- `models/` → define your data structure
    
- `controllers/` → implement CRUD logic
    
- `routes/` → expose endpoints
    

---

## 2️⃣ CRUD Operations

### **Create (POST)**

- **Purpose:** Add a new record to the database
    

```js
// Controller: createUser
const createUser = async (req, res) => {
  try {
    const user = await User.create(req.body); // MongoDB/Mongoose
    // const user = await User.create(req.body); // Sequelize
    res.status(201).json(user);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
};
```

---

### **Read (GET)**

- **Purpose:** Fetch data from the database
    

```js
// Controller: getUsers
const getUsers = async (req, res) => {
  try {
    const users = await User.find(); // Mongoose
    // const users = await User.findAll(); // Sequelize
    res.json(users);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
};

// Controller: getUserById
const getUserById = async (req, res) => {
  try {
    const { id } = req.params;
    const user = await User.findById(id); // Mongoose
    // const user = await User.findByPk(id); // Sequelize
    if (!user) return res.status(404).json({ error: "User not found" });
    res.json(user);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
};
```

---

### **Update (PUT/PATCH)**

- **Purpose:** Modify an existing record
    

```js
const updateUser = async (req, res) => {
  try {
    const { id } = req.params;
    const user = await User.findByIdAndUpdate(id, req.body, { new: true }); // Mongoose
    // const user = await User.update(req.body, { where: { id }, returning: true }); // Sequelize
    if (!user) return res.status(404).json({ error: "User not found" });
    res.json(user);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
};
```

---

### **Delete (DELETE)**

- **Purpose:** Remove a record
    

```js
const deleteUser = async (req, res) => {
  try {
    const { id } = req.params;
    const user = await User.findByIdAndDelete(id); // Mongoose
    // const deleted = await User.destroy({ where: { id } }); // Sequelize
    if (!user) return res.status(404).json({ error: "User not found" });
    res.status(204).send(); // 204 = no content
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
};
```

---

## 3️⃣ Routes Integration

```js
const express = require("express");
const router = express.Router();
const userController = require("../controllers/userController");

router.get("/", userController.getUsers);
router.get("/:id", userController.getUserById);
router.post("/", userController.createUser);
router.put("/:id", userController.updateUser);
router.delete("/:id", userController.deleteUser);

module.exports = router;
```

---

## 4️⃣ Model Integration Example

### Mongoose Model (`User.js`)

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: Number
});

module.exports = mongoose.model("User", userSchema);
```

### Sequelize Model (`User.js`)

```js
const { DataTypes } = require("sequelize");
const sequelize = require("../config/db");

const User = sequelize.define("User", {
  name: { type: DataTypes.STRING, allowNull: false },
  email: { type: DataTypes.STRING, allowNull: false, unique: true },
  age: DataTypes.INTEGER
});

module.exports = User;
```

---

## 5️⃣ Error Handling & Response Patterns

- Use **try/catch** for async errors
    
- Return **consistent JSON format**:
    

```json
{
  "success": true,
  "data": {...},
  "error": null
}
```

- For errors:
    

```json
{
  "success": false,
  "data": null,
  "error": "User not found"
}
```

---

## ✅ Summary

- **CRUD** = Create, Read, Update, Delete
    
- **Controllers** = implement CRUD logic using models
    
- **Routes** = expose endpoints for clients
    
- **Models** = define structure & validation of data (Mongoose or Sequelize)
    
- **Error handling** = try/catch + consistent JSON response
    

---
