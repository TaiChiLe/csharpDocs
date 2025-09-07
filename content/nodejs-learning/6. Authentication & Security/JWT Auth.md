JWT (JSON Web Token) authentication is **the standard for stateless authentication** in modern Node.js apps. Let’s go **step by step**, covering the full flow.

---

## 1️⃣ What is JWT?

- **JWT** = JSON Web Token
    
- Used to **authenticate users without storing sessions on the server**
    
- Composed of **three parts**:
    

```
header.payload.signature
```

1. **Header** → algorithm & token type
    
2. **Payload** → user info, claims (e.g., `id`, `role`)
    
3. **Signature** → HMAC/SHA256 hash to verify token integrity
    

---

## 2️⃣ Install Dependencies

```bash
npm install jsonwebtoken bcryptjs
```

- `jsonwebtoken` → create & verify JWTs
    
- `bcryptjs` → hash passwords
    

---

## 3️⃣ User Registration (Hash Password)

```js
const bcrypt = require("bcryptjs");
const User = require("../models/User");

const register = async (req, res) => {
  try {
    const { name, email, password } = req.body;

    // Hash password
    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(password, salt);

    const user = await User.create({ name, email, password: hashedPassword });
    res.status(201).json({ message: "User created", userId: user._id });
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
};
```

---

## 4️⃣ User Login (Generate JWT)

```js
const jwt = require("jsonwebtoken");

const login = async (req, res) => {
  try {
    const { email, password } = req.body;
    const user = await User.findOne({ email });
    if (!user) return res.status(400).json({ error: "Invalid credentials" });

    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(400).json({ error: "Invalid credentials" });

    // Generate JWT
    const token = jwt.sign(
      { userId: user._id, email: user.email },
      process.env.JWT_SECRET,
      { expiresIn: "1h" }
    );

    res.json({ token });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
};
```

---

## 5️⃣ Protect Routes (JWT Middleware)

```js
const jwt = require("jsonwebtoken");

const authMiddleware = (req, res, next) => {
  const authHeader = req.headers["authorization"];
  if (!authHeader) return res.status(401).json({ error: "No token provided" });

  const token = authHeader.split(" ")[1]; // Bearer <token>
  if (!token) return res.status(401).json({ error: "No token provided" });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded; // add user info to request
    next();
  } catch (err) {
    res.status(401).json({ error: "Invalid or expired token" });
  }
};
```

---

## 6️⃣ Example Protected Route

```js
const express = require("express");
const router = express.Router();
const authMiddleware = require("../middlewares/authMiddleware");

router.get("/profile", authMiddleware, (req, res) => {
  res.json({ message: "Welcome!", user: req.user });
});

module.exports = router;
```

- Only users with a **valid JWT** can access `/profile`.
    

---

## 7️⃣ Best Practices

1. **Store secret securely** → `.env` file (`JWT_SECRET`).
    
2. **Short expiration time** → e.g., 1 hour, refresh token if needed.
    
3. **Send token in `Authorization: Bearer <token>` header**.
    
4. **Hash passwords** before saving in DB.
    
5. Optionally, **blacklist/revoke tokens** on logout or password change.
    

---

## ✅ Summary

- JWT = stateless, compact authentication token
    
- **Register** → hash password
    
- **Login** → verify credentials → generate JWT
    
- **Middleware** → verify JWT → protect routes
    
- **Client** → sends JWT in `Authorization` header
    

---
