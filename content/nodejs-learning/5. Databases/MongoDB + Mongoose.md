Now we’re diving into **MongoDB** (a NoSQL database) and **Mongoose** (an ODM for Node.js). This is a core part of building backend apps with Node.js for flexible, schema-based data handling. Let’s go step by step.

---

## 1️⃣ What is MongoDB?

- **NoSQL database** → stores data as **documents (JSON-like objects)** instead of tables.
    
- Flexible schema → fields can vary between documents.
    
- Great for **scalable, dynamic applications**.
    

Example document:

```json
{
  "_id": "64f72e4f12b34abc12345def",
  "name": "Tyson",
  "email": "tyson@example.com",
  "age": 30
}
```

- Collections = tables (e.g., `users`, `posts`)
    
- Documents = rows (individual entries)
    

---

## 2️⃣ Mongoose Overview

- **Mongoose** = ODM (Object Data Modeling) library for MongoDB.
    
- Provides **schemas, models, and validation** on top of MongoDB.
    
- Lets you interact with MongoDB using **JS objects** instead of raw queries.
    

---

## 3️⃣ Install MongoDB + Mongoose

```bash
npm install mongoose
```

Make sure you have a MongoDB instance running (local or cloud e.g., MongoDB Atlas).

---

## 4️⃣ Connecting to MongoDB

```js
const mongoose = require("mongoose");

mongoose.connect("mongodb://localhost:27017/myapp", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
  .then(() => console.log("MongoDB connected"))
  .catch((err) => console.error("DB connection error:", err));
```

- `useNewUrlParser` and `useUnifiedTopology` are recommended options.
    
- Connection returns a **Promise** → use `async/await` if preferred.
    

---

## 5️⃣ Defining a Schema & Model

```js
const { Schema, model } = mongoose;

// Define schema
const userSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, min: 0 },
  createdAt: { type: Date, default: Date.now }
});

// Create model
const User = model("User", userSchema);
```

- Schema = structure + validation rules
    
- Model = interface to query and manipulate data
    

---

## 6️⃣ CRUD Operations with Mongoose

### Create

```js
const newUser = await User.create({ name: "Tyson", email: "tyson@example.com", age: 30 });
console.log(newUser);
```

### Read

```js
const users = await User.find(); // all users
const singleUser = await User.findOne({ email: "tyson@example.com" });
```

### Update

```js
const updatedUser = await User.findByIdAndUpdate(newUser._id, { age: 31 }, { new: true });
```

### Delete

```js
await User.findByIdAndDelete(newUser._id);
```

---

## 7️⃣ Querying & Filtering

```js
// Filter + sort + limit
const result = await User.find({ age: { $gte: 18 } }) // age >= 18
  .sort({ name: 1 })   // ascending by name
  .limit(10);
```

- `$gte`, `$lte`, `$in`, `$ne` → MongoDB query operators
    

---

## 8️⃣ Relationships (References)

- **One-to-Many example:** Users → Posts
    

```js
const postSchema = new Schema({
  title: String,
  content: String,
  author: { type: Schema.Types.ObjectId, ref: "User" }
});

const Post = model("Post", postSchema);

// Populate author details
const posts = await Post.find().populate("author");
```

- `populate()` = fetch related documents
    

---

## ✅ Summary

|Concept|MongoDB|Mongoose|
|---|---|---|
|Type|NoSQL, document-oriented|ODM (Object Data Modeling) library|
|Data|Collections & Documents|Schema + Model abstraction|
|Queries|JSON-like|Chainable JS methods (`find`, `create`)|
|Validation|Optional|Built-in schema validation|
|Relationships|References & embedding|`populate()` for references|

**Key Takeaways:**

- MongoDB = flexible, scalable NoSQL database
    
- Mongoose = adds **schemas, validation, and easy queries**
    
- Combine with Node.js + Express for full REST APIs
    

---
