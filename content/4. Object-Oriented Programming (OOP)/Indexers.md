Let’s go through **indexers in C#** — a feature that makes objects **behave like arrays**.

---

# 📌 1. **What is an Indexer?**

- An **indexer** allows an object of a class or struct to be **accessed using array-like syntax** (`obj[index]`).
    
- Essentially, it **defines how the object responds to the `[]` operator**.
    
- Useful when your class **wraps a collection** or you want **custom access logic**.
    

---

# 📌 2. **Syntax of an Indexer**

```csharp
public class SampleCollection
{
    private string[] data = new string[5];

    // Indexer
    public string this[int index]
    {
        get { return data[index]; }
        set { data[index] = value; }
    }
}
```

- `this[int index]` → defines the indexer
    
- `get` → called when accessing `obj[index]`
    
- `set` → called when assigning `obj[index] = value`
    

---

# 📌 3. **Example Usage**

```csharp
SampleCollection collection = new SampleCollection();

// Using indexer like an array
collection[0] = "Hello";
collection[1] = "World";

Console.WriteLine(collection[0]); // Hello
Console.WriteLine(collection[1]); // World
```

- Notice how `collection[0]` behaves like a normal array, but **the logic is defined in your class**.
    

---

# 📌 4. **Key Points**

1. **Indexers can be overloaded** with different parameter types (int, string, etc.)
    
2. **No name** is needed for an indexer; it’s always `this[...]`
    
3. **Can be read-write, read-only, or write-only** depending on which accessors you implement
    

### **Example: String-based Indexer**

```csharp
public string this[string key]
{
    get { return data[Array.IndexOf(data, key)]; }
}
```

---

# 📌 5. **Analogy**

- Think of an **indexer** as a **custom array**:
    
    - Your object acts like an array
        
    - You can control **how data is stored or retrieved**
        

---

# 📌 6. **Benefits**

- Encapsulates **internal data structures**
    
- Provides **array-like access** to objects without exposing the underlying collection
    
- Supports **custom logic when getting or setting values**
    

---

✅ **Tip:**

- Use indexers when your class **represents a collection** or **needs array-like access with custom logic**.
    
- They are often used in **wrappers around dictionaries, lists, or custom data structures**.
    

---
