Let’s go over **delegates in C#** — a fundamental concept for **type-safe function references**.

---

# 📌 1. **What is a Delegate?**

- A **delegate** is a **type that represents references to methods** with a **specific signature**.
    
- In other words, it’s like a **pointer to a function** that is **type-safe**.
    
- Delegates allow methods to be **passed as parameters** or **assigned to variables**.
    

---

# 📌 2. **Why Use Delegates?**

- **Callback methods** → pass a method to another method to be called later
    
- **Event handling** → events in C# use delegates internally
    
- **Decoupling** → allows objects to interact without knowing each other directly
    

---

# 📌 3. **Basic Syntax**

```csharp
// Step 1: Declare a delegate
public delegate void MyDelegate(string message);

// Step 2: Define methods matching the delegate signature
public class Program
{
    public static void ShowMessage(string msg)
    {
        Console.WriteLine(msg);
    }

    static void Main()
    {
        // Step 3: Instantiate delegate and assign method
        MyDelegate del = ShowMessage;

        // Step 4: Call method via delegate
        del("Hello from delegate!");
    }
}
```

**Output:**

```
Hello from delegate!
```

- `MyDelegate` can reference any method with **`void` return type and a single string parameter`**.
    

---

# 📌 4. **Multicast Delegates**

- A delegate can reference **multiple methods**.
    
- Use `+` or `+=` to combine, `-=` to remove.
    

```csharp
public static void AnotherMessage(string msg)
{
    Console.WriteLine("Another: " + msg);
}

MyDelegate del = ShowMessage;
del += AnotherMessage;

del("Hello again!");
```

Output:

```
Hello again!
Another: Hello again!
```

- Both methods are called in order.
    

---

# 📌 5. **Built-in Delegates**

1. **Action** → method with no return value
    
    ```csharp
    Action<string> action = ShowMessage;
    action("Hi!");
    ```
    
2. **Func** → method that returns a value
    
    ```csharp
    Func<int, int, int> add = (x, y) => x + y;
    Console.WriteLine(add(3,4)); // 7
    ```
    
3. **Predicate** → method returning a `bool`
    
    ```csharp
    Predicate<int> isEven = x => x % 2 == 0;
    Console.WriteLine(isEven(4)); // True
    ```
    

---

# 📌 6. **Difference Between Delegate and Event**

- **Delegate** → can be invoked anywhere you have access to it
    
- **Event** → can only be invoked **inside the class** that declared it
    

---

# 📌 7. **Analogy**

- Delegate → **like a remote control**: you assign a method (device) to the delegate (remote), then pressing the remote calls the device.
    

---

✅ **Tip:**

- Delegates are at the heart of **events, callbacks, and functional programming patterns** in C#.
    
- Modern C# often uses **Action, Func, lambdas**, but understanding delegates is crucial for events.
    

---

