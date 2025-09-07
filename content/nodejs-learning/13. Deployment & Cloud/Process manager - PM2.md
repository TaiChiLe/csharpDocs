Perfect — let’s go through **PM2**, a widely used **process manager for Node.js** that’s essential for **production deployments**.

---

## **1️⃣ What is PM2?**

- PM2 = **Production Process Manager** for Node.js
    
- Helps **keep applications alive**, **restart on failure**, and **monitor performance**
    
- Works for **single apps, clusters, or multiple apps**
    

**Key Features:**

1. **Process management:** Start, stop, restart Node apps
    
2. **Clustering:** Run app across all CPU cores
    
3. **Monitoring:** Track CPU, memory, uptime
    
4. **Log management:** Aggregate and persist logs
    
5. **Auto-restart:** Restart apps if they crash or the server restarts
    
6. **Zero-downtime reloads:** Graceful reloads for updates
    

---

## **2️⃣ Installing PM2**

```bash
npm install pm2 -g
```

---

## **3️⃣ Starting and Managing Apps**

### Start a Node App

```bash
pm2 start app.js --name my-app
```

- `--name` → assign a friendly process name
    

### List Running Processes

```bash
pm2 list
```

### Stop / Restart App

```bash
pm2 stop my-app
pm2 restart my-app
pm2 reload my-app   # zero-downtime reload
```

### Delete App

```bash
pm2 delete my-app
```

---

## **4️⃣ Clustering Mode**

- Utilize **all CPU cores** for better throughput
    

```bash
pm2 start app.js -i max   # max = number of CPU cores
```

- PM2 automatically **spawns multiple worker processes**
    

---

## **5️⃣ Monitoring & Logs**

### Real-time Monitoring

```bash
pm2 monit
```

### Logs

```bash
pm2 logs my-app       # live logs
pm2 flush             # clear logs
pm2 logs --lines 100  # last 100 lines
```

---

## **6️⃣ Auto Startup on Server Reboot**

```bash
pm2 startup
pm2 save
```

- Ensures Node apps **restart automatically** after server reboot
    

---

## **7️⃣ Ecosystem File (Optional)**

- Manage multiple apps with a **JSON/YAML configuration file**
    

```json
// ecosystem.config.js
module.exports = {
  apps: [
    {
      name: "my-app",
      script: "./app.js",
      instances: "max",
      exec_mode: "cluster",
      watch: true
    }
  ]
};
```

- Start all apps: `pm2 start ecosystem.config.js`
    

---

## **8️⃣ Key Takeaways**

1. **PM2** is essential for **production Node.js apps**
    
2. Provides **process management, clustering, monitoring, and logging**
    
3. Supports **zero-downtime reloads** for seamless deployments
    
4. Easily integrates with **server startup scripts**
    

---

Next, we could cover **Node.js production best practices**, combining **performance, monitoring, logging, security, clustering, and deployment strategies** for real-world applications.

Do you want me to go there next?