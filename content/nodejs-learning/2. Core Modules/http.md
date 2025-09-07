The **`http` module** is the backbone of Node.js web servers. It lets you create servers without needing external frameworks (like Express). Let’s break it down:

---

## 📂 1. Importing

```js
const http = require("http");
```

---

## ⚡ 2. Creating a Basic Server

```js
const http = require("http");

const server = http.createServer((req, res) => {
  res.statusCode = 200; // HTTP status
  res.setHeader("Content-Type", "text/plain");
  res.end("Hello, World!\n");
});

server.listen(3000, () => {
  console.log("Server running at http://localhost:3000/");
});
```

- `http.createServer()` takes a callback `(req, res)` that runs **on every request**.
    
- `req` = incoming request object.
    
- `res` = response object you use to send back data.
    

---

## 🔎 3. Request (`req`)

`req` contains info about the client request:

```js
const server = http.createServer((req, res) => {
  console.log(req.method); // GET, POST, etc.
  console.log(req.url);    // e.g. "/about"
  console.log(req.headers); // headers object

  res.end("Check the console!");
});
```

---

## 📤 4. Response (`res`)

You control what goes back to the client:

```js
const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/html" });
  res.write("<h1>Hello from Node.js</h1>");
  res.end(); // must call end() to finish
});
```

---

## 🌐 5. Routing Basics

You can create simple routes by checking `req.url`:

```js
const server = http.createServer((req, res) => {
  if (req.url === "/") {
    res.end("Home Page");
  } else if (req.url === "/about") {
    res.end("About Page");
  } else {
    res.statusCode = 404;
    res.end("Page Not Found");
  }
});
```

---

## 📦 6. JSON Response

```js
const server = http.createServer((req, res) => {
  if (req.url === "/api") {
    res.setHeader("Content-Type", "application/json");
    res.end(JSON.stringify({ message: "Hello API" }));
  } else {
    res.end("Hello World");
  }
});
```

---

## ⚡ 7. Handling POST Data (Streams)

Request bodies are streams, so you need to collect data:

```js
const server = http.createServer((req, res) => {
  if (req.method === "POST") {
    let body = "";

    req.on("data", (chunk) => {
      body += chunk.toString();
    });

    req.on("end", () => {
      res.end("Data received: " + body);
    });
  } else {
    res.end("Send a POST request");
  }
});
```

---

## ✅ In Short

- `http.createServer((req, res) => {...})` → main entry point.
    
- `req` → incoming request (method, url, headers, body).
    
- `res` → response (status, headers, body).
    
- Node servers are **low-level** → frameworks like Express build on this to make routing & middleware easier.
    

---