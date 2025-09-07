Perfect — let’s go through **error tracking in Node.js** using **Sentry** and **Rollbar**, which are essential for **detecting, reporting, and resolving runtime errors** in production applications.

---

## **1️⃣ Why Error Tracking Matters**

- Detect **uncaught exceptions** and **runtime errors**
    
- Get **stack traces, context, and user info** for debugging
    
- Alerts developers **in real time**
    
- Reduces **time to fix critical bugs**
    

---

## **2️⃣ Sentry**

- **Sentry** = popular error tracking and monitoring platform
    
- Supports **Node.js, frontend frameworks, and serverless apps**
    
- Provides **alerts, issue tracking, and breadcrumbs**
    

### a) Install

```bash
npm install @sentry/node
```

### b) Basic Setup

```js
const Sentry = require("@sentry/node");

Sentry.init({
  dsn: "https://your_dsn_here@sentry.io/project_id",
  environment: process.env.NODE_ENV || "development",
  tracesSampleRate: 1.0, // optional for performance monitoring
});

// Express integration
const express = require("express");
const app = express();

// Request handler
app.use(Sentry.Handlers.requestHandler());

// Example route
app.get("/", (req, res) => {
  throw new Error("Test error for Sentry!");
});

// Error handler
app.use(Sentry.Handlers.errorHandler());

app.listen(3000, () => console.log("Server running"));
```

- Errors automatically appear in **Sentry dashboard** with **stack traces, request info, and user context**
    

---

## **3️⃣ Rollbar**

- **Rollbar** = real-time error tracking and crash reporting
    
- Supports **Node.js, frontend, and serverless apps**
    
- Provides **alerts, telemetry, and deployments tracking**
    

### a) Install

```bash
npm install rollbar
```

### b) Basic Setup

```js
const Rollbar = require("rollbar");
const rollbar = new Rollbar({
  accessToken: "YOUR_ACCESS_TOKEN",
  captureUncaught: true,
  captureUnhandledRejections: true,
});

const express = require("express");
const app = express();

// Example route
app.get("/", (req, res) => {
  rollbar.info("Hello world route accessed");
  throw new Error("Test error for Rollbar!");
});

// Express error handler
app.use(rollbar.errorHandler());

app.listen(3000, () => console.log("Server running"));
```

- Errors appear in **Rollbar dashboard** with detailed context
    

---

## **4️⃣ Key Features Comparison**

|Feature|Sentry|Rollbar|
|---|---|---|
|Real-time alerts|✅|✅|
|Exception tracking|✅|✅|
|Performance monitoring|✅|Limited|
|Source maps support|✅|✅|
|Deployment tracking|✅|✅|
|Open-source SDK|✅|✅|

---

## **5️⃣ Best Practices**

1. **Capture uncaught exceptions and unhandled promise rejections**
    
2. **Integrate with Express or other frameworks**
    
3. **Include environment & user context**
    
4. **Combine with logs (Winston/Pino) and metrics (Prometheus)** for full observability
    
5. **Set alerts** to notify devs of critical errors immediately
    

---

## **6️⃣ Key Takeaways**

- **Sentry and Rollbar** → detect runtime errors, unhandled exceptions, and performance issues
    
- Provide **real-time alerts, stack traces, and contextual info**
    
- Essential for **production-grade Node.js applications**
    
- Best used **together with logging and monitoring** for full observability
    

---

Next, we could go through **Node.js production best practices**, combining **performance, monitoring, logging, security, clustering, and deployment strategies** for real-world applications.

Do you want to go there next?