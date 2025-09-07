Let’s go through **Clustering & Horizontal Scaling in Node.js**, which is essential for **leveraging multi-core CPUs** and **scaling Node apps** beyond a single thread.

---

## 1️⃣ Why Clustering?

- Node.js is **single-threaded** by default → only one CPU core handles the event loop.
    
- On multi-core machines, a single Node process **cannot fully utilize all cores**.
    
- **Clustering** allows spawning **multiple Node processes** (workers) to handle more load.
    

---

## 2️⃣ Node.js Cluster Module

- Core module: `cluster`
    
- Spawns **worker processes** that share the same server port.
    

### Basic Example

```js
const cluster = require("cluster");
const http = require("http");
const numCPUs = require("os").cpus().length;

if (cluster.isMaster) {
  console.log(`Master ${process.pid} is running`);

  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on("exit", (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died. Restarting...`);
    cluster.fork();
  });
} else {
  // Workers share the same port
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end(`Handled by worker ${process.pid}\n`);
  }).listen(3000);

  console.log(`Worker ${process.pid} started`);
}
```

- `cluster.isMaster` → true for master process
    
- `cluster.fork()` → spawn a worker
    
- Workers **share server port** automatically
    

---

## 3️⃣ Benefits of Clustering

1. **Utilize all CPU cores** → higher throughput
    
2. **Fault tolerance** → master can respawn dead workers
    
3. **Independent memory** → each worker has its own memory heap
    

---

## 4️⃣ Horizontal Scaling

- **Clustering** is **vertical scaling** on a single machine.
    
- **Horizontal scaling** → multiple machines or containers behind a **load balancer**.
    

### Example Architecture

```
          +------------+
Client -->| Load Balancer |--> Node.js Cluster (Server 1)
          +------------+          |
                                 Node.js Cluster (Server 2)
```

- Use **NGINX, HAProxy, or cloud LB**
    
- Each server can run **multiple clusters** → fully utilize cores
    

---

## 5️⃣ Key Considerations

1. **Sticky sessions**
    
    - Needed for WebSocket connections → ensure the same client hits the same worker.
        
    - Use **`sticky-session` modules** or cloud LB with session affinity.
        
2. **Shared state**
    
    - Workers don’t share memory → use **Redis, databases, or message queues** for shared state.
        
3. **Logging & monitoring**
    
    - Multiple processes → aggregate logs with **Winston, Bunyan, or ELK stack**
        
4. **Process management**
    
    - Use **PM2** for clustering, monitoring, auto-restart, and zero-downtime reloads
        

---

## 6️⃣ PM2 Example (Simpler Clustering)

```bash
npm install pm2 -g
pm2 start app.js -i max   # runs one process per CPU core
pm2 monit                 # monitor CPU, memory usage
pm2 reload all            # zero-downtime reload
```

- `-i max` → automatically runs as many processes as CPU cores
    
- PM2 handles **restart on failure**, **log aggregation**, and **graceful reloads**
    

---

## 7️⃣ Key Takeaways

- Node.js is single-threaded → use **cluster** for multi-core CPU utilization
    
- **Clustering** = vertical scaling; **multiple machines** = horizontal scaling
    
- Workers are **independent processes** → shared state must be externalized
    
- PM2 simplifies **process management, scaling, and monitoring**
    

---
