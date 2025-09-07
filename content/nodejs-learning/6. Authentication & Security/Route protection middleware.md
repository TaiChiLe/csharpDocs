Now we’re going into **route protection middleware**, which is essential for **controlling access** in Node.js/Express apps. This builds directly on JWT or session-based authentication.

---

## 1️⃣ What is Route Protection Middleware?

- Middleware that **checks if a user is authenticated** (and optionally authorized) before allowing access to certain routes.
    
- Prevents unauthorized users from accessing protected resources.
    
- Can be used with **JWT, OAuth, or session-based auth**.
    

---

## 2️⃣ JWT-Based Route Protection

### Middleware Example

```js
const jwt = require("jsonwebtoken");

const authMiddleware = (req, res, next) => {
  const authHeader = req.headers["authorization"];
  if (!authHeader) return res.status(401).json({ error: "No token provided" });

  const token = authHeader.split(" ")[1]; // Bearer <token>
  if (!token) return res.status(401).json({ error: "No token provided" });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded; // attach user info to request
    next();
  } catch (err) {
    res.status(401).json({ error: "Invalid or expired token" });
  }
};

module.exports = authMiddleware;
```

---

## 3️⃣ Applying Middleware to Routes

```js
const express = require("express");
const router = express.Router();
const authMiddleware = require("../middlewares/authMiddleware");

router.get("/profile", authMiddleware, (req, res) => {
  res.json({ message: "Welcome to your profile", user: req.user });
});

router.get("/settings", authMiddleware, (req, res) => {
  res.json({ message: "Settings page", user: req.user });
});
```

- Only users with a **valid JWT** can access `/profile` or `/settings`.
    

---

## 4️⃣ Role-Based Authorization (Optional)

You can extend the middleware to check **user roles**:

```js
const roleMiddleware = (requiredRole) => {
  return (req, res, next) => {
    if (!req.user) return res.status(401).json({ error: "Unauthorized" });
    if (req.user.role !== requiredRole)
      return res.status(403).json({ error: "Forbidden" });
    next();
  };
};

// Usage
router.get("/admin", authMiddleware, roleMiddleware("admin"), (req, res) => {
  res.json({ message: "Welcome Admin!" });
});
```

---

## 5️⃣ Express Best Practices

1. **Apply middleware selectively** → only protect routes that need it
    
2. **Attach user info** → `req.user` makes it easy for downstream handlers
    
3. **Consistent error handling** → return JSON with `status` and `error`
    
4. **Combine with other middleware** → e.g., logging, validation, rate limiting
    

---

## ✅ Summary

- Route protection middleware **guards routes** from unauthorized access.
    
- **JWT-based middleware** → check token validity, attach user info.
    
- **Role-based middleware** → restrict certain resources to specific users.
    
- Works in combination with **bcrypt, JWT, and OAuth** authentication.
    

---
