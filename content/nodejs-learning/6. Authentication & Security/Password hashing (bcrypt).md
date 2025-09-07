Password hashing is **essential for security** in any Node.js app. Let’s go **step by step** and cover why, how, and best practices using **bcrypt**.

---

## 1️⃣ Why Hash Passwords?

- **Never store plain-text passwords** — if your DB is breached, users’ credentials are exposed.
    
- Hashing converts the password into a **fixed-length, irreversible string**.
    
- **Salting** adds randomness, making hash attacks much harder.
    

---

## 2️⃣ Install `bcrypt`

```bash
npm install bcryptjs
```

- `bcryptjs` → pure JS implementation (works everywhere)
    
- Alternatively, `bcrypt` → faster native bindings
    

---

## 3️⃣ Hashing a Password

```js
const bcrypt = require("bcryptjs");

const hashPassword = async (plainPassword) => {
  const salt = await bcrypt.genSalt(10);      // generate salt (complexity = 10)
  const hashedPassword = await bcrypt.hash(plainPassword, salt);
  return hashedPassword;
};

// Example usage
(async () => {
  const password = "superSecret123";
  const hashed = await hashPassword(password);
  console.log(hashed); // $2a$10$...
})();
```

---

## 4️⃣ Verifying Passwords

When a user logs in, compare the **plain password** with the **hashed password** in DB:

```js
const comparePassword = async (plainPassword, hashedPassword) => {
  const isMatch = await bcrypt.compare(plainPassword, hashedPassword);
  return isMatch;
};

// Example
const isValid = await comparePassword("superSecret123", hashedFromDB);
console.log(isValid); // true or false
```

---

## 5️⃣ Integrating with User Registration/Login

### Registration (hash password before saving)

```js
const register = async (req, res) => {
  try {
    const { name, email, password } = req.body;
    const hashedPassword = await bcrypt.hash(password, 10);
    const user = await User.create({ name, email, password: hashedPassword });
    res.status(201).json({ message: "User created", userId: user._id });
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
};
```

### Login (verify password)

```js
const login = async (req, res) => {
  try {
    const { email, password } = req.body;
    const user = await User.findOne({ email });
    if (!user) return res.status(400).json({ error: "Invalid credentials" });

    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(400).json({ error: "Invalid credentials" });

    res.json({ message: "Login successful" });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
};
```

---

## 6️⃣ Best Practices

1. **Never store plain-text passwords**
    
2. **Use a strong salt** → `bcrypt.genSalt(10)` is standard
    
3. **Do not log hashed passwords**
    
4. **Use async functions** → avoid blocking the event loop
    
5. **Set strong password policies** → min length, complexity
    

---

✅ **Summary**

- `bcrypt` = secure hashing for passwords
    
- **Hash** passwords before saving, **compare** during login
    
- Protects user data even if DB is compromised
    
- Works well with **JWT, OAuth, or any authentication system**
    

---

Next, we can go through **refresh tokens + token rotation** to handle long-lived sessions securely alongside JWT and bcrypt authentication.

Do you want to go there next?