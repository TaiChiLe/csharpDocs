The **`events` module** and the **EventEmitter pattern** are central to Node.js’ event-driven architecture. Let’s dig in.

---

## 📂 1. Importing

```js
const EventEmitter = require("events");
```

---

## ⚡ 2. Creating an EventEmitter

```js
const EventEmitter = require("events");

const emitter = new EventEmitter();

// Register a listener
emitter.on("greet", (name) => {
  console.log(`Hello, ${name}!`);
});

// Emit (trigger) the event
emitter.emit("greet", "Tyson");
```

👉 Output:

```
Hello, Tyson!
```

- `on(event, listener)` → listen for an event.
    
- `emit(event, ...args)` → trigger the event with optional arguments.
    

---

## 🔄 3. Multiple Listeners

```js
emitter.on("data", (msg) => console.log("Listener 1:", msg));
emitter.on("data", (msg) => console.log("Listener 2:", msg));

emitter.emit("data", "Hello");
// Listener 1: Hello
// Listener 2: Hello
```

---

## 🧹 4. One-time Listeners

```js
emitter.once("init", () => {
  console.log("This runs only once!");
});

emitter.emit("init"); // Runs
emitter.emit("init"); // Ignored
```

---

## ❌ 5. Removing Listeners

```js
function logMessage(msg) {
  console.log("Message:", msg);
}

emitter.on("message", logMessage);
emitter.emit("message", "First");  // Message: First

emitter.removeListener("message", logMessage);
emitter.emit("message", "Second"); // (nothing happens)
```

---

## 📊 6. Inheritance Pattern

Most Node.js core modules (like `http.Server`, `fs.ReadStream`) are **EventEmitters**.  
You can extend `EventEmitter` in your own classes:

```js
class MyServer extends EventEmitter {
  start() {
    console.log("Server started...");
    this.emit("ready");
  }
}

const server = new MyServer();

server.on("ready", () => console.log("Server is ready!"));

server.start();
```

---

## ⚠️ 7. Best Practices

- Always remove listeners if they’re no longer needed (`removeListener` or `removeAllListeners`).
    
- Watch out for memory leaks: Node warns if you add **>10 listeners** to one event.
    
- Prefer `.once()` for initialization/setup events.
    

---

## ✅ In Short

- `EventEmitter` lets you **decouple logic** using events and listeners.
    
- Core Node APIs (HTTP, Streams, etc.) are built on this pattern.
    
- You can use it to build your own async event-driven modules.
    

---

