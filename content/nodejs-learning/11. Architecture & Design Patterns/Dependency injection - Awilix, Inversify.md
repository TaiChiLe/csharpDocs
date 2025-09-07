Perfect — let’s go through **Dependency Injection (DI) in Node.js**, including **Awilix** and **Inversify**, which are essential for **decoupling components and improving testability**.

---

## **1️⃣ What is Dependency Injection?**

- **Definition:** A design pattern where an object receives its dependencies from an **external source** rather than creating them itself.
    
- **Purpose:**
    
    - Reduces **tight coupling**
        
    - Improves **testability**
        
    - Enhances **modularity**
        

### Without DI

```js
class UserService {
  constructor() {
    this.userRepo = new UserRepository(); // tightly coupled
  }
}
```

### With DI

```js
class UserService {
  constructor(userRepo) {
    this.userRepo = userRepo; // injected dependency
  }
}
```

---

## **2️⃣ Awilix**

- **Awilix** = lightweight dependency injection container for Node.js
    
- Supports **class, function, and value registration**
    
- Integrates easily with **Express**
    

### Install

```bash
npm install awilix awilix-express
```

### Example

```js
const { createContainer, asClass, asValue } = require("awilix");
const { scopePerRequest } = require("awilix-express");
const express = require("express");

const app = express();

// Services
class UserService {
  constructor({ userRepo }) { this.userRepo = userRepo; }
}

// Repositories
class UserRepository {
  getAll() { return ["Alice", "Bob"]; }
}

// Container setup
const container = createContainer();
container.register({
  userService: asClass(UserService).scoped(),
  userRepo: asClass(UserRepository).scoped()
});

// Middleware
app.use(scopePerRequest(container));

// Route using injected service
app.get("/users", (req, res) => {
  const users = req.scope.resolve("userService").userRepo.getAll();
  res.json(users);
});

app.listen(3000);
```

- **`.scoped()`** → new instance per request
    
- **`.singleton()`** → one shared instance
    

---

## **3️⃣ InversifyJS**

- **InversifyJS** = TypeScript-first DI container for Node.js
    
- Uses **decorators** and **interfaces** to define injections
    

### Install

```bash
npm install inversify reflect-metadata
```

### Example

```ts
import "reflect-metadata";
import { injectable, inject, Container } from "inversify";

@injectable()
class UserRepository {
  getAll() { return ["Alice", "Bob"]; }
}

@injectable()
class UserService {
  constructor(@inject(UserRepository) private userRepo: UserRepository) {}
  listUsers() { return this.userRepo.getAll(); }
}

const container = new Container();
container.bind(UserRepository).toSelf();
container.bind(UserService).toSelf();

const service = container.get(UserService);
console.log(service.listUsers()); // ["Alice", "Bob"]
```

- **Benefits:**
    
    - Type-safe
        
    - Great for **large-scale TypeScript apps**
        
    - Supports **interfaces and decorators**
        

---

## **4️⃣ Benefits of Using DI**

1. **Decoupling:** Classes don’t instantiate dependencies themselves
    
2. **Testability:** Easy to mock dependencies in tests
    
3. **Flexibility:** Swap implementations without changing the consumer
    
4. **Scalability:** Useful in large applications with many services
    

---

## **5️⃣ Key Takeaways**

|Feature|Awilix|Inversify|
|---|---|---|
|Language|JS/TS|TypeScript-first|
|Decorators|No|Yes|
|Scope|Scoped/Singleton|Scoped/Singleton|
|Express Integration|Easy with `awilix-express`|Manual injection|
|Best Use Case|Medium-to-large JS apps|Large-scale TS apps|

---

Next, we could cover **Logging & Monitoring in Node.js** for production apps, including **Winston, Morgan, and APM tools**.

Do you want to go there next?