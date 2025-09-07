The **`crypto` module** is Node.js’s built-in toolkit for **hashing, encryption, signing, and verifying data**. It’s widely used for passwords, tokens, and secure communication. Let’s break it down:

---

## 📂 1. Importing

```js
const crypto = require("crypto");
```

---

## 🔑 2. Hashing

Hashing = one-way transformation of data → fixed length output.  
Commonly used for passwords, checksums, integrity checks.

### Example: SHA256 hash

```js
const hash = crypto.createHash("sha256")
  .update("mysecretpassword")
  .digest("hex");

console.log("SHA256:", hash);
```

👉 Output is always the same for the same input.  
👉 Cannot be reversed.

---

## 🧂 3. HMAC (Hash-based Message Authentication Code)

Hash + secret key → ensures **integrity** + **authenticity**.

```js
const hmac = crypto.createHmac("sha256", "supersecretkey")
  .update("some message")
  .digest("hex");

console.log("HMAC:", hmac);
```

---

## 🔒 4. Symmetric Encryption (AES)

Encrypt + decrypt with the **same secret key**.

```js
const algorithm = "aes-256-cbc";
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

const cipher = crypto.createCipheriv(algorithm, key, iv);
let encrypted = cipher.update("Hello, world!", "utf8", "hex");
encrypted += cipher.final("hex");

console.log("Encrypted:", encrypted);

// Decrypt
const decipher = crypto.createDecipheriv(algorithm, key, iv);
let decrypted = decipher.update(encrypted, "hex", "utf8");
decrypted += decipher.final("utf8");

console.log("Decrypted:", decrypted);
```

👉 Good for **storing sensitive data at rest**.

---

## 🔑 5. Asymmetric Encryption (RSA)

Encrypt/decrypt with a **public/private key pair**.

```js
const { generateKeyPairSync, publicEncrypt, privateDecrypt } = crypto;

// Generate keys
const { publicKey, privateKey } = generateKeyPairSync("rsa", {
  modulusLength: 2048,
});

// Encrypt with public key
const encryptedData = publicEncrypt(
  publicKey,
  Buffer.from("Secret message")
);

// Decrypt with private key
const decryptedData = privateDecrypt(
  privateKey,
  encryptedData
);

console.log("Decrypted:", decryptedData.toString());
```

👉 Used in **TLS/HTTPS, JWT signing, digital signatures**.

---

## 📝 6. Password Hashing (PBKDF2 / bcrypt / scrypt)

Passwords should never be stored raw. Use **slow, salted hashes**.

### Example with PBKDF2

```js
crypto.pbkdf2("mypassword", "mysalt", 100000, 64, "sha512", (err, derivedKey) => {
  if (err) throw err;
  console.log("Password hash:", derivedKey.toString("hex"));
});
```

👉 In practice, people often use libraries like **bcrypt** or **argon2** (friendlier than raw `crypto`).

---

## ✅ In Short

- **Hashing** → one-way (SHA256, SHA512, etc.).
    
- **HMAC** → hash + secret for authentication.
    
- **Symmetric encryption** → AES (same key to encrypt/decrypt).
    
- **Asymmetric encryption** → RSA/ECC (public/private keys).
    
- **Password hashing** → use PBKDF2, bcrypt, or argon2 (not plain SHA).
    

---