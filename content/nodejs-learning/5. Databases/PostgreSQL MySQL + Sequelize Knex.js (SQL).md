Now let’s cover **SQL databases** (PostgreSQL/MySQL) and **Node.js ORMs/query builders** like **Sequelize** and **Knex.js**. This is the **relational alternative** to MongoDB + Mongoose.

---

## 1️⃣ SQL vs NoSQL Recap

|Feature|SQL (PostgreSQL/MySQL)|NoSQL (MongoDB)|
|---|---|---|
|Data model|Tables (rows & columns)|Collections (documents)|
|Schema|Fixed|Flexible|
|Relationships|JOINs, foreign keys|References or embedding|
|Transactions|Strong support (ACID)|Limited ACID, flexible|
|Query language|SQL|MongoDB queries|

---

## 2️⃣ Sequelize (ORM for SQL)

- **Sequelize** = full-featured ORM for Node.js.
    
- Works with PostgreSQL, MySQL, SQLite, MSSQL.
    
- Provides **models, associations, migrations**, and query building in **JS syntax**.
    

### Install Sequelize + Postgres

```bash
npm install sequelize pg pg-hstore
```

### Setup Sequelize

```js
const { Sequelize, DataTypes } = require("sequelize");

const sequelize = new Sequelize("mydb", "username", "password", {
  host: "localhost",
  dialect: "postgres",
});

sequelize.authenticate()
  .then(() => console.log("DB connected"))
  .catch((err) => console.error("DB connection error:", err));
```

### Define Models

```js
const User = sequelize.define("User", {
  name: { type: DataTypes.STRING, allowNull: false },
  email: { type: DataTypes.STRING, allowNull: false, unique: true }
});

const Post = sequelize.define("Post", {
  title: { type: DataTypes.STRING, allowNull: false },
  content: DataTypes.TEXT
});

// Associations
User.hasMany(Post);
Post.belongsTo(User);

(async () => {
  await sequelize.sync({ force: true }); // create tables
})();
```

### CRUD Example

```js
// Create
const user = await User.create({ name: "Tyson", email: "tyson@example.com" });

// Read
const users = await User.findAll();
const singleUser = await User.findOne({ where: { email: "tyson@example.com" } });

// Update
await user.update({ name: "Tyson L." });

// Delete
await user.destroy();
```

---

## 3️⃣ Knex.js (Query Builder)

- **Knex.js** = flexible **SQL query builder** for Node.js.
    
- Lightweight, doesn’t enforce models — great if you want **raw SQL flexibility with JS syntax**.
    
- Works with PostgreSQL, MySQL, SQLite, and more.
    

### Install Knex + Postgres

```bash
npm install knex pg
```

### Initialize Knex

```js
const knex = require("knex")({
  client: "pg",
  connection: {
    host: "127.0.0.1",
    user: "username",
    password: "password",
    database: "mydb"
  }
});
```

### Example Queries

```js
// Create table
await knex.schema.createTable("users", (table) => {
  table.increments("id").primary();
  table.string("name").notNullable();
  table.string("email").unique().notNullable();
});

// Insert
await knex("users").insert({ name: "Tyson", email: "tyson@example.com" });

// Select
const users = await knex("users").select("*");

// Update
await knex("users").where({ id: 1 }).update({ name: "Tyson L." });

// Delete
await knex("users").where({ id: 1 }).del();
```

---

## 4️⃣ Key Differences: Sequelize vs Knex

|Feature|Sequelize|Knex.js|
|---|---|---|
|Abstraction level|High (models, ORM, associations)|Low (query builder only)|
|Migrations|Built-in|Supported via knex CLI|
|Learning curve|Slightly higher|Lower (closer to raw SQL)|
|Flexibility|Less raw SQL freedom|Full SQL control|
|Best use case|Full-featured apps, ORM needed|Lightweight apps, raw SQL|

---

## ✅ Summary

- **PostgreSQL/MySQL** = relational databases with strong schemas & relationships.
    
- **Sequelize** = ORM → JS models, associations, validations, migrations.
    
- **Knex.js** = query builder → flexible, closer to raw SQL.
    
- Choose **Sequelize** if you want an ORM experience like Mongoose.
    
- Choose **Knex** if you want a lightweight, SQL-centric approach.
    

---
