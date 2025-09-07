Now we’re moving from **Node.js core APIs** into how apps are usually **structured** in the real world. Node itself doesn’t enforce folder structure, but when building web apps (esp. with **Express**), a clean layout makes your code **scalable, testable, and maintainable**.

Here’s the standard breakdown:

---

## 📂 Typical Node.js Project Structure

```
project-root/
│
├── src/                  # Main application source code
│   ├── routes/           # Route definitions (Express routers)
│   ├── controllers/      # Request handlers (business logic)
│   ├── models/           # Database models (e.g., Mongoose, Sequelize)
│   ├── middlewares/      # Express middleware (auth, logging, validation)
│   ├── utils/            # Helper functions (e.g., token generation)
│   ├── config/           # Config files (db, env vars)
│   └── app.js            # App bootstrap (Express app setup)
│
├── tests/                # Unit/integration tests
├── node_modules/         # Installed dependencies
├── package.json          # Project metadata, scripts, dependencies
├── .env                  # Environment variables (gitignored)
└── server.js             # Entry point (starts Express server)
```

---

## 🛠️ 1. `src/routes/`

Defines API endpoints, usually mapped to controllers.

Example: `src/routes/userRoutes.js`

```js
const express = require("express");
const router = express.Router();
const userController = require("../controllers/userController");

router.get("/", userController.getAllUsers);
router.post("/", userController.createUser);

module.exports = router;
```

---

## 🛠️ 2. `src/controllers/`

Implements business logic.

Example: `src/controllers/userController.js`

```js
const User = require("../models/User");

exports.getAllUsers = async (req, res) => {
  const users = await User.find();
  res.json(users);
};

exports.createUser = async (req, res) => {
  const user = new User(req.body);
  await user.save();
  res.status(201).json(user);
};
```

---

## 🛠️ 3. `src/models/`

Database layer (often with an ORM like **Mongoose** for MongoDB or **Sequelize** for SQL).

Example: `src/models/User.js`

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: String,
  email: { type: String, unique: true },
  password: String
});

module.exports = mongoose.model("User", userSchema);
```

---

## 🛠️ 4. `src/middlewares/`

Middleware functions → run **before or after** route handlers.

Example: `src/middlewares/authMiddleware.js`

```js
module.exports = (req, res, next) => {
  const token = req.headers["authorization"];
  if (!token) return res.status(401).json({ error: "Unauthorized" });

  // validate token...
  next();
};
```

---

## 🛠️ 5. `src/app.js`

Sets up the Express app.

```js
const express = require("express");
const userRoutes = require("./routes/userRoutes");

const app = express();

app.use(express.json());
app.use("/users", userRoutes);

module.exports = app;
```

---

## 🛠️ 6. `server.js`

Entry point that starts the server.

```js
const app = require("./src/app");

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

## ✅ In Short

- **`routes/`** → define endpoints.
    
- **`controllers/`** → handle request/response logic.
    
- **`models/`** → interact with the database.
    
- **`middlewares/`** → reusable request-processing functions.
    
- **`src/`** → keeps app code separate from config and scripts.
    

---
