Let’s do a **deep dive into EventEmitter and event-driven design** in Node.js. This is **core to Node’s asynchronous architecture**.

---

## 1️⃣ What is Event-Driven Design?

- **Event-driven programming** = design pattern where code **reacts to events** rather than executing sequentially.
    
- Instead of directly calling a function, you **emit events** and **listen for them**.
    
- Enables **decoupling**, **asynchronous handling**, and **non-blocking I/O**.
    

> Node.js itself is heavily event-driven (think: HTTP requests, streams, timers, I/O callbacks).

---

## 2️⃣ EventEmitter Basics

- `EventEmitter` is a **core Node.js class** (`require('events')`)
    
- Allows **registering listeners** and **emitting events**
    

### Import and Create Emitter

```js
const EventEmitter = require("events");
const myEmitter = new EventEmitter();
```

---

### Listening to an Event

```js
myEmitter.on("greet", (name) => {
  console.log(`Hello, ${name}!`);
});
```

- `.on(eventName, listener)` → register listener for an event
    
- Multiple listeners can listen to the **same event**
    

---

### Emitting an Event

```js
myEmitter.emit("greet", "Tyson"); 
// Output: Hello, Tyson!
```

- `.emit(eventName, ...args)` triggers all listeners for that event
    

---

## 3️⃣ One-Time Event Listeners

- `.once` → listener fires **only once**, then removed automatically
    

```js
myEmitter.once("welcome", (user) => {
  console.log(`Welcome, ${user}`);
});

myEmitter.emit("welcome", "Tyson"); // fires
myEmitter.emit("welcome", "Alice"); // ignored
```

---

## 4️⃣ Removing Listeners

```js
const listener = (msg) => console.log(msg);

myEmitter.on("message", listener);
myEmitter.removeListener("message", listener);
```

- Prevents memory leaks in long-running apps
    

---

## 5️⃣ Practical Example: Event-Driven HTTP Server

```js
const EventEmitter = require("events");
const http = require("http");

const serverEvents = new EventEmitter();

serverEvents.on("requestReceived", (url) => {
  console.log(`Incoming request for ${url}`);
});

const server = http.createServer((req, res) => {
  serverEvents.emit("requestReceived", req.url);
  res.end("Hello, world!");
});

server.listen(3000, () => console.log("Server running on port 3000"));
```

- The HTTP server emits **custom events**
    
- You can **handle logging, metrics, or analytics** without cluttering the main request handler
    

---

## 6️⃣ EventEmitter with Streams

- Node streams are **built on EventEmitter**
    
- Examples:
    
    - `readable.on("data", chunk => ...)`
        
    - `writable.on("finish", () => ...)`
        

---

## 7️⃣ Key Takeaways

1. Node.js is **event-driven at its core**
    
2. `EventEmitter` allows **decoupling logic** and **asynchronous handling**
    
3. `.on` = register listeners, `.emit` = trigger events
    
4. `.once` = one-time listener
    
5. Removing listeners prevents **memory leaks**
    
6. Event-driven design is widely used in **HTTP servers, streams, WebSockets, and microservices**
    

---

Next, we can go through **child processes & clustering in Node.js**, which is about **scaling Node apps beyond a single thread**.

Do you want to go there next?