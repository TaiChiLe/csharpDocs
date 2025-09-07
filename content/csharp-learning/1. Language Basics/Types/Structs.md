
# 📌 1. What is a Struct?

- A **struct** is a **value type** in C#.
    
- Unlike **classes**, which are reference types (live on the heap), **structs are usually stored on the stack** and are copied by value.
    
- Structs are typically used for **small, lightweight objects** that represent a single value or a group of related values.
    

---

# 📌 2. Syntax Example

```csharp
public struct Point
{
    public int X;
    public int Y;

    // Constructor
    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    // Method
    public void Display()
    {
        Console.WriteLine($"Point({X}, {Y})");
    }
}

class Program
{
    static void Main()
    {
        Point p1 = new Point(3, 4);
        p1.Display();

        // Copying by value
        Point p2 = p1;
        p2.X = 10;
        p1.Display(); // still (3,4)
        p2.Display(); // (10,4)
    }
}
```

**Output:**

```
Point(3, 4)
Point(3, 4)
Point(10, 4)
```

- Notice how **changing `p2` doesn’t affect `p1`** — because structs are **copied by value**.
    

---

# 📌 3. Key Differences Between Structs and Classes

|Feature|Struct|Class|
|---|---|---|
|Type|Value type|Reference type|
|Memory|Stack (usually)|Heap|
|Copying|By value (copy)|By reference (pointer)|
|Inheritance|Cannot inherit from another struct|Can inherit|
|Default Constructor|Automatically provided|Can define explicitly|
|Ideal for|Small, immutable data|Large, complex objects|

---

# 📌 4. When to Use Structs

- Represent **small, simple data structures**: points, coordinates, RGB colors, etc.
    
- Performance-critical code where **heap allocation is expensive**.
    
- When you want **value semantics** (copy by value instead of reference).
    

---

# 📌 5. Notes

- Structs **can have methods, properties, and constructors** (but no parameterless constructor in older versions of C#).
    
- Avoid large structs — copying big structs can hurt performance.
    
- Structs **cannot inherit** from other structs or classes (but they can implement interfaces).
    

---

✅ **In short:**  
Structs are **value types**, lightweight alternatives to classes, and are copied by value rather than by reference. They are part of **C#’s type system**, just like classes, enums, tuples, and so on.
