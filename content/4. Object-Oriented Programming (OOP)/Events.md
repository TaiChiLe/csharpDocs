Let’s go through **events in C#** — a key concept for **communication between objects**.

---

# 📌 1. **What is an Event?**

- An **event** is a way for a class (the **publisher**) to **notify other classes (subscribers)** that something happened.
    
- Events are based on **[[Delegates]]**, which define the **signature of the method** that handles the event.
    
- Think of it as a **“signal” that something occurred**, and any interested object can **react to it**.
    

---

# 📌 2. **Basic Anatomy of an Event**

1. **Delegate** → defines the **method signature** for event handlers
    
2. **Event** → defines the **notification mechanism**
    
3. **Subscriber** → a method that runs when the event is raised
    

---

### **Example: Simple Event**

```csharp
using System;

public class Alarm
{
    // Step 1: Define delegate
    public delegate void AlarmEventHandler(string message);

    // Step 2: Define event
    public event AlarmEventHandler AlarmRaised;

    public void TriggerAlarm()
    {
        // Step 3: Raise the event
        AlarmRaised?.Invoke("Alarm triggered!");
    }
}

public class Program
{
    static void Main()
    {
        Alarm alarm = new Alarm();

        // Step 4: Subscribe to the event
        alarm.AlarmRaised += HandleAlarm;

        alarm.TriggerAlarm();
    }

    static void HandleAlarm(string msg)
    {
        Console.WriteLine($"Event received: {msg}");
    }
}
```

**Output:**

```
Event received: Alarm triggered!
```

- `AlarmRaised?.Invoke(...)` → raises the event (safely checking for subscribers)
    
- `alarm.AlarmRaised += HandleAlarm` → subscribes a method to the event
    

---

# 📌 3. **Key Points**

|Concept|Explanation|
|---|---|
|Delegate|Defines the **signature** of the event handler|
|Event|Publisher sends notifications|
|Subscriber|Object/method that reacts to the event|
|Raising event|Uses `Invoke()` (or shorthand `?.Invoke`) to notify subscribers|
|Syntax sugar|Can use `EventHandler` delegate for standard events: `public event EventHandler MyEvent;`|

---

# 📌 4. **EventHandler Shortcut**

- Standardized delegate in .NET:
    

```csharp
public event EventHandler SomethingHappened;

protected virtual void OnSomethingHappened()
{
    SomethingHappened?.Invoke(this, EventArgs.Empty);
}
```

- `sender` → the object that raised the event
    
- `EventArgs` → additional event information (can use a derived class for custom data)
    

---

# 📌 5. **Analogy**

- **Publisher** → alarm clock
    
- **Event** → alarm bell ringing
    
- **Subscriber** → person who hears the bell and reacts
    

---

✅ **Tip:**

- Events in C# **decouple classes** — the publisher doesn’t need to know about subscribers.
    
- Common in **UI programming, async notifications, or reactive programming**.
    

---
